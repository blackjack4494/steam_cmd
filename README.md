# steam_cmd
SteamCMD

This: https://developer.valvesoftware.com/wiki/SteamCMD
Is using SteamPipe: https://developer.valvesoftware.com/wiki/SteamPipe

SteamPipe is using HTTP as content delivery.


Research so far:

TCP Ports: 60986,60987
HTTP: 80


First three Packets are TCP Packets (handshake behavior?):

First Packet:
Client sends TCP with Syn Flag to Server 
>synchronizing sequence numbers (those are random)

Second Packet:
Server sends TCP with Ack and Syn Flag to Client
>sending ack equal to client's previous sequence number (len/amount of data does not matter right now)
>sending (syn) sequence number to client

Third Packet:
Client sends TCP with Ack Flag to Server
>none

-> 'Handshake' complete?!


Next two Packets are HTTP Packets:

First Packet:
Client to Server -> Request command "GET" /client/steam_cmd_win32 (using windows7)
  PacketDetails:
  User-Agent: Valve/Steam HTTP Client 1.0 (client;windows;10;1428529675) <--guess that last two numbers could be version and timestamp/checksum(sha!?/md5??)
  Host: client-download.steampowered.com
  HeaderEnd: CLRF
also sending TCP with Ack (again) and Push (interactive traffic; force sending data without buffering)
>checking if current steamcmd is newest version/updated

Second Packet:
Server to Client -> Response Status: Moved temporarily, URL: /client/steam_cmd_win32 
  StatusCode: 302, Moved temporarily
  Location:  http://media2.steampowered.com/client/steam_cmd_win32?1428537885 <--comparing/checking number (first packet)
>none (guess some servers are too heavy loaded/used)



After this steamcmd is using another server.
Same way handshake like first three tcps.







