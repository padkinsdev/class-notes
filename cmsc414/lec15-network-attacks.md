# Network Attacks

## Introduction
- IP routes by subnets, which are groups of addresses with a common prefix
- The internet is a network of networks, comprised of many autonomous systems (AS). Each AS is composed of one or more local area networks (LANs)
- The protocol for communicating between autonomous systems is the Border Gateway Protocol (BGP), in which each router announces what networks it can provide, as well as what the pathway onward from the router is
- Each AS implicitly trusts its surroundings ASes and accepts advertised routes
    - This means that a malicious AS can lie and claim to be responsible for a network which it isn't. This is called BGP hijacking
    - Mitigating BGP hijacking relies on defenses from higher levels
- IP spoofing: Malicious clients can send IP packets with source IP values set to a spoofed value

## Transmission control protocol (TCP)
- IP is unreliable and only provides a best effort delivery service, which means that apckets can be lost, corrupted, or delivered out of order
- IP packets also have a limited size
- TCP breaks up messages to fit the size of an IP packet, then reassembles them at destination
- When sending packets, TCP labels each byte of the message with increasing numbers, thus facilitating in-order reassembly at the destination
- Before starting a TCP connection, the client and server establish two sets of sequence numbers, one for server-to-client communication and one for client-to-server communications
- Steps:
    - Client chooses an initial sequence number x for its bytes and sends a SYN (synchronize) packet to the server
    - Server chooses an initial sequence number y for its bytes and responds with a SYN-ACK packet
    - Client responds with an ACK, once both devices have synchronized sequence numbers, the connection is established
- TCP handlers on both ends track which TCP segments have been received for each connection
- Data from the bytestream (which is how data in TCP is presented) can be presented to the application when all data before has been received and presented
- Byte i of the bytestream is represented by sequence number x+i. The first byte of data is i=1 because sequence number x was used for the SYN packet and y for the SYN-ACK packet
- If a packet is dropped, the recipient will not reply with an ACK and thus the sender will know that the packet was not received. If the ACK is dropped instead then the sender will resend the data and the recipient will ignore it and resend the ACK
- A packet’s sequence number is the number of the first byte of its data
- A packet’s ACK number, if the ACK flag is set, is the number of the byte immediately after the last received byte
- When packets are dropped in TCP, TCP assumes that there is congestion and begins to send packets at a slower rate
- To end a conncetion, one side sends a packet with the FIN (finish) flag set, which should be acknowledged. This flag indicates that packets will no longer be sent but may still be received
- To abort the connection, one side sends a packet with the RST (reset) flag set, which means that they will no longer be sending or receiving data
- TCP flags:
    - ACK: Acknowledged
    - SYN: Indicates the beginning of a connection
    - FIN: Indicates that one will only receive (not send) packets
    - RST: Indicates that one will no longer send or receive packets

### TCP attacks
- TCP hijacking: Tampering with an existing session to modify or inject data into a connection
- Data injection: Spoofing packets to inject malicious data into a connection. 
    -This requires the attacker to know the sender's sequence number, which is easy for man-in-the-middle and on-path attackers, but off-path attackers must guess the 32 bit sequence (called blind injection/hijacking)
    - For on-path attackers this is a race to be faster than the legitimate response. If successful, the receiver will ignore the legitimate packet as a duplicate
- TCP spoofing: Spoofing a TCP connection to appear to come from another source IP address
    - IP packets can be spoofed if the AS doesn't block source addresses
    - TCP spoofing requires the sequence number in the server's response SYN-ACK packet
    - This is easy for MITM and on-path attackers but off-path attackers must blindly spoof
    - For on-path attackers, this is a race condition, since the real client will send a RST upon receiving the server’s SYN-ACK
- RST injection: Spoofing an RST packet to forcibly terminate a connection
    - Requires knowledge of the sender's sequence number
    - Easy of MITM and on-path attackers, but hard for off-path attackers
    - Often used for censorship
- SYN flooding: Sending a large volume of SYN packets with spoofed IP addresses to chew through server resources, as the server will respond to each connection request
    - While the server waits for the final ACK packet (which never arrives), the attacker continues to send new SYN packets, ideally until all open ports on the server are used up and the server can no longer function

## TCP conclusion
- TCP itself provides no confidentiality or integrity, so defenses rely on higher layers to prevent attacks
- Defense against off-path attacks involves randomly choosing sequence numbers, as bad randomness can make guessing sequence numbers easy

## User Datagram Protocol (UDP)
- Provides a datagram abstraction with no reliability or ordering guarantees
- Much faster than TCP due to the lack of confirmation and ordering constraints
- It is very easy to inject data into a UDP connection given the lack of sequence numbers