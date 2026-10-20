# CISCO 3 — MIDTERM EXAM REVIEWER
## ENSA Modules 6–9 (NAT, WAN, VPN/IPsec, QoS)
### TYPE OF TEST (166 PTS): Modified Multiple Choice • Modified True or False • Exemplification • Essay • Illustration

---

# HOW TO USE THIS REVIEWER

| Test Type | How to Answer | Where to Drill |
|---|---|---|
| **Modified MC** | Watch for "All of the above / Both A and B / Which is NOT" combos. Eliminate 2 wrong answers first. | Every module's Drill A |
| **Modified T/F** | If FALSE, you must usually state WHY or correct the wrong word. Memorize the "trap words" (always, only, never). | Every module's Drill B |
| **Exemplification** | Question says "Give examples of..." — answer with the exact list from the slides (3+ items). | Every module's Drill C |
| **Essay** | Use the point-based outlines. Write 1 idea per bullet, expand each in 1–2 sentences. | Every module's Drill D |
| **Illustration** | Draw the diagram + label ALL parts + add a 1-sentence caption. | Every module's Drill E |

**Golden rule of this exam:** The slides are the ONLY source. If a slide says "3 items," the exam wants 3 items.

---
---

# MODULE 6: NAT FOR IPv4

## 6.1 Core Concepts

- **Problem:** Not enough public IPv4 addresses for every internet device.
- **Primary use of NAT:** CONSERVE public IPv4 addresses.
- **RFC 1918 private addresses** (cannot be routed on the internet):

| Class | Range | CIDR |
|---|---|---|
| A | 10.0.0.0 – 10.255.255.255 | 10.0.0.0/8 |
| B | 172.16.0.0 – 172.31.255.255 | 172.16.0.0/12 |
| C | 192.168.0.0 – 192.168.255.255 | 192.168.0.0/16 |

- NAT router operates at the **border of a stub network**.
- The border router reads the **source** IPv4 address to decide if translation is needed.

## 6.2 NAT Terminology (4 Addresses)

> **Golden rule:** Terminology is ALWAYS from the perspective of the device being translated (PC1).

| Address | Meaning | Example (PC1) |
|---|---|---|
| **Inside Local** | Source address as seen from INSIDE network (usually private) | 192.168.10.10 |
| **Inside Global** | Source address as seen from OUTSIDE network (public) | 209.165.200.226 |
| **Outside Global** | Destination address as seen from OUTSIDE network | 209.165.201.1 |
| **Outside Local** | Destination address as seen from INSIDE network (rarely different) | 209.165.201.1 |

- **Inside** = address of the device being translated; **Outside** = address of destination device.
- **Local** = appears on the inside portion; **Global** = appears on the outside portion.

## 6.3 Types of NAT

| Feature | Static NAT | Dynamic NAT | PAT (NAT Overload) |
|---|---|---|---|
| Mapping | **1-to-1**, constant, manually configured | Pool, **first-come first-served** | Many private → **1 public** (or few) |
| Uses ports? | No | No | **Yes** — source port identifies session |
| Modifies | IPv4 address only | IPv4 address only | **Address + TCP/UDP port** |
| Best for | Web servers, devices needing consistent public address | General outbound access | Everyday internet sharing (most common) |
| Risk | Needs enough public IPs for all simultaneous sessions | If pool is full, device **must wait** | Overcomes public address shortage |

**PAT details:**
- PAT tries to **preserve the original source port**; if taken, assigns next available port from groups: **0–511, 512–1023, 1024–65535**.
- If no ports left and pool has more addresses → PAT moves to the **next external address**.
- Packets without Layer 4 port (e.g., **ICMPv4** echo) → PAT uses the **Query ID** to match request/reply.

## 6.4 Advantages vs Disadvantages of NAT

**Advantages (6):**
1. Conserves legally registered addressing scheme (privatization of intranets)
2. Conserves addresses through port-level multiplexing
3. Increases flexibility of connections to the public network
4. Provides consistency for internal addressing schemes
5. Allows private scheme to stay while easily changing public addresses
6. **Hides** internal IPv4 addresses (security through obscurity)

**Disadvantages (5):**
1. Increases **forwarding delays** (translation takes time)
2. **End-to-end addressing is lost**
3. **End-to-end traceability is lost**
4. Complicates tunneling protocols like **IPsec** (header modified → integrity checks fail)
5. Disrupts services needing connections initiated from outside / stateless protocols (UDP)

## 6.5 Configuration Commands

**Static NAT (2 tasks):**
```
R2(config)# ip nat inside source static 192.168.10.254 209.165.201.5
R2(config)# interface serial0/1/0   → ip nat inside
R2(config)# interface serial0/1/1   → ip nat outside
```

**Dynamic NAT (5 tasks):**
```
Step 1: ip nat pool NAT-POOL1 209.165.200.226 209.165.200.240 netmask 255.255.255.224
Step 2: access-list 1 permit 192.168.0.0 0.0.255.255
Step 3: ip nat inside source list 1 pool NAT-POOL1
Step 4: ip nat inside   (inside interface)
Step 5: ip nat outside  (outside interface)
```

