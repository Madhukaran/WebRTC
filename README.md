# WebRTC
Trying out the New WebRTC p2p networks.Learning What is WebRTC and How well we can Use that.

# Checklists
- [x] Check there is a connection stable on the iceconnction State
- [x] Create Datachannel,AddTracks or Do whatever you Spcifically want to share on the RTCPeerConnection() before Creating #Offer or #Answer
- [x] Creating Offer of the Answer Will be the Final Stage (i.e,) Before Exchanging SDP
- [x] for the Video Processing Use Offer-Pranswer-Answer Mechanism 
- [x] IceTricleCandidate shold be True & connectionState should be connected

# Server
```js
const localConnection = new RTCPeerConnection()
 

localConnection.onicecandidate = e =>  {
console.log(" NEW ice candidnat!! on localconnection reprinting SDP " )
 console.log(JSON.stringify(localConnection.localDescription))
}


const sendChannel = localConnection.createDataChannel("sendChannel");
 sendChannel.onmessage =e =>  console.log("messsage received!!!"  + e.data )
   sendChannel.onopen = e => console.log("open!!!!");
     sendChannel.onclose =e => console.log("closed!!!!!!");


localConnection.createOffer().then(o => localConnection.setLocalDescription(o) )
```

# Client
```js
//set offer const offer = ...
const remoteConnection = new RTCPeerConnection()

remoteConnection.onicecandidate = e =>  {
console.log(" NEW ice candidnat!! on localconnection reprinting SDP " )
 console.log(JSON.stringify(remoteConnection.localDescription) )
}

 
remoteConnection.ondatachannel= e => {

      const receiveChannel = e.channel;
      receiveChannel.onmessage =e =>  console.log("messsage received!!!"  + e.data )
      receiveChannel.onopen = e => console.log("open!!!!");
      receiveChannel.onclose =e => console.log("closed!!!!!!");
      remoteConnection.channel = receiveChannel;

}


remoteConnection.setRemoteDescription(offer).then(a=>console.log("done"))

//create answer
await remoteConnection.createAnswer().then(a => remoteConnection.setLocalDescription(a)).then(a=>
console.log(JSON.stringify(remoteConnection.localDescription)))
//send the anser to the client
```

# Final_Server
```js
//this opens the connection

localConnection.setRemoteDescription (answer).then(a=>console.log("done"))
```
