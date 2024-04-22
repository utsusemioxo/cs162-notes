# Address Space
![](address_space_01.svg)
## What is Address Space
- **Address space** => the set of accessible address + state associated with them:
	- For 32-bit processor there are $2^{32} = 4$ billion addresses.
## What happens when you read or write to an address?
- Perhaps acts like regular memory 如果对于地址空间一无所知，可能第一感觉是这一个
- Perhaps ignores writes
- Perhaps causes I/O operation
	- (Memory-mapped I/O)
- Perhaps causes exception (fault)
- Communicates with another program
- ...