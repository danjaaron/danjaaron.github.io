## Active Directory 

<details>
<summary>
0. Prologue 
</summary>

The imagination is your most powerful asset. 

Sometimes when I want to learn a skill, but I find the content is too dry, I try to illuminate the content with my imagination.

Active directory can be a very dry topic. See for yourself, straight from Microsoft's official documentation ([src](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)):

<blockquote>
A directory is a hierarchical structure that stores information about objects on the network.

A directory service, such as Active Directory Domain Services (AD DS), provides the methods for storing directory data and making this data available to network users and administrators.

For example, AD DS stores information about user accounts, such as names, passwords, phone numbers, and so on, and enables other authorized users on the same network to access this information.
</blockquote>

Not exactly exciting stuff for a newbie. 

So, I invented a trick to prepare myself to tackle AD:   

	You see a city on a hill, surrounded by tall gates. What do you do?

I imagined that I was playing a text adventure game, and that I was trying to sneak into a kingdom.

First, I would need to LOOK (equivalent to scanning with nmap).

I see a public marketplace, HTTP, where I can interact with the kingdom. Maybe if I poke around I'll find a secret entrance into the castle. 

I see a locked back door into the castle dungeons, SSH. With the right key, I'll be inside the castle.

But...what's this? Port 389 / Microsoft Windows Active Directory LDAP? 

This is like a window straight into the throne room. If I peek through this window, I may be able to find some pretty important information. Who knows? If I know enough about the royal family, maybe I can trick the castle guards into letting me in...

</details>

<details>
<summary>
1. Reconnaissance  
</summary>

A standard nmap scan is always a good starting place: 

	nmap -sC -sV -sT -Pn -n [TARGET_IP] -vv -oN [LOG_FILE]

The ```-sC``` flag is extremely important when your target is an Active Directory Domain Controller (AD DC), because this flag triggers nmap to use scripts which enumerate the domain through LDAP running on port 389.

Most throne rooms have windows, and most AD DCs run LDAP on port 389.

	You peer into the throne room, and spy the royal seal. Welcome to the kingdom of [DOMAIN NAME].

So, now you know the Domain Name (DN). This is like the name of the kingdom that you're trying to infiltrate. Maybe it's Briton, Wales, or Asgard. You're definitely going to need to know the name of the kingdom if you're going to be tricking its guards into letting you in. In our case, nmap returns the following output:


	3268/tcp open  ldap          syn-ack Microsoft Windows Active Directory LDAP (Domain: DANJA-DC.LOCAL0., Site: Default-First-Site-Name)

Welcome to the kingdom of DANJA-DC.LOCAL! 

<blockquote>
Note that domain names can end in any top-level domain (TLD) like .com, or .org. By convention, DNs often end with a ".local" TLD. This is a fictional TLD which is used for local domains that are not typically accessible over the internet, unlike domains that end in .com or .org. It's sort of like how some IP addresses are private, starting with "192" or "127" or "10", but for a domain name ending with ".local" instead.
</blockquote>

Great, we know the domain name. We need the DN so that we can enumerate users on the domain. These users are like the inhabitants of the castle. Maybe we can see whether we can impersonate one of them to slip inside. 
</details>


<details>
<summary>
2. Exploitation 
</summary>
</details>


<details>
<summary>
3. Privilege Escalation 
</summary>
</details>
