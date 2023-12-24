# k8s-playground

Kubernetes, Minikube, Helm, Terraform

## Install Minikube

To get a Kubernetes cluster up and running, I chose to install minikube on a local machine (an old windows laptop), which can be accessed from my dev machine (a Mac laptop) within my home intranet.

> Detail setup is available on [this blog](https://blog.jdriven.com/2020/10/minikube-on-lan/)

|            |                                    |
| ---------- | ---------------------------------- |
| OS         | Windows 10 Pro (build: 19045.3803) |
| RAM        | 8GB                                |
| Minikube   | 1.26.1                             |
| Kubernetes | 1.24.3                             |

Then start the Minikube cluster

```bash
minikube start --vm-driver hyperv --hyperv-virtual-switch "thinkpad-k8s" --addons=ingress --embed-certs

```
