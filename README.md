# EternalBlueC
Scanner for a machine's MS17-010 vulnerability status &amp; DoublePulsar backdoor detector

ms17_vuln_status.cpp - This program sends 4 SMB packets and reads the NT_STATUS response from a TransNamedPipeRequest ( PeekNamedPipe request ) and determines if NT_STATUS = 0xC0000205 ( STATUS_INSUFF_SERVER_RESOURCES ).  If this is the correct response, then the target is vulnerable to MS17-010.  Tested on unpatched Windows 7 x64 bit.

doublepulsar_check.cpp - This program sends 4 SMB requests and sends a Trans2 SESSION SETUP request.  Doing so, the multiplex id can be compared against value: 0x51 or 81.  If that is the response, that means DoublePulsar is present on the machine.  Afterwards, you can send commands to burn the DoublePulsar backdoor.  ( NOTE: DoublePulsar becomes dormant and not removed ).  Tested on Windows 7 x64 bit.

[NOT FINISHED]
doublepular_upload.cpp - This program sends 4 SMB requests and sends a Trans2 SESSION SETUP request to obtain the DoublePular XORKey.  Then a file then reads a DLL file (payload.dll) and combines it with userland shellcode (userland_shellcode.bin) and XORs the buffer with the DoublePulsar key.  A packet is generated by allocating memory, copying the Trans2 packet, editing the values needed for the SMB transaction to work ( UserID, ProcessID, TreeID, MultiplexID ) then copying the XORed data (shellcode + DLL) to the end and loop through it sending it at a total packet length of 4096 bytes at a time to DoublePulsar.  This is still in progress.  

TODO: Might need to implement the Trans2 Upload function using a structure and not editing the Trans2 packet capture.

Repository also contains
  * DoublePulsar Upload DLL python scripts & DoublePulsar x64 userland shellcode for DLL injection
  * EternalBlue All in one binary
  * Multi arch kernel queue apc assembly code & Windows x86/x64 Multi-Arch Kernel Ring 0 to Ring 3 via Queued APC kernel code
  * EternalBlue x64/x86 kernel shellcode from worowit
  * Eternalblue replay file
