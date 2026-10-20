# DEADFACE CTF 2026 — Write-ups

Personal technical write-ups from DEADFACE CTF 2026, held on October 17–18, 2026. I documented the challenges for which I retained enough technical notes to describe the solution accurately.

## Solved challenges

| Challenge | Category | Points | Write-up |
|---|---|---:|---|
| Too Good to be True | Database | 310 | [Read write-up](writeups/database/too-good-to-be-true.md) |
| Two Places at Once | Database | 450 | [Read write-up](writeups/database/two-places-at-once.md) |
| Skeleton Key | Pwn | 125 | [Read write-up](writeups/pwn/skeleton-key.md) |
| The Crypt | Pwn | 100 | [Read write-up](writeups/pwn/the-crypt.md) |
| Prying Eyes | Database | 55 | Notes not yet documented |
| The Attending | Database | 40 | Notes not yet documented |
| Admitted Under Another Name | Database | 15 | Notes not yet documented |
| A Pirate's Life for Me: Part 1 | Traffic Analysis | 25 | [Read write-up](writeups/traffic-analysis/a-pirates-life-part-1.md) |
| Factory Settings | Brain Scans | 25 | Notes not yet documented |
| Ghost of Squares Past | OSINT | 150 | Notes not yet documented |
| Don't Leave Me Hanging | Traffic Analysis | 500 | [Read write-up](writeups/traffic-analysis/dont-leave-me-hanging.md) |
| Car Crash | OSINT | 50 | Notes not yet documented |
| Data Go Brrrrr | Traffic Analysis | 25 | Notes not yet documented |
| Bad Guys Be Bad | Traffic Analysis | 350 | Notes not yet documented |
| Who Goes There? | De Monne Breach | 25 | Notes not yet documented |

## Techniques covered

- SQL schema inspection, joins, grouping, and statistical outlier detection
- PCAP triage with Wireshark and `tshark`
- SIP/TLS and SRTP analysis, audio conversion, and DTMF extraction
- ELF inspection, `checksec`, `objdump`, stack-overflow analysis, and ROP basics
- Running an x86-64 ELF under QEMU on an ARM64 Kali environment

## Scope

I wrote these notes from the steps and artifacts retained during the event. I have not filled in missing challenge solutions with guesses. The local test flag mentioned in the Skeleton Key write-up is explicitly a test value, not a claimed event flag.

Please check the event rules before sharing challenge-specific material publicly.
