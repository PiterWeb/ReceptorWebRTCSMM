# ReceptorWebRTCSMM

## Gstreamer 

- Imagen docker con Gstreamer (+ Plugins WebRTC):
https://hub.docker.com/r/livekit/gstreamer/tags

- Hacer pull:
  docker pull livekit/gstreamer:1.26.7-prod-rs
- Ejecutar bash sobre el contenedor docker:
  docker run --rm -it --entrypoint bash livekit/gstreamer:1.26.7-prod-rs 

## Referencias integración de WHIP

- https://webrtchacks.com/webrtc-plumbing-with-gstreamer/
- https://www.metered.ca/blog/webrtc-whip-whep-tutorial-build-a-live-streaming-app/
