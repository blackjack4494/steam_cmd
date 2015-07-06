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
//synchronizing sequence numbers (those are random)

Second Packet:
Server sends TCP with Ack and Syn Flag to Client
//sending ack equal to client's previous sequence number (len/amount of data does not matter right now)
//sending (syn) sequence number to client

Third Packet:
Client sends TCP with Ack Flag to Server
//none

-> 'Handshake' complete?!


Next two Packets are HTTP Packets:

First Packet:
Client to Server -> Request command "GET" /client/steam_cmd_win32 (using windows7)
  PacketDetails:
  User-Agent: Valve/Steam HTTP Client 1.0 (client;windows;10;1428529675) <--guess that last two numbers could be version and timestamp/checksum(sha!?/md5??) ::Update '10' seems to be something else; long number is version
  Host: client-download.steampowered.com
  HeaderEnd: CLRF
also sending TCP with Ack (again) and Push (interactive traffic; force sending data without buffering)
//checking if current steamcmd is newest version/updated

Second Packet:
Server to Client -> Response Status: Moved temporarily, URL: /client/steam_cmd_win32 
  StatusCode: 302, Moved temporarily
  Location:  http://media2.steampowered.com/client/steam_cmd_win32?1428537885 <--comparing/checking number (first packet)
//none (guess some servers are too heavy loaded/used)



After this steamcmd is using another server.
*Domain is always the same. Just ip changes ('valve network') 
Same way handshake like first three tcps.
Then same first HTTP Packet but this time with Query
  Http: Request, GET /client/steam_cmd_win32, Query:1428537885 (look above same number)
  
Then server sends client TCP packet with ack (to accept request?)

Server Response
  Http: Response, HTTP/1.1, Status: Ok, URL: /client/steam_cmd_win32
  ETag:  "1428537885" (so this number is called ETag)
  ContentType:  application/octet-stream (means unknown file format)
  X-HW:  1434985944.dop007.fr7.t,1434985944.cds028.fr7.c (dunno what this should be; tho length is same as Etag but do not guess there is a relation
  
  


  
TODO:
-check out ETag and X-HW a couple of times. tho it belongs to steamcmd.exe
-emulate handshake in own program (look at first three tcps)
  -> ACK-Layer (and SYN)
-user-agent for now have to be updated manually if there will be errors with connection/auth
-more..

useful Resources:
https://support.microsoft.com/en-us/kb/169292







Side goal:
can be connected with fritz.box repo
e.g. auto-forward ports and close/delete them if server is not running.





















