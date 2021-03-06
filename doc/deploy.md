# How to Deploy bitstashinfo

bitstashinfo is splitted into 3 repos:
* [https://github.com/bitstashco/bitstashinfo-api](https://github.com/bitstashco/bitstashinfo-api)
* [https://github.com/bitstashco/bitstashinfo-api](https://github.com/bitstashco/bitstashinfo-api)
* [https://github.com/bitstashco/bitstashinfo-ui](https://github.com/bitstashco/bitstashinfo-ui)

## Prerequisites

* node.js v12.0+
* mysql v8.0+
* redis v5.0+

## Deploy bitstash core
1. `git clone --recursive https://github.com/bitstashproject/bitstash.git --branch=bitstashinfo`
2. Follow the instructions of [https://github.com/bitstashco/bitstash/blob/master/README.md#building-bitstash-core](https://github.com/bitstashco/bitstash/blob/master/README.md#building-bitstash-core) to build bitstash
3. Run `bitstashd` with `-logevents=1` enabled

## Deploy bitstashinfo
1. `git clone https://github.com/bitstashproject/bitstashinfo.git`
2. `cd bitstashinfo && npm install`
3. Create a mysql database and import [docs/structure.sql](structure.sql)
4. Edit file `bitstashinfo-node.json` and change the configurations if needed.
5. `npm run dev`

It is strongly recommended to run `bitstashinfo` under a process manager (like `pm2`), to restart the process when `bitstashinfo` crashes.

## Deploy bitstashinfo-api
1. `git clone https://github.com/bitstashproject/bitstashinfo-api.git`
2. `cd bitstashinfo-api && npm install`
3. Create file `config/config.prod.js`, write your configurations into `config/config.prod.js` such as:
    ```javascript
    exports.security = {
        domainWhiteList: ['http://example.com']  // CORS whitelist sites
    }
    // or
    exports.cors = {
        origin: '*'  // Access-Control-Allow-Origin: *
    }

    exports.sequelize = {
        logging: false  // disable sql logging
    }
    ```
    This will override corresponding field in `config/config.default.js` while running.
4. `npm start`

## Deploy bitstashinfo-ui
This repo is optional, you may not deploy it if you don't need UI.
1. `git clone https://github.com/bitstashproject/bitstashinfo-ui.git`
2. `cd bitstashinfo-ui && npm install`
3. Edit `package.json` for example:
   * Edit `script.build` to `"build": "BitstashINFO_API_BASE_CLIENT=/api/ BitstashINFO_API_BASE_SERVER=http://localhost:3001/ BitstashINFO_API_BASE_WS=//example.com/ nuxt build"` in `package.json` to set the api URL base
   * Edit `script.start` to `"start": "PORT=3000 nuxt start"` to run `bitstashinfo-ui` on port 3000
4. `npm run build`
5. `npm start`
