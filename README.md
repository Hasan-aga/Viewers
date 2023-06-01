# Getting started

make sure you have docker on your machine and that it is running.

to start the viewer we currently need node v16.20.0 (v18 is not working). a
simple way to switch between node versions is using `nvm use v16.20.0`.

we run `yarn start` to start the viewer with the default config (test data
comming from https://server.dcmjs.org)

## using a local Pacs (Orthanc) for development

to run a development server with a local orthanc instance we do the following:

1.  `yarn orthanc:up` which runs this command
    `"docker-compose -f .docker/Nginx-Orthanc/docker-compose.yml up"`. which
    will start an orthanc instance at the 8042 port.

2.  `yarn dev:orthanc` which runs this command
    `cross-env NODE_ENV=development PROXY_TARGET=/dicom-web PROXY_DOMAIN=http://localhost:8042 APP_CONFIG=config/docker_nginx-orthanc.js webpack-dev-server --config .webpack/webpack.pwa.js`
    (this command is in this file: `Viewers/platform/viewer/package.json`)

# building

we have an npm script called `start-ohif-orthanc` that we can use to do the
following:

`yarn build` will generate a `dist/` folder inside
`Viewers/platform/viewer/dist`

## building a container

we start with `yarn build` to produce an up-to-date `dist/` directory. note that
most recipes will copy the entire project and rebuild it but our recipe does not
do that as it is faster to simply copy `dist/`.

we navigate to `platform/viewer/.recipes` where we can find multiple
docker-compose configs.

we use `docker-compose up --build` to build the recipe. the build flag means we
always build from scratch instead of using cache.

## deployment

we must configure ohif so it connects to our orthanc on the deployment machine.
this is possible by editing the default config or by writing our own config
files and specifying them in APP_CONFIG env variable.

example working config:

```
window.config = {
  routerBasename: '/',
  showStudyList: true,
  servers: {
    dicomWeb: [
      {
        name: 'Orthanc',
        wadoUriRoot: '/pacs/wado',
        qidoRoot: '/pacs/dicom-web',
        wadoRoot: '/pacs/dicom-web',
        qidoSupportsIncludeField: false,
        imageRendering: 'wadors',
        thumbnailRendering: 'wadors',
        enableStudyLazyLoad: true,
        supportsFuzzyMatching: true,
        // requestOptions: {
        // undefined to use JWT + Bearer auth
        // auth: 'orthanc:orthanc',
        // },
      },
    ],
  },
};

```

> **note how we use relative links instead of specifying an IP address or a
> domain (or localhost).**

## general notes for development

editing nginx.conf of the dockerfile will not take effect entirely unless we
perform a restart of the development machine. there may be an easier way but as
of this writing I have no other alternative than a full restart.
