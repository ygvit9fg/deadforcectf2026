# Skeleton Key

- **Category:** Pwn
- **Points:** 125
- **Binary:** `Skeleton_Key`
- **SHA-256:** `9d3b0e9b9d13f48977b922c860f6ae73479277fc723b30007c63d7c4637c03ff`
- **Challenge service noted during the event:** `159.65.228.107:5002`

## Binary inspection

I used `file` and `checksec` to inspect the binary. It was an x86-64 ELF with Partial RELRO, NX enabled, no stack canary, and no PIE. The symbols were present, so I used `objdump` to inspect `main`, `Wretched_Lock`, and `gadgets`.

`Wretched_Lock` compared its argument with `0xdeadc0de`. Passing the expected value would cause the function to open `flag.txt` and print its contents. The input path in `main` used `gets()`, allowing a stack buffer overflow and return-address overwrite.

## ROP chain

The relevant addresses were:

- `pop rdi; ret`: `0x401615`
- `ret`: `0x401016`
- `Wretched_Lock`: `0x401507`

I built a ROP chain using a 72-byte offset, a `ret` for stack alignment, `pop rdi; ret`, the value `0xdeadc0de`, and the address of `Wretched_Lock`.

## Running the x86-64 binary on ARM64 Kali

My Kali environment was ARM64, while the challenge binary targeted x86-64. QEMU needed the explicit x86-64 loader and library path:

```bash
qemu-x86_64 \
  ~/libc-database/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2 \
  --library-path ~/libc-database/lib/x86_64-linux-gnu \
  ./Skeleton_Key
```

For a local test, I created `flag.txt` containing `deadface{LOCAL_TEST_FLAG}`. Running `solve.py` printed that test string, confirming that the ROP chain reached the target function in the local setup.
