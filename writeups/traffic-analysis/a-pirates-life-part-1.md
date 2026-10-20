# A Pirate's Life for Me: Part 1

- **Category:** Traffic Analysis
- **Points:** 25

## Artifacts

- Archive: `pirateslife.7z`
- Capture: `ctf2026dead.pcapng`
- SHA-256: `7bcebd81928b2d01718f2f853c507ea313a44e2b19037f4b7d0cc1c638affe93`

## Investigation

I extracted the capture and opened it in Wireshark. With more than 400,000 packets, I narrowed the traffic using this display filter:

```wireshark
http.user_agent contains "Sunstalker"
```

The matching requests used the User-Agent `SunstalkerDFBrower/2.0 (Linux; Android 14)`. Three HTTP requests to different domains contained custom headers with consecutive fragments:

| Packet | Domain | Header value |
|---:|---|---|
| 6 | `nightvaleuniversity.edu` | `X-1-Trace-Part: deadface{X-M4rk5_` |
| 8 | `aurorahhs.com` | `X-2-Mark: th3_sp0t_` |
| 11 | `birdsarejustbirds.gov` | `X-3-Spot: I_gu355}` |

I concatenated the values in X-1, X-2, X-3 order, preserving their exact characters and capitalization.

## Reconstructed flag

```text
deadface{X-M4rk5_th3_sp0t_I_gu355}
```