**PAT (single address) — just add `overload`:**
```
ip nat inside source list 1 interface serial0/1/0 overload
```
**PAT (pool) — add `overload`:**
```
ip nat inside source list 1 pool NAT-POOL2 overload
```

**Verification / Maintenance:**
- `show ip nat translations` — shows active translations (static always present)
- `show ip nat translation verbose` — adds create/use time, timeout
- `show ip nat statistics` — total translations, pool size, addresses allocated
- `clear ip nat translation *` — clears ALL dynamic entries
- Default translation timeout: **24 hours** (`ip nat translation timeout`)

## 6.6 NAT64 / IPv6
- IPv6 was designed to make NAT unnecessary, BUT IPv6 has **Unique Local Addresses (ULAs)** — similar to RFC 1918 but different purpose (local communication only; NOT for extra address space, NOT for security).
- **NAT64** = protocol translation between **IPv4 and IPv6** networks (IPv6-only ↔ IPv4-only).
- NAT64 is a **temporary migration mechanism**, NOT a long-term strategy. Not private↔global IPv6 translation.

---

## MODULE 6 DRILLS

**A. Modified Multiple Choice**
1. What is the PRIMARY use of NAT?
   a) Encrypt traffic b) Conserve public IPv4 addresses c) Route private packets d) Replace DHCP
2. PAT identifies each session using the:
   a) Destination MAC b) ICMP ID c) Source port number d) ToS field
3. Which is TRUE about Static NAT?
   a) Uses a pool b) First-come first-served c) One-to-one constant mapping d) Uses overload keyword
4. Which command clears ALL dynamic NAT entries?
   a) clear ip nat statistics b) clear ip nat translation * c) no ip nat translation d) show ip nat translations
5. Which is NOT a disadvantage of NAT?
   a) Increased forwarding delay b) Loss of end-to-end addressing c) Hides internal addresses d) Complicates IPsec
6. Which statement(s) about PAT port assignment is/TRUE?
   A. It tries to preserve the original source port.
   B. Port groups used are 0–511, 512–1023, 1024–65535.
   C. It never uses ports.
   a) A only b) A and B c) B only d) All of the above

**B. Modified True or False (correct the FALSE)**
1. ___ NAT translates private addresses to public addresses.
2. ___ Static NAT requires fewer public addresses than PAT.
3. ___ NAT terminology is applied from the perspective of the destination device.
4. ___ ICMPv4 messages are handled by PAT using their Query ID.
5. ___ NAT preserves end-to-end IPv4 traceability.
6. ___ Dynamic NAT assigns addresses first-come, first-served from a pool.

**C. Exemplification**
1. Exemplify the four NAT address types using the PC1/web server scenario.
2. Give three examples of NAT advantages.
3. Give two examples of protocols/devices where NAT causes problems.
4. List the RFC 1918 private address ranges.
5. Give examples of devices best served by Static NAT.

**D. Essay — Model Answers**
1. **Explain how NAT works** (use the 6-step flow): PC1 (192.168.10.10) sends to web server 209.165.201.1 → R2 reads source address, decides translation needed → R2 adds local-to-global mapping to NAT table → R2 forwards packet with translated source (inside global 209.165.200.226) → server replies to 209.165.200.226 → R2 checks NAT table, translates back to 192.168.10.10 and delivers to PC1.
2. **Compare Static NAT, Dynamic NAT, and PAT.** (Use the 6.3 comparison table — mapping type, port usage, public IP demand, use case.)
3. **Discuss the advantages and disadvantages of NAT.** (List all 6 + 5 with 1-line explanation each.)

**E. Illustration — Practice Drawing**
1. Draw the **NAT translation flow** (PC1 → R2 → web server), labeling: inside local, inside global, outside global, NAT table, and both directions of traffic.
2. Draw a **PAT scenario** where 2 PCs share ONE public IP — show how ports (1444, 1445) differentiate sessions.

---
---

# MODULE 7: WAN CONCEPTS

## 7.1 LAN vs WAN

| | LAN | WAN |
|---|---|---|
| Area | Small geographic area | Large geographical areas |
| Connects | Local computers/peripherals | Remote users, networks, sites |
| Owned by | Organization/home user | ISP, telephone, cable, satellite providers |
| Fee | No usage fee | Provided **for a fee** |
| Speed | High bandwidth (Ethernet/Wi-Fi) | Low-to-high bandwidth over long distances |

- **Private WAN** = dedicated to a single customer → guaranteed service level, consistent bandwidth, security.
- **Public WAN** = via internet → variable service/bandwidth, no security guarantee.

## 7.2 WAN Topologies (5)

| Topology | Key Trait | Weakness/Cost |
|---|---|---|
| **Point-to-Point** | Circuit between two endpoints; L2 transport; transparent to customer | Expensive with many connections |
| **Hub-and-Spoke** | One hub interface shared by all spokes; spokes talk only via hub | **Hub = single point of failure** |
| **Dual-homed** | Redundancy, load balancing, backup connections | More expensive, more complex config |
| **Fully Meshed** | Multiple virtual circuits connect ALL sites | Most fault-tolerant (also costly) |
| **Partially Meshed** | Connects many but not all sites | Less redundancy than full mesh |

