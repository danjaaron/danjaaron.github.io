March 20, 2021

1. Find the address of the value

2. Find what accesses the above address and record the instruction

e.g.

	0042814E - 89 46 18  - mov [esi+18],eax

3. Record the base address from above

e.g.

	0042814E - 89 46 18  - mov [esi+18],eax
	ESI=018198D0

4. Search for this address using an exact hex scan and record the result

e.g.

	0042814E - 89 46 18  - mov [esi+18],eax
	ESI=018198D0 --search--> 017A8BA8

5. Repeat steps 4 and 5 until no more results come up

e.g

	0042814E - 89 46 18  - mov [esi+18],eax
	ESI=018198D0 --search--> 017A8BA8
	017A8BA8 --search--> 01800EAC
	01800EAC --search--> None
	NEW ADDRESS = 01800EAC  


6. Repeat steps 2-6 until you find a static pointer

e.g. 

	"Tutorial-i386.exe"+2426E0
	*** static pointer, we've reached the bottom

7. Assemble a pointer from the static address to the initial value (from step 1) manually.

7.a. Click "Add Address Manually", check "Pointer", and input the static address ("Tutorial-i386.exe"+2426E0) as the address.

7.b. Collect the offset from each of the previous steps from the instructions that were recorded.

	e.g. 

	0042814E - 89 46 18  - mov [esi+18],eax
	ESI=018198D0 --search--> 017A8BA8
	017A8BA8 --search--> 01800EAC
	01800EAC --search--> None
	NEW ADDRESS = 01800EAC

	=> OFFSET=18

7.c. Add the offsets to the pointer dialog box in the order that they were recorded.

	e.g. 

	My instructions were:

		0042814E - 89 46 18  - mov [esi+18],eax
		004280C0 - 83 7E 14 00 - cmp dword ptr [esi+14],00
		0042807C - 83 7E 0C 00 - cmp dword ptr [esi+0C],00
		
	and, from top to bottom, my pointer dialog box reads the offsets like this:

			18
			0
			14
			0C
			"Tutorial-i386.exe"+2426E0

8. The pointer you've made might not point to the correct value at first.

Look at the addresses in blue which appear to the right of each offset that you input. 

Move around the offsets and zeros so that those addresses match the addresses you've recorded.

To the right of each offset you've recorded, you should see the corresponding address you recorded. 

Once these are all correct, the pointer you've made will point to the initial value you were attempting to capture.


<iframe width="560" height="315" src="https://www.youtube.com/embed/JMmaCMe9rEc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
