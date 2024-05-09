# [How to setup two-tier application deployment on kubernetes cluster](https://github.com/LondheShubham153/two-tier-flask-app/blob/master/k8s/README.md#how-to-setup-two-tier-application-deployment-on-kubernetes-cluster)

## [First setup kubernetes kubeadm cluster](https://github.com/LondheShubham153/two-tier-flask-app/blob/master/k8s/README.md#first-setup-kubernetes-kubeadm-cluster)

Use this repository to setup kubeadm [https://github.com/dushyantkumark/kubestarter/blob/main/kubeadm_installation.md]()

## [SetUp](https://github.com/LondheShubham153/two-tier-flask-app/blob/master/k8s/README.md#setup)

* First clone the code to your machine

```shell
https://github.com/dushyantkumark/two-tier-flask-app.gitgit
```

* Move to k8s directory

```shell
cd two-tier-flask-app/k8s
```

* Now, execute below commands one by one

```shell
kubectl apply -f twotier-deployment.yml
```

```shell
kubectl apply -f twotier-deployment-svc.yml
```

```shell
kubectl apply -f mysql-deployment.yml
```

```shell
kubectl apply -f mysql-deployment-svc.yml
```

```shell
kubectl apply -f persistent-volume.yml
```

```shell
kubectl apply -f persistent-volume-claim.yml
```
