### Run "Reddit-app in Kubernetes cluster

#### Preq:
- Install kubectl (https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- Install minikube (requires local Hypervisor, f.e. [Virtualbox](https://www.virtualbox.org/wiki/Downloads))
```
brew cask install minikube
```
Then:
- Run Minikube: ``` minikube start ```, Check it's running: ```kubectl get nodes```
- Run App:
```
kubectl apply -f kubernetes/ -n dev
```
Or... file by file:

```
kubectl apply -f ui-deployment.yml
kubectl apply -f ui-service.yml
kubectl apply -f post-deployment.yml
...
```
To check services are running can forward POD's port to a local machine:
```
kubectl get pods --selector component=ui
kubectl port-forward <pod-name> 8080:9292
```
Ui should be running on `http://localhost:8080`

- Services can be run in a different namespace:
```
kubectl apply -f dev-namespace.yml
kubectl apply -n dev -f kubernetes/
```