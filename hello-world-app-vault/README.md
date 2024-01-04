# Summary

This is a demo of fetching secrets stored in Hashicorp Vault, from a Kubernetes Pod.

When a Kubernetes Pod is created:

- A JWT token will be created for the service account that is used by the Pod
- The Vault agent uses this token to authenticate with Vault
- Underneath, Vault verify the token via Kubernetes TokenReview API
- Once verified, Vault server returns a token to Vault agent, which then saves the token to local file system (sink)
- With this token, Vault agent fetches secrets from Vault server and save them to local file system `/vault/secrets/xxxx`

## Setup

1. A Minikube cluster running on another machine with the LAN

   ```bash
   minikube start --vm-driver hyperv --hyperv-virtual-switch "thinkpad-k8s" --addons=ingress
   ```

   This will start a K8s cluster accessible by static IP `192.168.xx.xx`

1. A Vault server running on local machine inside a docker container

   ```bash
   docker run --cap-add=IPC_LOCK -e 'VAULT_DEV_ROOT_TOKEN_ID=myroot' -p 8200:8200 hashicorp/vault
   ```

   Log in Vault (http://127.0.0.1:8200) and create a key-value secret under `secret/hello-world`

## Steps

1. Install Vault agent injector

   ```bash
   helm install vault hashicorp/vault --set "global.externalVaultAddr=http://192.168.123.172:8200"
   ```

   This will spin up a new pod in K8s cluster

1. Config Vault auth method (Kubernetes), role and policy

   - K8s host
   - K8s CA cert

1. Create service account, cluster role and role binding

   ```bash
   kubectl apply -f sa.yaml
   kubectl apply -f cluster-role.yaml
   kubectl apply -f cluster-role-binding.yaml
   ```

1. Deploy the demo app

   ```bash
   kubectl apply -f deployment.yaml
   ```
