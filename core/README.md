# Fusepool P3 Platform Reference Implementation Docker Image

## Getting

To get the latest version from dockerhub:

    docker pull fusepoolp3/platform-reference-implementat

## Building

If instead you want to build it yourself, clone the github repository and run:

    docker build -t fusepoolp3/platform-reference-implementat .

## Running

### Using Marmotta LDP

To access the Fusepool P3 entry page at the default port 80 start the docker like that

    docker run --privileged -d --name=p3-platform -p 80:80 -p 8181:8181 -p 8151:8151 -p 8200:8200 -p 8201:8201 -p 8202:8202 -p 8203:8203 -p 8204:8204 -p 8205:8205 -p 8300:8300 -p 8301:8301 -p 8302:8302 -p 8303:8303 -p 8304:8304 -p 8305:8305 -p 8306:8306 -p 8307:8307 -p 8308:8308 -p 8310:8310 -p 8386:8386 -p 8387:8387 -p 8388:8388 fusepoolp3/platform-reference-implementat
    
### Using VOS LDP

The P3 platform uses Marmotta as the default LDP server. VOS (Virtuoso Open Source) provides an alternative LDP service. To use VOS as the LDP backend, start the platform using:

    docker run --privileged -d --name=p3-platform --link vos:vos --env LDPURI=http://vos:8890/ --env LDP_HOST=vos -p 80:80 --add-host {container-host-name}:{container-host-ip-address} -p 8181:8181 -p 8151:8151 -p 8200:8200 -p 8201:8201 -p 8202:8202 -p 8203:8203 -p 8204:8204 -p 8205:8205 -p 8300:8300 -p 8301:8301 -p 8302:8302 -p 8303:8303 -p 8304:8304 -p 8305:8305 -p 8306:8306 -p 8307:8307 -p 8308:8308 -p 8310:8310 -p 8386:8386 -p 8387:8387 -p 8388:8388  fusepoolp3/platform-reference-implementat

In this example, `vos` identifies a container hosting a VOS database instance, running on port 8890 (see [virtuoso-docker](https://github.com/fusepoolP3/virtuoso-docker) for details of  running VOS in Docker). When omitted, the environment variables `LDP_HOST` and `LDP_URI` default to `marmotta` and `http://localhost:8080/`. [ldp_config_vos.js](https://github.com/fusepoolP3/p3-platform-reference-implementation/blob/master/core/ldp_config_vos.js) and [ldp_config_marmotta.js](https://github.com/fusepoolP3/p3-platform-reference-implementation/blob/master/core/ldp_config_marmotta.js) specify the platform LDP root container and sparql endpoint paths. In the case of VOS, the LDP root container must be preconfigured in the database instance.

To stop the container:

    docker stop p3-platform

to restart the container:

    docker start p3-platform

Starting things can take quite a while, be patient. To see what's going on, use the following command:

    docker logs -f p3-platform

Attention: this docker container uses [docker-in-docker](https://github.com/jpetazzo/dind) which will mount the hosts' 
/var/lib/docker and use disk space there which won't be freed after docker rm.

## Using

When the container has finished starting, you can access the Fusepool P3 
platform directly on the host running the container. With a web browser 
access http://<yourhost>/. Important: The P3 Platform will autoconfigure itself 
with the hostname that it used to first access it, so if you intend the platform
to be accessible on http://example.com/ the first access must done using this URI
and *not* with http://localhost/, of course if you just want to try things out 
locally using /localhost/ is just fine.

The different components of Fusepool P3 
can be accessed over the ports exposed by the container. Curently, these are:

* 8181 - Fusepool's LDP over [p3-proxy](https://github.com/fusepoolP3/p3-proxy)
* 8151 - [p3-transformer-web-client](https://github.com/fusepoolP3/p3-transformer-web-client)
* 8200 - [p3-dashboard](https://github.com/fusepoolP3/p3-dashboard)
* 8201 - [p3-pipeline-gui-js](https://github.com/fusepoolP3/p3-pipeline-gui-js)
* 8202 - [p3-dictionary-matcher-factory-gui](https://github.com/fusepoolP3/p3-dictionary-matcher-factory-gui)
* 8203 - [p3-batchrefine-factory-gui](https://github.com/fusepoolP3/p3-batchrefine-factory-gui)
* 8204 - [p3-xslt-factory-gui](https://github.com/fusepoolP3/p3-xslt-factory-gui)
* 8205 - [p3-resource-gui](https://github.com/fusepoolP3/p3-resource-gui)
* 8300 - [p3-pipeline-transformer](https://github.com/fusepoolP3/p3-pipeline-transformer)
* 8301 - [p3-dictionary-matcher-transformer](https://github.com/fusepoolP3/p3-dictionary-matcher-transformer)
* 8302 - [p3-geo-enriching-transformer](https://github.com/fusepoolP3/p3-geo-enriching-transformer)
* 8303 - [p3-any23-transformer](https://github.com/fusepoolP3/p3-any23-transformer)
* 8305 - [p3-literal-extraction-transformer](https://github.com/fusepoolP3/p3-literal-extraction-transformer)
* 8306 - [p3-silkdedup](https://github.com/fusepoolP3/p3-silkdedup)
* 8307 - [p3-xslt-transformer](https://github.com/fusepoolP3/p3-xslt-transformer)
* 8308 - [p3-geocoordinates-transformer](https://github.com/fusepoolP3/p3-geocoordinates-transformer)
* 8310 - [p3-batchrefine](https://github.com/fusepoolP3/p3-batchrefine)
* 8386 - [p3-pundit-transformer](https://github.com/fusepoolP3/punditTransformer)
* 8387 - [Elasticsearch-Logstash-Kibana]
* 8388 - Real-time log monitoring with [Log.io](https://github.com/NarrativeScience/Log.io)
