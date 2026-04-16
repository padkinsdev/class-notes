# Low Level Network Attacks

## Introduction
- Three types of threat actors arise: man-in-the-middle, on-path, and off-path attackers
- Man-in-the-middle attackers can modify and delete as well as read packets
- On-path attackers can only read packets, and off-path attackers can neither modify, delete, or read packets

## Address Resolution Protocol (ARP)
- Translates level 3 IP addresses to level 2 MAC addresses
- ARP involves checking cache to see if an IP is known to map to a MAC address. If the address is not in cache, a request is broadcasted to the local network to learn of the correct mapping. The user with that IP address responds with their MAC address
- Ultimately, whoever responds to the broadcasted request first is trusted as the correct IP to MAC mapping
- If a malicious user responds to the ARP request before the legitimate user, then the malicious user successfully performs ARP spoofing and impersonates the desired user
- Network switches and tools like `arpwatch` help to mitigate ARP spoofing
- Switches partition the LAN into isolated VLANs (virtual local area networks)

## Dynamic Host Configuration Protocol (DHCP)
- To connect to a network the user needs and IP address, the IP address of the DNS server to look up IPs of domain names, and the IP address of the router to contact machines outside of the LAN
- DHCP gives the user a configuration the first time they connect to the network
    - **CLient Discover:** The user connects to the network and send out a request for a DHCP configuration
    - **DHCP Offer:** Any DHCP server may respond offering a configuration
    - **Client Request:** The client broadcasts which of the DHCP offers it has accepted
    - **DHCP Acknowledgement:** The chosen server acknowledges that its configuration has been accepted by the client
- The client has no way of verifying that a DHCP response is legitimate, so an attacker can impersonate a DHCP server to become a man-in-the-middle. This requires the attacker to be on the same LAN as the client
- Usually the client will accept the first DHCP offer, so DHCP spoofing simply requires a faster response than the legitimate DHCP server. This is similar to ARP spoofing
- DHCP spoofing is difficult to defend against by itself, so defenses usually involve higher levels

## WIFI
- A 2-layer protocol that wirelessly connects machines in a LAN
- Parts of a WIFI network:
    - **Access point:** Machine that helps to connect to tthe network
    - **SSID:** The name of the network
    - **Password:** Optional
- WPA2 (WIFI protected access 2) is a protocol that secures connections using cryptography
    - The client sends an authentication request to the access point
    - Both use the password to derive the pre-shared key (PSK)
    - Both exchange random nonces
    - Both sides use the PSK, nonces, and MAC addresses to derive the pairwise transport keys (PTK)
    - Both sides exchange MICs (Message Integrity Check) to ensure that no one has tampered witht eh nonces and that the PTK was correctly derived
    - The access point encrypts and sends the group temporal key (GTK) to the client, which is used for broadcasts that anyone can encrypt
    - The client acknowledges receipt of the GTK
- Exploiting WPA-PSK involves impersonating the access point to become a man in the middle
- Alternatively, one may simply brute force the network password
- Nonces are sent unencrypted and client IP/MAC addresses are public, so sniffing is fuitful for malicious purposes
- WPA lacks forward secrecy, meaning that the attacker who records nonce values can later derive the key if they later learn the network password or PSK

## WPA Enterprise
- One issue that arises with regular WPA is that every user starts with teh same PSK to derive the PT. This can be mitigated by having each user use a different username and password
- Similarly, instead of using PSK one may use a randomly generated key from the authentication server
- Mitigating WPA security risks still leaves on open to lower level attacks like DHCP and ARP spoofing