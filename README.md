
# Using a ConfigMap to store configuration

ConfigMaps and Secrets are used to store configuration information separate from the code so that nothing is hardcoded. It also lets the application pick up configuration changes without needing to be redeployed. To demonstrate this, we’ll store the application’s message for a simple javascript code in `app.js` in a ConfigMap so that the message can be updated simply by updating the ConfigMap.

* Create a ConfigMap that contains a new message.
```bash
  kubectl create configmap app-config --from-literal=MESSAGE="This message came from a ConfigMap!"
 ```
* Use the Explorer to edit `deployment-configmap-env-var.yaml`.You need to change it to the docker image that will be created and pushed in the later part of the lab. Make sure to save the file when you’re done.
* Use the Explorer to open the `app.js` file. Notice the line **res.send(process.env.MESSAGE + '\n')**
* Build and push a new image that contains your new application code.
 ```bash
  docker build -t your-dockerhub-username/your-repo-name:tag . && docker push your-dockerhub-username/your-repo-name:tag
 ```
* Apply the new Deployment configuration.
 ```bash
  kubectl apply -f deployment-configmap-env-var.yaml
  ```
* In order to access the application, we have to expose it to the internet via a Kubernetes Service. Cluster IPs are only accesible within the cluster. To make this externally accessible, we will create a port forwarding.
 ```bash
  kubectl expose deployment/hello-world
  kubectl get pods
  kubectl port-forward pod/pod-name 8081:8080

```
* Go back to your original terminal window, ping the application to get a response.
  > NOTE: Do not close the terminal window where the *port-forward* command is still running.
```bash
  curl http://localhost:8081
  ```
  You see the message, “This message came from a ConfigMap!”

