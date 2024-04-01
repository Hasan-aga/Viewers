# Hevi demo
this demo uses an Ohif v3 image that is built and pushed to Hevi's Nexus repo. This makes the build process faster.

the demo consists of ohif and orthanc hidden behind nginx reverse proxy. the port on which ohif runs is determined by docker compose file and nginx conf. orthanc does not have its own port and instead is reachable via `/pacs-admin` endpoint.

## How to build
`docker compose up -d` should start the demo.

## development
if we wish to make changes to ohif, we can do so from ohif's repo then build an up-to-date image and use it in the demo.