- **SLA** = service level agreement (reliability/availability).
- **Carrier** owns/maintains physical connection.
- **Single-carrier** = 1 provider, 1 SLA. **Dual-carrier** = 2 providers, 2 SLAs → redundancy + higher availability.

## 7.3 WAN Standards & OSI

- Standards bodies: **TIA/EIA, ISO, IEEE**
- **Layer 1:** SDH (global), SONET (North American), DWDM (multiplexes multiple wavelengths to boost capacity)
- **Layer 2:** Broadband (DSL/Cable), Wireless, Ethernet WAN (Metro Ethernet), MPLS, PPP, HDLC, Frame Relay (legacy), ATM (legacy)

## 7.4 WAN Terminology & Devices

| Term | Definition |
|---|---|
| **DTE** | Connects subscriber LANs to WAN communication device |
| **DCE** | Device used to communicate with the provider |
| **CPE** | DTE + DCE devices at enterprise edge |
| **POP** | Point where subscriber connects to provider network |
| **Demarcation point** | Physical spot separating CPE from provider equipment |
| **Local loop (last mile)** | Cable connecting CPE to provider CO |
| **CO** | Local provider facility connecting CPE to provider network |
| **Toll network** | Long-haul fiber lines/switches inside provider network |
| **Backhaul network** | Connects multiple access nodes of provider network |
| **Backbone** | Large high-capacity networks interconnecting providers (redundant) |

**Devices:** Voiceband modem (dial-up, legacy), DSL/Cable modem (broadband, Ethernet to DTE), **CSU/DSU** (connects digital device to digital leased line), Optical converter (fiber↔copper), Wireless router/AP, WAN core (high-speed routers + L3 switches).

## 7.5 Switching Types

- **Serial communication:** bits sent sequentially over a single channel; parallel is limited to short distances (timing sensitivity).
- **Circuit-switched:** dedicated circuit/path established BEFORE communication; same path always. Examples: **PSTN** (dial-up, <56 kbps), **ISDN** (digital, 45 Kbps–2.048 Mbps).
- **Packet-switched:** data split into packets over shared network; cheaper + more flexible. Examples: **Ethernet WAN, MPLS, Frame Relay, ATM**.

## 7.6 Traditional WAN

- **Leased lines** = point-to-point lines you pay monthly for.
  - **T-carrier (North America):** T1 = **1.544 Mbps**, T3 = **43.7 Mbps**
  - **E-carrier (Europe):** E1 = **2.048 Mbps**, E3 = **34.368 Mbps**
  - Advantages: **Simplicity, Quality, Availability** (permanent dedicated capacity for VoIP/video)
  - Disadvantages: **Cost** (most expensive WAN access), **Limited flexibility** (fixed capacity vs variable traffic)
- **Frame Relay:** L2 NBMA technology; creates **PVCs** identified by **DLCI**.
- **ATM:** cell-based (NOT frame-based); cells fixed **53 bytes**; carries voice/video/data.
- Both Frame Relay & ATM largely replaced by Metro Ethernet/internet solutions.

## 7.7 Modern WAN

- **Dedicated broadband:** fiber installed/leased (dark fiber).
- **Ethernet WAN** (over fiber) — also called **Metro E, EoMPLS, VPLS**. Benefits: reduced expenses/admin, easy integration, enhanced productivity. Replaces serial P2P, Frame Relay, ATM.
- **MPLS:** high-performance provider routing tech; works regardless of access method/payload.
  - Supports Ethernet, DSL, Cable, Frame Relay access; encapsulates IPv4 & IPv6.
  - Router roles: **CE (customer edge), PE (provider edge), P (internal provider)**; all are **LSRs** (attach labels).
  - Provides QoS, traffic engineering, redundancy, VPNs.

## 7.8 Internet-Based Connectivity

- **Wired:** DSL, cable, optical fiber (consistent bandwidth, lower error/latency).
- **Wireless:** cheaper (radio waves) but affected by distance, interference, weather.

**DSL:**
- High-speed, always-on, uses existing **twisted-pair** telephone lines.
- **ADSL/ADSL2+** = asymmetric (higher downstream); **SDSL** = symmetric (same both ways).
- Rate depends on local loop length/condition.
- **DSLAM** at the CO concentrates DSL subscribers. DSL is **NOT a shared medium**.
- **PPP** is the Layer 2 protocol: authenticates subscriber, assigns public IPv4, link-quality management. **PPPoE** deployment: host with PPPoE client, or router as PPPoE client.

**Cable:**
- Coaxial cable; **DOCSIS** = international standard for adding data to cable systems.
- Optical node converts RF→light; **CMTS** at provider headend talks to cable modems.
- **Shared bandwidth** — more users = possible speed drop.

**FTTx (highest bandwidth of all broadband):**
- **FTTH** = fiber to home boundary; **FTTB** = fiber to building boundary; **FTTN** = fiber to node/neighborhood.

