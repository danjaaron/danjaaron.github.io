## How to find pointers and offsets for a value using Cheat Engine

1. Find the address of the value

2. Find what accesses that address

3. Look at "more information" for any of those items, so long as they show pointer dereference with brackets ("[__]")

4. Record the pointer base address and offset directly from the debugger

5. Search for the pointer base address in hex

6. Record the bottom result address from the above search

7. Manually add a pointer using the result address (from #6) and base offset (from #4)

<iframe width="560" height="315" src="https://www.youtube.com/embed/6tKnCXT866M" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
