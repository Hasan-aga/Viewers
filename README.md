# Getting started

to start the viewer we currently need node v16.20.0 (v18 is not working). we run
`yarn start` to start the viewer with the default config (test data comming from
https://server.dcmjs.org)

## using a local Pacs (Orthanc) for development

to run a development server with a local orthanc instance we do the following:

1.  `yarn orthanc:up` which runs this command
    `"docker-compose -f .docker/Nginx-Orthanc/docker-compose.yml up"`. which
    will start an orthanc instance at the 8042 port.

2.  `yarn dev:orthanc` which runs this command
    `cross-env NODE_ENV=development PROXY_TARGET=/dicom-web PROXY_DOMAIN=http://localhost:8042 APP_CONFIG=config/docker_nginx-orthanc.js webpack-dev-server --config .webpack/webpack.pwa.js`
    (this command is in this file:
    `/home/hasan/work/Viewers/platform/viewer/package.json`)