**Wireless:**
- Municipal Wi-Fi; Cellular (3G/4G/5G, LTE); Satellite (rural/remote, geosynchronous orbit, affected by trees/heavy rain); **WiMAX = IEEE 802.16** (broad cell-like coverage).

**VPN (intro from M7):** encrypted connection between private networks over a public network. Benefits: **cost savings, security, scalability, compatibility**. Types: **site-to-site** (configured on routers, clients unaware) and **remote access** (user aware/initiates, e.g., HTTPS to bank).

**ISP connectivity options (4):**
| Option | Connection | Trait |
|---|---|---|
| Single-homed | 1 link to 1 ISP | No redundancy, cheapest |
| Dual-homed | 2 links to SAME ISP | Redundancy + load balancing; dies if ISP fails |
| Multihomed | Links to 2 DIFFERENT ISPs | More redundancy + load balancing, expensive |
| Dual-multihomed | Redundant links to multiple ISPs | MOST resilient, MOST expensive |

**SPAN Engineering evolution:** Small (DSL broadband) → Campus (**CAN**, firewall, in-house IT) → Branch (**MAN** for city sites, dedicated lines) → Distributed (thousands worldwide, **site-to-site + remote access VPNs**).

---

## MODULE 7 DRILLS

**A. Modified Multiple Choice**
1. Which topology is the MOST fault-tolerant?
   a) Hub-and-spoke b) Point-to-point c) Fully meshed d) Partially meshed
2. T1 bandwidth (North America) is: a) 2.048 Mbps b) 1.544 Mbps c) 43.7 Mbps d) 56 kbps
3. Which Layer 1 standard is the NORTH AMERICAN optical standard?
   a) SDH b) SONET c) DWDM d) MPLS
4. ATM cells are always how many bytes? a) 64 b) 1500 c) 53 d) 48
5. Which is TRUE?
   A. DSL is a shared medium. B. Cable bandwidth is shared among subscribers.
   C. MPLS routers are called LSRs.
   a) A only b) B and C c) A and C d) All of the above
6. Which is NOT a WAN topology from the slides?
   a) Dual-homed b) Star-and-leaf c) Partially meshed d) Hub-and-spoke
7. Which option connects to two DIFFERENT ISPs?
   a) Dual-homed b) Single-homed c) Multihomed d) Dual-multihomed

**B. Modified True or False (correct the FALSE)**
1. ___ WAN services are provided for a fee, while a LAN has no usage fee.
2. ___ In a hub-and-spoke topology, spoke routers can communicate directly with each other.
3. ___ ISDN allows rates up to 2.048 Mbps, while PSTN dial-up is limited to under 56 kbps.
4. ___ Frame Relay PVCs are identified by a DLCI.
5. ___ MPLS can only carry IPv4 traffic.
6. ___ A dual-homed ISP connection survives an outage of that ISP.

**C. Exemplification**
1. Exemplify the 5 WAN logical topologies.
2. Give examples of Layer 1 and Layer 2 WAN protocols.
3. List examples of circuit-switched and packet-switched WAN technologies.
4. Give examples of WAN devices.
5. Exemplify wireless internet-based broadband options.
6. Give the four ISP connectivity options with one trait each.

**D. Essay — Model Answers**
1. **Compare LANs and WANs** (5 points of comparison: area, what they connect, ownership, fee, speed).
2. **Explain circuit-switched vs packet-switched communication** (dedicated path vs shared network; PSTN/ISDN vs Metro Ethernet/MPLS; cost & flexibility).
3. **Discuss traditional vs modern WAN connectivity** (leased lines T/E-carrier + pros/cons → Ethernet WAN/MPLS/internet broadband).

**E. Illustration — Practice Drawing**
1. Draw and label a **point-to-point, hub-and-spoke, and fully meshed** WAN topology (3 drawings or 1 combined figure).
2. Draw the **WAN demarcation chain**: CPE (DTE/DCE) → demarcation point → local loop → CO → toll/backbone network → POP.

---
---

# MODULE 8: VPN AND IPSEC CONCEPTS

## 8.1 VPN Technology

- **VPN** = virtual (private info carried over a PUBLIC network) + private (traffic is **encrypted**).
- **4 Benefits:**
  1. **Cost savings** — reduces connectivity costs while increasing bandwidth
  2. **Security** — encryption + authentication protocols
  3. **Scalability** — add users without new infrastructure
  4. **Compatibility** — works across WAN link options incl. broadband

| | Site-to-Site VPN | Remote Access VPN |
|---|---|---|
| Terminated on | **VPN gateways** | Client ↔ VPN terminating device |
| Encryption | Only **between gateways** | Between client and device |
| Host awareness | Internal hosts have **no knowledge** of VPN | User is aware & initiates |
| Creation | Pre-configured on routers | **Dynamically created** when needed |
| Encryption tech | IPsec | IPsec or SSL |

- **Enterprise VPNs** — created/managed by enterprise (IPsec & SSL).
- **Service Provider VPNs** — created/managed by provider using **MPLS** at L2/L3.

