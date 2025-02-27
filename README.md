Network Diagram FlexDash Widget for Node-RED
============================================

How-To using Docker
-------------------

- create a working directory, I'm using /home/flexdash
- cd into that directory
- git clone the flexdash sources: https://github.com/tve/flexdash
- git clone node-red-flexdash : https://github.com/flexdash/node-red-flexdash
- git clone network-diagram: : https://github.com/tve/node-red-fd-network-diagram
- cd flexdash/src
- npm install

   Otherwise you would get following error when starting the flow:
   
   ![image](https://user-images.githubusercontent.com/14224149/160299480-286c6eb7-883e-461c-92f8-fb52fa3c10f9.png)

- create a shell script to start Node-RED in docker, adjust paths and ports:
```
#! /usr/bin/env bash
SRC=/home/flexdash
mkdir -p network-diagram-data
docker run --rm -ti -p 1990:1880 -p 3000:3000 \
  -v $SRC/node-red-fd-network-diagram:/usr/src/node-red/node-red-fd-network-diagram \
  -v $SRC/node-red-flexdash:/usr/src/node-red/node-red-flexdash \
  -v $PWD/network-diagram-data:/data \
  -v $SRC/flexdash:/data/flexdash-src \
  --entrypoint bash \
  --name network-diagram \
  nodered/node-red:2.2.2 \
  -c "npm i ./node-red-flexdash ./node-red-fd-network-diagram; npm start --cache /data/.npm -- -v -userDir /data"
```
- launch the docker container by running the script
- open the flow editor at http://localhost:1990 and import the network-diagram simple example
- deploy
- open FlexDash at http://localhost:1990/flexdash-src, wait a minute for the dashboard to appear
- you should have a widget with a 3-node diagram
- in the flow editor, hit the set_network inject node, then hit the add_nodes inject node, observe the changes

Non-docker (untested)
---------------------

- git clone network-diagram: https://github.com/tve/node-red-fd-network-diagram
- install it in Node-RED (in the Node-RED dir run `npm install ../path/to/node-red-fd-network-diagram`)
- start Node-RED
- create a new flow and import the example flow from the network diagram pkg
- you will need to wait a while the first time while the dev server downloads flexdash and gets it running
- open FlexDash and proceed as above
