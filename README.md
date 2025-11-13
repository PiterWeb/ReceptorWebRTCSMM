# ReceptorWebRTCSMM

## Gstreamer 

### Imagen Docker

- Imagen docker con Gstreamer (+ Plugins WebRTC):
https://hub.docker.com/r/livekit/gstreamer/tags

- Hacer pull:
  ```sh
  docker pull livekit/gstreamer:1.26.7-prod-rs
  ```
- Ejecutar bash sobre el contenedor docker:
  ```sh
    docker run --rm -it --entrypoint bash livekit/gstreamer:1.26.7-prod-rs 
  ```
- Ejecutar imagen docker lista para usar la red host:
  ```sh
    docker run --network=host --rm -it --entrypoint bash livekit/gstreamer:1.26.7-prod-rs
  ```

### WebRTC

- Recibir un stream webrtc y reproducirlo en ventana:
  ```sh
    gst-launch-1.0 whipserversrc signaller::host-addr=http://127.0.0.1:8190 ! videoconvert ! autovideosink
  ```
- Emisor de stream webrtc de prueba (sirve para probar el receptor simplemente):
  ```sh
    gst-launch-1.0 videotestsrc ! videoconvert ! video/x-raw ! queue ! whipclientsink name=ws signaller::whip-endpoint="http://127.0.0.1:8190/whip/endpoint"
  ```

#### Comandos pruebas Gstreamer

```sh
# Receptor webrtc que hace un stream rtp sencillo para probar desde fuera del docker que llega la información
gst-launch-1.0 whipserversrc signaller::host-addr=http://127.0.0.1:8190 ! videoconvert ! autovideosink ! video/x-raw,framerate=20/1 ! videoscale ! videoconvert ! x264enc tune=zerolatency bitrate=500 speed-preset=superfast ! rtph264pay ! udpsink host=127.0.0.1 port=5000

# Receptor rtp sencillo para leer el stream rtp generador por el receptor webrtc
gst-launch-1.0 -v udpsrc port=5000 caps = "application/x-rtp, media=(string)video, clock-rate=(int)90000, encoding-name=(string)H264, payload=(int)96" ! rtph264depay ! decodebin ! videoconvert ! autovideosink
```

<img width="370" height="327" alt="Captura desde 2025-11-13 18-43-55" src="https://github.com/user-attachments/assets/311d2755-f4eb-47d1-9e4d-2e94abcf7f64" />


#### Guias

- https://arunraghavan.net/2024/09/gstreamer-and-webrtc-http-signalling/
- https://gitlab.freedesktop.org/gstreamer/gst-plugins-rs/-/tree/main/net/webrtc#whip-server

## Referencias integración de WHIP

- https://webrtchacks.com/webrtc-plumbing-with-gstreamer/
- https://www.metered.ca/blog/webrtc-whip-whep-tutorial-build-a-live-streaming-app/
