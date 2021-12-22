# Custom project from start.vaadin.com

This project can be used as a starting point to create your own Vaadin application with Spring Boot.
It contains all the necessary configuration and some placeholder files to get you started.

## Running the application

The project is a standard Maven project. To run it from the command line,
type `mvnw` (Windows), or `./mvnw` (Mac & Linux), then open
http://localhost:8080 in your browser.

You can also import the project to your IDE of choice as you would with any
Maven project. Read more on [how to import Vaadin projects to different 
IDEs](https://vaadin.com/docs/latest/flow/guide/step-by-step/importing) (Eclipse, IntelliJ IDEA, NetBeans, and VS Code).

## Deploying to Production

To create a production build, call `mvnw clean package -Pproduction` (Windows),
or `./mvnw clean package -Pproduction` (Mac & Linux).
This will build a JAR file with all the dependencies and front-end resources,
ready to be deployed. The file can be found in the `target` folder after the build completes.

Once the JAR file is built, you can run it using
`java -jar target/myapp-1.0-SNAPSHOT.jar` (NOTE, replace
`myapp-1.0-SNAPSHOT.jar` with the name of your jar).

## Project structure

<table style="width:100%; text-align: left;">
  <tr><th>Directory</th><th>Description</th></tr>
  <tr><td><code>frontend/</code></td><td>Client-side source directory</td></tr>
  <tr><td>&nbsp;&nbsp;&nbsp;&nbsp;<code>index.html</code></td><td>HTML template</td></tr>
  <tr><td>&nbsp;&nbsp;&nbsp;&nbsp;<code>index.ts</code></td><td>Frontend entrypoint, contains the client-side routing setup using <a href="https://vaadin.com/router">Vaadin Router</a></td></tr>
  <tr><td>&nbsp;&nbsp;&nbsp;&nbsp;<code>main-layout.ts</code></td><td>Main layout Web Component, contains the navigation menu, uses <a href="https://vaadin.com/components/vaadin-app-layout">App Layout</a></td></tr>
  <tr><td>&nbsp;&nbsp;&nbsp;&nbsp;<code>views/</code></td><td>UI views Web Components (TypeScript)</td></tr>
  <tr><td>&nbsp;&nbsp;&nbsp;&nbsp;<code>themes/</code></td><td>Custom  
CSS styles</td></tr>
  <tr><td><code>src/main/java/&lt;groupId&gt;/</code></td><td>Server-side 
source directory, contains the server-side Java views</td></tr>
  <tr><td>&nbsp;&nbsp;&nbsp;&nbsp;<code>Application.java</code></td><td>Server entry-point</td></tr>
</table>

## Useful links

- Read the documentation at [vaadin.com/docs](https://vaadin.com/docs/latest/).
- Create new projects at [start.vaadin.com](https://start.vaadin.com/).
- Search UI components and their usage examples at [vaadin.com/components](https://vaadin.com/components).
- View use case applications that demonstrate Vaadin capabilities at [vaadin.com/examples-and-demos](https://vaadin.com/examples-and-demos).
- Discover Vaadin's set of CSS utility classes that enable building any UI without custom CSS in the [docs](https://vaadin.com/docs/latest/ds/foundation/utility-classes).
- Find a collection of solutions to common use cases in [Vaadin Cookbook](https://cookbook.vaadin.com/).
- Find Add-ons at [vaadin.com/directory](https://vaadin.com/directory).
- Ask questions on [Stack Overflow](https://stackoverflow.com/questions/tagged/vaadin) or join our [Discord channel](https://discord.gg/MYFq5RTbBn).
- Report issues, create pull requests in [GitHub](https://github.com/vaadin/platform).


## Deploying using Docker

To build the Dockerized version of the project, run

```
docker build . -t myapp:latest
```

Once the Docker image is correctly built, you can test it locally using

```
docker run -p 8080:8080 myapp:latest
```


## Deploying using Kubernetes

We assume here that you have the Kubernetes cluster from Docker Desktop running (can be enabled in the settings).

First build the Docker image for your application. You then need to make the Docker image available to you cluster. With Docker Desktop Kubernetes, this happens automatically. With Minikube, you can run `eval $(minikube docker-env)` and then build the image to make it available. For other clusters, you need to publish to a Docker repository or check the documentation for the cluster.

The included `kubernetes.yaml` sets up a deployment with 2 pods (server instances) and a load balancer service. You can deploy the application on a Kubernetes cluster using

```
kubectl apply -f kubernetes.yaml
```

If everything works, you can access your application by opening http://localhost:8000/.
If you have something else running on port 8000, you need to change the load balancer port in `kubernetes.yaml`.

Tip: If you want to understand which pod your requests go to, you can add the value of `VaadinServletRequest.getCurrent().getLocalAddr()` somewhere in your UI.

### Troubleshooting

If something is not working, you can try one of the following commands to see what is deployed and their status.

```
kubectl get pods
kubectl get services
kubectl get deployments
```

If the pods say `Container image "myapp:latest" is not present with pull policy of Never` then you have not built your application using Docker or there is a mismatch in the name. Use `docker images ls` to see which images are available.

If you need even more information, you can run

```
kubectl cluster-info dump
```

that will probably give you too much information but might reveal the cause of a problem.

If you want to remove your whole deployment and start over, run

```
kubectl delete -f kubernetes.yaml
```
