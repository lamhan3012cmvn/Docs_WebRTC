# Docs_WebRTC

## Introduction WebRTC

  ### What is the WebRTC?
    - WebRTC abbreviated Real Time Communication.
    - Basically WebRTC allows web application to establish peer-to-peer communication.
  ### Table of Content
  #### 1. Communication "Peer to Peer "
  #### 2. Firewall and Nat Traversal
  #### 3. Signaling, Session, Protocol
  #### 4. API of WebRTC
  #### 5. WebRTC in Production
  #### 6. Find Connected Candidates

## 1. Communication Peer to Peer
  - To be able to communicate with each other through a web browser, each user's browser must perform the following steps
    + Agree to start communicating
    + Know how to locate an object
    + Overcoming security and thought of fire protection <! important>
    + Real-time transfer of all multimedia communications
  - One of the biggest challenges related to browser-based P2P communications is how to locate & establish a network socket connection with another browser to transport data communication

## 2. Firewall and Nat Traversal
  - This computer does not have a public IP address. The reason is that your computer has to browse behind the firewall and NAT the device (Network access translation **Router**).
  - The NAT device will translate the **private IP address** from within **the firewall** into a **public IP address** (public-facing IP).
  - We need NAT devices to secure and address IPv4's limitation of publicly available IP addresses.
  - Because of the involvement of NAT devices, your browser needs to find the **IP address** of the computer with which you want to communicate.
  - At this point, we have to rely on STUN (**S**ession **T**raversal **U**tilities for **N**AT ) and TURN (**T**raversal **U**sing **R**elays around **N**AT). 
In order for WebRTC technologies to work, a request for your public IP address will first be sent to the STUN server.
  - Assuming the process is up and running and you get your public IP address and port number, you should be able to tell other peers how to connect to you directly. These peers can also do the same thing, use STUN & server TURN and tell you which address to contact.
## 3. Signaling, Session, Protocol
  - WebRTC, which signals the part based on the JSEP (Javascript Session Establishment Protocol) standard.
  - For the connection to work, the peer obtains local media conditions for the metadata (e.g., codec size and type capabilities) and gathers possible network addresses for the application's host. The signaling mechanism used to forward/backward this important information is not defined in the WebRTC API.
  - Signaling is not regulated by the WebRTC standard and it is not implemented using its API to allow dynamic use of the required technologies and protocols. WebRTC developers will deal with signaling and the server handles signaling
  - Assuming your browser-based WebRTC app is able to determine its public IP address using STUN as mentioned above, the next step is actually a negotiation & establishment of an incoming session peer.
  - The session initiation and negotiation part occurs when using a signaling/communication protocol specified in multimedia communications. This protocol is also responsible for administering the rules in which sessions are managed and aborted.
  - One of such protocols is called Session Initiation Protocol (SIP). Note that due to the flexibility of WebRTC signaling, SIP is not the only signaling protocol that can be used. The signaling protocol selected must work with an application layer protocol called Session Description Protocol (SDP), which is used in the case of WebRTC. All media-specific metadata is transmitted using this SDP protocol.

<img src="https://cdn-images-1.medium.com/max/1000/0*SXRTlnVxy2-hE9ZX" />

## 4. Api of WebRTC
  - Media Capture and Streams 
  - RTCPeerConnection
<img src="https://cdn-images-1.medium.com/max/1000/0*Nm9r_NLcAhJernmo">
  - RTCDataChannel

## 5. WebRTC in Production
  - In Production WebRTC requires a server, but the reality is simpler:
    - Users discover their own partners and exchange details such as names.
    - Client-side WebRTC applications (peers) exchange network information.
    - Các peer trao đổi dữ liệu về media chẳng hạn như định dạng hình ảnh và độ phân giải.
    - Client-side WebRTC applications move through NAT ports and firewalls.
  - In Production, WebRTC should have 4 features on the server side:
    - Users discover and communicate
    - Signaling
    - Move NAT/Firewall
    - Forwarding servers in case of peer-to-peer communication failure
  - The STUN protocol and its extension TURN are used by ICE to enable RTCPeerConnection to deal with NAT migrations and other ambiguous network changes.
  - ICE is a protocol for connecting peers, such as two video chat clients. Upon initialization, ICE will attempt to connect directly to peers with the lowest possible latency via UDP. In this process, the STUN servers have a single task: enable a peer behind the NAT to find the public address & port number.
    - Link Stun: http://code.google.com/p/natvpn/source/browse/trunk/stun_server_list
  <img src="https://cdn-images-1.medium.com/max/1000/1*ONNxJHqmMTXB1Nuq3qTNXQ.png">
## 6. Find Connected Candidates
  - If UDP fails, ICE tries TCP: first HTTP, then HTTPS. If the direct connection fails - specifically because of NAT migration & enterprise level firewalls - ICE will use an intermediate TURN server (forwarding point). In other words, ICE will first use STUN with UDP to connect peers directly, if that fails, it will change its plan to use TURN relay server. The term "candidate search" refers to the process of searching for network interfaces & ports
  <img src="https://cdn-images-1.medium.com/max/1000/1*0REL14sYPR34hY7yua6-PA.png">

## 7. Security
  - There are many ways that a real-time communication application or plugin can be compromised in terms of security.
    - Ex: 
      - Unencrypted data or media may be intercepted in transit between browsers or between browser and server.
      - An application that can record and distribute audio and video without the user knowing it.
      - Malware or computer virus can be installed with a stupid application or plugin.
  - WebRTC has many features to avoid the above problems:
    - WebRTC deployment using security protocols such as DTLS and SRTP.
    - Encryption is essential for all WebRTC components, including signaling mechanisms.
    - WebRTC is not a plugin: its components run in the browser sandbox and do not run in their own thread, the components do not require individual installation and are updated every time the browser is updated.
    - Access to the camera and microphone must be explicitly granted and when the camera or microphone is active it must be shown in the user interface.

  ==> **WebRTC is an extremely interesting & powerful technology for products that need to work with real-time transfer model between browsers**