## 8.2 Types of VPNs

**Remote-Access:**
- **Clientless** — secured via web browser SSL connection.
- **Client-based** — requires VPN client software (e.g., **Cisco AnyConnect**).

**IPsec vs SSL (memorize this table):**

| Feature | IPsec | SSL |
|---|---|---|
| Applications | **Extensive** – all IP-based apps | **Limited** – web-based + file sharing |
| Auth strength | **Strong** – two-way (shared keys/certs) | Moderate – one-way or two-way |
| Encryption | Strong – **56–256 bits** | Moderate-strong – **40–256 bits** |
| Complexity | Medium – needs VPN client installed | Low – just a web browser |
| Connection option | Limited – specific devices/configs | Extensive – any device with browser |

**Site-to-site IPsec flow:** hosts send normal unencrypted traffic → VPN gateway encapsulates & encrypts → tunnel → receiving gateway strips headers, decrypts, relays to target host.

**GRE over IPsec:**
- **GRE** = non-secure tunneling protocol; encapsulates various network-layer protocols **+ multicast & broadcast**.
- GRE has **no encryption by default** → not secure alone.
- Standard IPsec alone = **unicast only**. GRE inside IPsec secures **multicast routing updates** (e.g., OSPF over IPsec).
- **3 terms:** **Passenger** = original packet (e.g., OSPF), **Carrier** = GRE (encapsulates), **Transport** = IPv4/IPv6 (forwards it).

**DMVPN (Dynamic Multipoint VPN):**
- Cisco solution for **many** sites — easy, dynamic, scalable.
- **Hub-and-spoke** configuration establishing a **full mesh** topology.
- Uses **mGRE** (multipoint GRE): ONE GRE interface dynamically supports multiple IPsec tunnels.
- Spokes learn about each other → **spoke-to-spoke tunnels** possible.

**IPsec VTI (Virtual Tunnel Interface):**
- Config applied to a **virtual interface** instead of static mapping to physical interface.
- Handles **unicast AND multicast** → routing protocols work **without GRE**.
- Works point-to-point or hub-and-spoke.

**Service Provider MPLS VPNs:**
- Traffic forwarded by **labels**; customers cannot see each other's traffic (secure).
- **Layer 3 MPLS VPN** — provider participates in customer routing (peering between customer & provider routers).
- **Layer 2 MPLS VPN** — provider NOT involved in routing; uses **VPLS** to emulate Ethernet multiaccess LAN; no routing involved.

## 8.3 IPsec

**IPsec = IETF standard** for securing VPNs over IP. Provides:
1. **Confidentiality** — encryption algorithms prevent reading packet contents
2. **Integrity** — hashing algorithms ensure no alteration in transit
3. **Origin authentication** — **IKE** protocol authenticates source/destination
4. **Diffie-Hellman** — secures key exchange

- IPsec is **not bound to specific rules** — "open slots" in the framework can be filled with any choice → creates a unique **Security Association (SA)**.
- IPsec protects traffic **Layer 4 through Layer 7**.

**Protocol encapsulation (first building block):**
| | AH | ESP |
|---|---|---|
| Confidentiality | **No** | **Yes** |
| Authentication | Yes | Yes |
| Use when | Confidentiality not required/permitted | Need both confidentiality + auth |

**Confidentiality (symmetric key cryptosystems):**
| Algorithm | Key |
|---|---|
| DES | **56-bit** |
| 3DES | three independent **56-bit** keys per 64-bit block |
| AES | **128, 192, 256-bit** |
| SEAL | **160-bit**, stream cipher (encrypts continuously, not blocks) |

Shorter key = easier to break.

**Integrity (HMAC — Hashed Message Authentication Code):**
- **MD5** — 128-bit shared-secret key
- **SHA** — 160-bit secret key

**Authentication (2 peer methods):**
1. **Pre-shared key (PSK)** — entered manually on each peer; easy; **does not scale**; must be on every peer.
2. **RSA** — uses **digital certificates**; each peer authenticates the opposite peer before tunnel is secure.

**Diffie-Hellman groups:**
- Groups **1, 2, 5** → should NO LONGER be used.
- **14 / 15 / 16** → **2048 / 3072 / 4096** bits
- **19 / 20 / 21 / 24** → **256 / 384 / 521 / 2048** bits, use **ECC** (faster key generation)
- DH lets two peers establish a **shared secret key over an insecure channel**.

---

## MODULE 8 DRILLS

**A. Modified Multiple Choice**
1. IPsec provides confidentiality, integrity, origin authentication, and:
   a) Compression b) Diffie-Hellman key exchange c) DHCP d) NAT
2. Which IPsec protocol provides BOTH confidentiality and authentication?
   a) AH b) GRE c) ESP d) IKE
3. DES uses a key length of: a) 128 bits b) 56 bits c) 256 bits d) 160 bits
4. In GRE-over-IPsec, GRE is the: a) passenger b) carrier c) transport d) payload
5. Which VPN type are end hosts unaware of?
   a) Remote access b) Site-to-site c) SSL d) Client-based
