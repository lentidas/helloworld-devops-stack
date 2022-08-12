# devops-helloworld-stack

A customized Nginx container to provide a static web page used to verify that everything is up after provisioning our [DevOps Stack](https://devops-stack.io).

The provided Dockerfile copies the static page on [`devops-stack-helloworld`](devops-stack-helloworld) to the folder `/usr/share/nginx/html` inside the container. The Nginx configuration file [`metrics.conf`](nginx_confs/metrics.conf) exposes a web page containing the metrics that an exporter can send to Prometheus (like in our use-case [here](https://github.com/camptocamp/devops-stack-helloworld-templates)).

This container expects to have the [`hyperlinks.js`](devops-stack-helloworld/assets/js/hyperlinks.js) file overloaded during the deployment of the container, usually using a [ConfigMap](https://github.com/camptocamp/devops-stack-helloworld-templates/blob/main/apps/helloworld/templates/helloworld_hyperlinks_configmap.yaml).

To test this web site locally, simply create an `hyperlink.js` (you can use the content from this [ConfigMap](https://github.com/camptocamp/devops-stack-helloworld-templates/blob/main/apps/helloworld/templates/helloworld_hyperlinks_configmap.yaml)) and then run this command on the same folder:

```docker
docker run --publish 8080:80 --mount type=bind,source="$(pwd)"/hyperlinks.js,target=/usr/share/nginx/html/assets/js/hyperlinks.js,readonly ghcr.io/camptocamp/devops-stack-helloworld:latest
```

You can then visit https://localhost:8080 to view the web site and if everything is good you can then kill the container and then delete `hyperlink.js` you created before.
