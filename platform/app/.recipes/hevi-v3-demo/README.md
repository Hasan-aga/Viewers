# Hevi demo
this demo uses an Ohif v3 image that is built and pushed to Hevi's Nexus repo.

the demo consists of ohif and orthanc hidden behind nginx reverse proxy. the port on which ohif runs is determined by docker compose file and nginx conf. orthanc does not have its own port and instead is reachable via /pacs-admin endpoint.

## How to build
from hevi-prostate repo we checkout demo-v3-fixed branch then we navigate to platform/app/.recipes/hevi-v3-demo

docker compose up -d starts the demo.

in the above case, docker compose will use and existing image, if you want to build a new one continue reading

## building new ohif image
if you want to update ohif, first go to the github repo and click the “sync fork” button, then pull from master branch on your machine.

then to build the demo with the new image cd into platform/app/.recipes/hevi-v3-demo/docker-compose-build.yml and use:



docker compose up --build
if you want to give the new image a new name you can do so from the docker-compose-build.yml file.

## deployment
deploying the demo is two steps:

start the demo app as explained above.

using nginx reverse proxy point the demo link to the demo app. the nginx machine is 10.2.0.4 and the nginx config is /etc/nginx/sites-enabled/v3-demo-suite.hevi.ai (heviai/nginx-configs ) this config points http://v3-demo-suite.hevi.ai to the app running on /media/ssd/tools/hevi-demo-v3 http://10.2.3.4:3001

development
if we wish to make changes to ohif, we can do so from ohif's repo then build an up-to-date image and use it in the demo.
