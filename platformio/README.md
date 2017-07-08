To build:

```bash
docker run -it --rm -v $(pwd):/src mariolopjr/pio
```

To upload
```bash
docker run -it --rm -v $(pwd):/src --device=/dev/  mariolopjr/pio --target upload
```

To build & flash SPIFFS
```bash
docker run -it --rm -v $(pwd):/src mariolopjr/pio --target buildfs && pio run --target uploadfs
```

