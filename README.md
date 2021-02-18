# WebRTCGame
A template for making real time online games with web browsers as clients and a pion backend as a server.

The biggest challenge with making real time online browser games (AKA .io games) is that there is no way to send an arbitray UDP packet to a browser client.
WebRTC solves this problem by allowing you to make a secure connection between browser clients and a [pion](https://pion.ly/) client and then send packets acting exactly like UDP between them.
Unordered, unreliable SCTP packets will behave exactly like vanilla UDP, but with encryption. In this example, we will be using an unreliable, **ordered** SCTP packets. We will include the ordered tag in the datachannel because any packets received older than the last packet received will be dropped, allowing every client the ability to assume every message received is the most recent update from the server.

# How to Use This Code
This code is structured with main.go being the web server/ webrtc client and index.html in the public folder being the web client.
You will need to add your public IP or domain name of the location you're hosting the web server (main.go) at to the `New Websocket` line in index.html.
Next you can add any game code you'd like to index.html, or a seperate .js file.

Send no-retransmit, but ordered, game state updates across the network to browser clients using the `dataChannel.SendText` or `dataChannel.Send` methods in main.go.

To receive client actions I chose to create another datachannel with ordered messages and retransmits enabled to be able to receive reliable messages from clients.
(No one wants to have guess if pressing right on their controller will actually make their character move right :P)
*Right now the reliable channel is under `reliableChannel` in main.go and `TCPChan` in index.html, imma change that tommorrow to be the same across the board

# Librarys Used
Thank you to:
- [Pion](https://pion.ly/)
  For the wonderful WebRTC library in Go!
- [Gorilla/Websocket](https://github.com/gorilla/websocket)
  For the wonderful websockets library in Go!

