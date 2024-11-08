---
id: PlatformIO
aliases:
  - PlatformIO Notes
tags: []
---

# PlatformIO Notes

## Intellisense for NeoVim

Incase the Intellisense is Not Working or Headers from System Libraries cannot be found, do the following:

Add this to your `platformio.ini`:
```ini
build_flags = -Ilib -Isrc
```
And then you need to run this command:
```bash
make clean && pio run -t compiledb
```

