FROM python:2.7-slim

ENV PLATFORMIOVER=3.4.0

RUN apt-get update \
    && apt-get install -y git \
    && pip install platformio==$PLATFORMIOVER \
    && platformio platform install espressif8266 \
    && platformio platform install teensy \
    && platformio settings set enable_telemetry No || 0

