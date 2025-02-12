---
{"dg-publish":true,"permalink":"/information-heap/radio-mesh-connection/","noteIcon":"","created":"2025-02-12T08:11:22.375-06:00"}
---

Options for getting data in to Emerson
1. Ethernet connection, through a router. Then assign a controller in developer studio. This is the classic approach.
2. For older equipment: modbus-r driver
3. Serial ports: Two-wire to box to ethernet or a card 
	- Serial flavors: RS485 vs RS232


We have a serial connection with modbus RS 485; we want to convert that to ethernet (modbus TCP)


Pete's idea:
1. Use existing serial connection to ethernet box
2. Wireless hub radio to Drop 6
3. Plug radio via ethernet to field LAN router 

Companies to provide transmitter hardware
- Xetawave
- GE 

See Pete Gabor's report Ovation Site Assessment.docx, cerca mid February 2025

[[People/Chris Luss\|Chris Luss]] has been informed and likes it.
Mike likes it.
