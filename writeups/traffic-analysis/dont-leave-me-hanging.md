# Don't Leave Me Hanging

- **Category:** Traffic Analysis
- **Points:** 500

## Artifact

- Capture: `DontLeaveMeHanging.pcap`
- Approximate size: 80 MB
- SHA-256: `93bf96711676fa3675d0bd8729ccdf00ffc80ef4ddcc4e1892dfa1a45f11e6f2`

## Investigation

I began by reviewing the protocol hierarchy and TCP conversations with `tshark` and `tcpdump`. The capture contained FTP traffic between `10.20.200.60` and `10.20.200.5`, as well as SIP over TLS.

Through FTP, I retrieved `pbx-diag-20261008.tar.gz` from `/var/log/asterisk/diag`. The archive contained `README.txt`, `tls-keys.log`, `tls-keys.log.1`, `messages.log`, and `pjsip_endpoints.txt`. The configuration showed endpoint `2003` using `dtmf_mode = inband`, and `messages.log` warned that DTMF for call `C-0000009a` was being transmitted in-band.

I loaded `tls-keys.log` as the `tls.keylog_file` in Wireshark/`tshark` to decrypt SIP over TLS. Using Call-ID `06826701-6637-4eb5-b9dc-2cd970a4cd0b`, I located the target call directed to endpoint 2003.

I extracted the SRTP keys from SDP and decoded the media streams with `pylibsrtp` using the `AES_CM_128_HMAC_SHA1_80` profile. I converted the resulting G.711 μ-law audio to WAV with `sox`, then used `multimon-ng -a DTMF` to recover the DTMF digits. Finally, I grouped the digit sequence into three-digit values and converted those values to ASCII.

## Flag

```text
deadface{Pr355_0n3_f0r_D34df4c3}
```

## Tools

- `tshark` and Wireshark
- `tcpdump`
- `pylibsrtp`
- `sox`
- `multimon-ng`
- Python

## Notes

Searching the PCAP directly for `deadface{` did not reveal the answer. The relevant data was carried through a multi-stage call flow and encoded in the audio rather than stored as a literal flag string in the capture.
