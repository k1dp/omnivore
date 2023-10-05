FROM willnorris/imageproxy:v0.10.0@sha256:444bddf1060bae4f5f0183879501a50407ff1215286bcbadb0707df805c6b564 as build

# Above imageproxy image is built from scratch image and is barebones
# Switching over to ubuntu base image to allow us to debug better.
FROM ubuntu:bionic@sha256:152dc042452c496007f07ca9127571cb9c29697f42acbfad72324b2bb2e43c98

WORKDIR /app

RUN apt update && apt install -y ca-certificates

COPY --from=build /app/imageproxy .

COPY start_imageproxy.sh .

# Using a custom script to customize imageproxy startup and to pass
# signatureKey through env variable
ENTRYPOINT ["/app/start_imageproxy.sh"]

EXPOSE 8080