6. Which statement(s) is/are TRUE?
   A. PSK does not scale well. B. RSA uses digital certificates.
   C. AH supports confidentiality.
   a) A and B b) B only c) A, B, C d) All of the above
7. Which is NOT a benefit of VPNs?
   a) Cost savings b) Security c) Scalability d) Reduced latency guarantee

**B. Modified True or False (correct the FALSE)**
1. ___ A VPN is "virtual" because it transports private information over a public network.
2. ___ In a site-to-site VPN, internal hosts know a VPN is being used.
3. ___ Standard IPsec VPNs can secure multicast routing updates by themselves.
4. ___ IPsec provides confidentiality, integrity, origin authentication, and Diffie-Hellman.
5. ~~SSL~~ IPsec supports only web-based applications.
6. ___ DMVPN uses mGRE so one GRE interface can support multiple IPsec tunnels.

**C. Exemplification**
1. Give the four essential security functions of IPsec.
2. List the four benefits of VPNs.
3. Exemplify the symmetric encryption algorithms with their key lengths.
4. Give examples of remote-access VPN connection types (clientless vs client-based).
5. List two IPsec peer authentication methods with one advantage/limitation each.

**D. Essay — Model Answers**
1. **Explain what a VPN is and its benefits.** (virtual + private definition, then cost savings/security/scalability/compatibility.)
2. **Compare site-to-site and remote-access VPNs** (termination, encryption scope, host awareness, who initiates).
3. **Explain the IPsec framework and its four functions**, including how AH vs ESP differ.

**E. Illustration — Practice Drawing**
1. Draw a **site-to-site IPsec VPN**: two LANs, two VPN gateways, encrypted tunnel over the internet, hosts sending unencrypted traffic internally. Label passenger flow.
2. Draw the **GRE-over-IPsec encapsulation stack**: original packet (passenger) inside GRE (carrier) inside IPsec inside IPv4 (transport).
3. Draw/duplicate the **IPsec framework diagram** with its four "open slot" functions.

---
---

# MODULE 9: QoS CONCEPTS

## 9.1 Network Transmission Quality

- When traffic > capacity → devices **queue** packets → causes **delay** → too many packets → memory fills → **packets dropped**.
- **A device implements QoS ONLY when it is experiencing congestion.**
- Bandwidth measured in **bps**.
- **Congestion points:** **aggregation, speed mismatch, LAN to WAN** — ideal QoS candidates.

**Delay types:**

| Delay | Type |
|---|---|
| Code delay (compression at source) | Fixed |
| Packetization delay (encapsulation) | Fixed |
| **Queuing delay** (waiting to transmit) | **Variable** |
| Serialization delay (transmitting frame onto wire) | Fixed |
| **Propagation delay** (travel between devices) | **Variable** |
| De-jitter delay (buffering, evenly spaced) | Fixed |

- **Jitter = variation of delay of received packets.**
- RTP audio → **playout delay buffer** smooths jitter; out-of-range packets discarded → audio dropouts.
- DSP interpolates for small losses; proper design → packet loss near **zero**.
- Without QoS, voice/video dropped as often as email/web.

## 9.2 Traffic Characteristics

| Trait | Voice | Video | Data |
|---|---|---|---|
| Pattern | **Smooth, benign** | **Bursty, greedy** | Smooth or bursty; benign/greedy |
| Drop sensitive | Yes | Yes (more than voice) | **Insensitive** (TCP retransmits) |
| Delay sensitive | Yes | Yes | **Insensitive** |
| Transport | **UDP priority** | **UDP priority** (RTSP port 554) | TCP |
| Latency | **< 150 ms** | **< 200–400 ms** | varies |
| Jitter | **< 30 ms** | **< 30–50 ms** | n/a |
| Loss | **< 1%** | **< 0.1–1%** | tolerable |
| Bandwidth | **30–128 Kbps** | **384 Kbps–20 Mbps** | variable |

- Voice uses **RTP ports 16384–32767** (Cisco).
- Video packet size/number varies every **33 ms**.
- Video = 70% of traffic (2017) → 82% (per Cisco VNI).
- Data QoE matrix: **Mission-critical + interactive** → lowest delay, 1–2 sec response; **not mission-critical + not interactive** → gets leftover bandwidth after voice/video.

## 9.3 Queuing Algorithms (congestion MANAGEMENT)

> Queuing becomes active **only on congestion**. It buffers, prioritizes, and can reorder packets.

| Algorithm | Key Points |
|---|---|
| **FIFO** | One queue, arrival order, **no priority/classes**, all treated equally |
| **WFQ** | Automated, **fair bandwidth** to all traffic; weights; classifies into **flows** by src/dst IP, MAC, port, protocol, ToS; **NOT supported with tunneling/encryption** (packet content modified) |
| **CBWFQ** | Extends WFQ to **user-defined classes** (match by protocol, ACL, input interface); **FIFO queue per class**; **bandwidth = guaranteed** during congestion; queue limit → **tail drop** (default response, treats all equally) |
| **LLQ** | Brings **strict priority queuing (PQ)** to CBWFQ; delay-sensitive (voice) sent **first**; Cisco recommends **ONLY voice** in priority queue |

