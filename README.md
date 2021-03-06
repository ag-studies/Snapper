## Synopsis

Snapper is a command-line utility that sniffs your local network for active
TCP connections (only those with ACK flags set) and then sends RST (reset)
packets to terminate those connections (using RAW sockets). Therefore
"snapper" is a program that generates a single packet DoS attack. It is
written as a proof-of-concept for a "Protocol Design" [PD] class at Aalto
University. You may have to start your browser in "incognito mode", so that
the browser does not load the pages from the cache. If this works
correctly, your browser tabs may not load or may load partially. You can
also verify the effect of the RST packet in [wireshark] where you'll notice
the browser reinitiate TCP handshake (SYN, SYN-ACK).

Sending spurious RST Packets can be used for malicious purposes 
(e.g., DoS attack [1,4]) or closing down suspicious connections. In recent years, 
it has been used by ISPs to close down P2P connections (e.g., by [Comcast]
(http://en.wikipedia.org/wiki/Comcast#Network_neutrality)).

## Compiling:
The project comes with a basic Makefile. If your system does not have
libpcap installed you can download it from [2]. 

[Note 1]: If you are compiling on linux, it is likely you will have to
change:
```
iph->ip_len = (IPTCPHDRSIZE);
to
iph->ip_len = htons(IPTCPHDRSIZE);
```

[Note 2]: If you want to capture some other types of packets (e.g., IP,
ICMP, UDP) then change the filter expression in: 
```
char filter_exp[] = "(tcp[13] == 0x10)";
to 
char filter_exp[] = "ip";
```
for capturing IP packets instead of TCP ACK packets (comments in code has
some examples)[3].

[Note 3]: If you want to parse a packet look at `got_packet()` there is a
switch case that parses the protocol field. You can add your own code or
function to parse the packet.

## References
- [1] "Detecting Forged TCP Reset Packets"; N. Weaver, R. Sommer, V. Paxson
- [2] http://www.tcpdump.org/release/libpcap-1.2.1.tar.gz
- [3] http://wiki.wireshark.org/CaptureFilters
- [4] "Slipping in the Window: TCP Reset Attacks"; Watson, P.
- [wireshark] http://www.wireshark.org/
- [PD] https://noppa.aalto.fi/noppa/kurssi/s-38.3159/
