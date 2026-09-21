# AP CSP — Packet Captures

Recordings of real network traffic, for the Internet unit.

## Get the files

Open a Terminal in your Kali VM and run:

```bash
cd ~
git clone https://github.com/heytonyy/apcsp-packets.git
cd apcsp-packets
ls
```

If you already cloned it and just need the newest files:

```bash
cd ~/apcsp-packets
git pull
```

## Open them

```bash
wireshark &
```

Then **File → Open** and browse to `/home/kali/apcsp-packets/`.

You can also open a file directly:

```bash
wireshark 01_tcp_web.pcap &
```

## What's in here

| File | What it is |
|---|---|
| `01_tcp_web.pcap` | A computer loading a web page over a real network. Handshake, request, response. |
| `02_tcp_loss.pcap` | The same kind of transfer, but over a **deliberately damaged** connection. Watch TCP notice and fix the damage. |
| `03_udp_frames.pcap` | 200 numbered "video frames" fired out over UDP. Recorded **at the sender**. |
| `received.txt` | The list of frames that actually **arrived**. Compare it to the capture. |
| `04_dns_lookups.pcap` | Name-to-address lookups. One of them fails on purpose. |
| `05_page_load.pcap` | The whole story of loading a page: lookup → connect → request → response. |
| `06_https_compare.pcap` | The same thing, encrypted. |

## Useful display filters

Type these into the bar at the top of Wireshark and press Enter.

```
tcp                              show only TCP
udp                              show only UDP
dns                              show only name lookups
http                             show only web requests/responses
http.request                     only the requests
http.response                    only the responses
tcp.flags.syn == 1               connection-opening packets
tcp.analysis.retransmission      packets that had to be sent twice
tcp.analysis.duplicate_ack       the receiver saying "I'm still missing something"
dns.flags.rcode == 3             lookups that failed ("no such name")
```

Clear a filter with the **✕** at the right end of the bar.

## Handy right-clicks

- Right-click a packet → **Follow → TCP Stream** — read the whole conversation as text
- Right-click a packet → **Follow → HTTP Stream** — same, formatted for web traffic
- **Statistics → Conversations** — who talked to whom, and how much
- **Statistics → Protocol Hierarchy** — what fraction of the capture was each protocol

## Notes

- Files `02` and `03` were recorded on a loopback interface, so both addresses show as `127.0.0.1`. Ignore the addresses in those two — we're watching **behavior**, not geography.
- These captures contain no personal information. They're recordings of visits to `example.com` and `google.com`.