## 9.4 QoS Models

| Model | Key Points |
|---|---|
| **Best-effort** | No QoS configured; most scalable; easiest/fastest; **no guarantees**, no preferential treatment |
| **IntServ** | Highest guarantee, **end-to-end**; resource reservation + admission control; **RSVP** signaling; connection-oriented; per-flow/microflow; edge router does admission control; **resource-intensive, NOT scalable** |
| **DiffServ** | **Scalable**, flexible; classifies into **aggregates (classes)**; **hop-by-hop**; NOT end-to-end guarantee; many levels of service; no absolute guarantee |

- IntServ & DiffServ sometimes **co-deployed**.

## 9.5 QoS Implementation Techniques

**3 categories of QoS tools:**
1. **Classification & Marking**
2. **Congestion Avoidance**
3. **Congestion Management** (queuing/scheduling)

**Avoiding packet loss (3 approaches):**
1. Increase link capacity
2. Guarantee bandwidth + buffer space (WFQ, CBWFQ, LLQ)
3. Drop lower-priority packets BEFORE congestion → **WRED**

**Tool sequence:** Ingress packets **classified & marked** → resources allocated per policy → **queued** → forwarded on egress per shaping/policing policy.
- Classification/marking: **ingress or egress**; queuing/shaping: **usually egress**.

**Classification:**
- L2/L3: interfaces, **ACLs**, **class maps**
- L4–7: **NBAR** (Network Based Application Recognition)

**Marking fields:**

| Layer | Field | Bits |
|---|---|---|
| Ethernet 802.1q/802.1p | **CoS** | 3 |
| 802.11 Wi-Fi | TID | 3 |
| MPLS | **EXP** | 3 |
| IPv4/IPv6 | **IP Precedence (IPP)** | 3 |
| IPv4/IPv6 | **DSCP** | 6 |

- **802.1Q** = VLAN tagging; **802.1p** = QoS using first 3 bits of TCI (Priority field) → **CoS 0–7 (8 levels)**.
- IPv4 **ToS** / IPv6 **Traffic Class** = 8-bit field.
- **RFC 791** = 3-bit IPP; **RFC 2474** supersedes → **DSCP (6 bits) = 64 classes**; remaining 2 bits = **ECN** (mark instead of drop).

**CoS values (memorize):**
| CoS | Binary | Use |
|---|---|---|
| 0 | 000 | Best-Effort Data |
| 1 | 001 | Medium-Priority Data |
| 2 | 010 | High-Priority Data |
| 3 | 011 | **Call Signaling** |
| 4 | 100 | **Videoconferencing** |
| 5 | 101 | **Voice bearer** |
| 6/7 | 110/111 | Reserved |

**DSCP categories:**
- **BE (Best-Effort)** = 0, default, normal routing, dropped on congestion
- **EF (Expedited Forwarding)** = decimal **46** (binary 101110); first 3 bits (101) = **CoS 5**; Cisco recommends **EF only for voice**
- **AF (Assured Forwarding)** = RFC 2597; 5 MSB for queue + drop preference
  - **AFxy:** 1st 3 bits = class (**4 = best queue, 1 = worst**), 4th+5th = drop preference, 6th bit = 0
  - Example: **AF32** = class 3 (011) + medium drop pref (10) → 011100 = **28**
- **CS (Class Selector)** = first 3 MSB of DSCP; map directly to CoS & IPP (compatibility with 802.1p & RFC 791)

**Trust Boundary:**
- Mark traffic **as close to source as possible** = trust boundary.
- 1. **Trusted endpoints** mark their own CoS/DSCP → 2. **Secure endpoints** → marked at **L2 switch** → 3. marked at **L3 switch/router**.

**Congestion Avoidance:**
- Monitors queue depth: below min → no drops; filling to max → small % dropped; past max → all dropped.
- **WRED** = weighted random early detection: buffer management, lets TCP throttle back before buffers exhausted; **avoids tail drops**, maximizes TCP performance.

**Shaping vs Policing:**
| | Shaping | Policing |
|---|---|---|
| Direction | **Outbound** | **Inbound** |
| Action | **Retains** excess in queue, transmits later → **smoothed rate** | Enforces **CIR** (may allow bursting) |
| Used by | Enterprises | **Service providers** |

**QoS Policy Guidelines (3):**
1. Enable **queuing at every device** in the path
2. **Classify and mark as close to source** as possible
3. **Shape and police as close to source** as possible

---

## MODULE 9 DRILLS

**A. Modified Multiple Choice**
1. Jitter is defined as:
   a) Total latency b) Variation of delay of received packets c) Packet loss d) Bandwidth
2. Voice latency should be no more than:
   a) 400 ms b) 150 ms c) 30 ms d) 1000 ms
3. Which queuing type brings strict priority queuing to CBWFQ?
   a) FIFO b) WFQ c) LLQ d) WRED
4. EF (Expedited Forwarding) DSCP decimal value is: a) 46 b) 28 c) 0 d) 5
5. Which statement(s) is/TRUE?
   A. WFQ works with encryption. B. Shaping is outbound. C. Policing is inbound.
   a) A only b) B and C c) A and C d) All of the above
