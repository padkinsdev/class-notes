# DNS and DNSSEC

## DNS
- The root nameserver is hardcoded into the local DNS server
- Caching responses ensures DNS success, as a lack of caching would require the DNS path to be rediscovered each time a request is processed
    - Cache poisoning involves maliciously altering the cache contents to point to an incorrect destination

### DNS records
- A record: Gives the authoritative response for the IP address of the hostname
- NS record: Gives the name of the nameserver which has more information about the answer to the query

### Cache poisoniong
- Cache poisoning involves responding to a local nameserver request to an authoritiative DNS server with a malicious response, thus providing the incorrect mapping for a particular query (which the local nameserver will cache)
- This requires the attacker to know what the query id of the request they wish to poison is, as well as responding to the local nameserver request faster than the authoritative DNS server
- If the local nameserver chooses to randomize query ids, the attacker may simply issue a large number of query ids
- An attacker may further redirect nameserver requests by setting the A record ip corresponding to the NS record for a legitimate domain to a custom ip. This effectively tells the local nameserver that the attacker's ip is the authoritative source for DNS records related to the legitimate domain
    - Poisoning a nameserver record is more difficult but much more impactful than a simple cache poisoning

### DNSSEC
- DNSSEC involves each level's nameserver providing a public-key signature of the zone-key binding, the public key of the higher level nameserver, and the name of the higher level nameserver to ask for the next domain level path
- If DNSSEC is universally deployed and the root's keys are known, then spoofed responses are prevented. Unfortunately, DNSSEC is not universally deployed. Thus if a reply does not contain DNSSEC:
    - Accepting the reply: Insecure but at least usable
    - Denying the reply: Secure but a large number of endpoints become inaccessible