6. CoS value 5 (101) is used for: a) Voice bearer b) Call signaling c) Videoconf d) Best-effort
7. Which QoS model uses RSVP?
   a) Best-effort b) IntServ c) DiffServ d) DiffServ AF
8. What does AF32 decode to?
   a) Class 3, medium drop preference, DSCP 28 b) Class 3, low drop preference, DSCP 32
   c) Class 2, medium drop preference, DSCP 32 d) Class 4, high drop, 28
9. Which is NOT a congestion avoidance technique?
   a) WRED b) Tail drop c) Min/max threshold monitoring d) Early drop of low priority

**B. Modified True or False (correct the FALSE)**
1. ___ A device implements QoS only when experiencing congestion.
2. ___ De-jitter delay and queuing delay are both variable delays.
3. ___ WFQ is not supported with tunneling and encryption.
4. ___ LLQ should carry voice AND large file transfers in the priority queue.
5. ___ DiffServ provides an absolute end-to-end guarantee of service quality.
6. ___ DSCP has 6 bits, giving 64 possible classes of service.
7. ___ Policing retains excess packets in a queue to smooth output.

**C. Exemplification**
1. List the three typical congestion points.
2. Give the six sources of delay and mark fixed vs variable.
3. Exemplify voice traffic requirements (latency, jitter, loss, bandwidth, ports).
4. List the four queuing algorithms.
5. Give examples of 3 QoS tool categories.
6. List the three QoS policy guidelines.

**D. Essay — Model Answers**
1. **Explain why QoS is needed** (traffic > capacity → queue → delay → drops; voice/video dropped same as email without QoS; congestion points).
2. **Compare voice, video, and data traffic characteristics** (use the 9.2 table: pattern, sensitivity, transport, latency/jitter/loss/bandwidth).
3. **Compare the three QoS models** (best-effort / IntServ with RSVP / DiffServ) with benefits and drawbacks.
4. **Compare FIFO, WFQ, CBWFQ, and LLQ.**

**E. Illustration — Practice Drawing**
1. Draw the **QoS tool sequence**: classify → mark → allocate resources → queue → shape/police → forward (ingress vs egress labeled).
2. Draw/duplicate the **CoS table (0–7)** and the **DSCP categories (BE/EF/AF/CS)**.
3. Draw the **trust boundary**: trusted endpoint → L2 switch → L3 switch/router, with CoS/DSCP marking points.
4. Draw a **WRED queue chart**: min threshold / max threshold / drop behavior.

---
---

# FINAL REVIEW — HIGH-PROBABILITY ESSAY TOPICS
*(Practice writing these in 5–8 sentences each)*

1. **How NAT works step-by-step** + types of NAT comparison (M6)
2. **Advantages and disadvantages of NAT** (M6)
3. **LAN vs WAN** — five comparison points (M7)
4. **Circuit-switched vs packet-switched** communication (M7)
5. **What is a VPN? Benefits? Site-to-site vs remote access?** (M8)
6. **The IPsec framework** — four functions, AH vs ESP, algorithms (M8)
7. **Why QoS is needed** + traffic characteristics of voice/video/data (M9)
8. **Queuing algorithms** — FIFO, WFQ, CBWFQ, LLQ (M9)
9. **QoS models** — Best-effort, IntServ, DiffServ (M9)

---

# FINAL REVIEW — ILLUSTRATION SHORTLIST
*(These diagrams are the most likely to be drawn)*

1. NAT translation flow with 4 address types (M6)
2. PAT with 2 PCs sharing 1 public IP + ports (M6)
3. WAN topologies (point-to-point, hub-spoke, mesh) (M7)
4. Site-to-site VPN over the internet (M8)
5. GRE-over-IPsec encapsulation stack (M8)
6. QoS tools sequence (classify→mark→queue→shape) (M9)
7. CoS/DSCP marking table (M9)

---

# TRAP-WORD CHECKLIST (Modified T/F favorites)

- NAT is used to **increase** forwarding speed → **FALSE** (it increases delays)
- Static NAT needs **fewer** public IPs than PAT → **FALSE**
- Spokes in hub-and-spoke communicate **directly** → **FALSE** (via hub only)
- MPLS carries **only IPv4** → **FALSE** (IPv4 + IPv6, any protocol)
- DSL is a **shared** medium → **FALSE** (cable is shared; DSL is not)
- Site-to-site VPN hosts **know** they use a VPN → **FALSE**
- GRE **provides encryption** → **FALSE** (needs IPsec)
- IPsec **AH** provides confidentiality → **FALSE** (only ESP does)
- PSK **scales well** → **FALSE**
- QoS is always active → **FALSE** (only during congestion)
- LLQ should carry **all** traffic → **FALSE** (voice only)
- DiffServ gives **absolute** guarantees → **FALSE** (that's IntServ)
- WFQ works **with** encryption → **FALSE**
- Shaping is **inbound** → **FALSE** (outbound; policing is inbound)
