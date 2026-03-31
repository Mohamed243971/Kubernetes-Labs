# Kubernetes Lab Solution

## Task 1: Kubectl Config (5 points)

### a. Create a k3s Cluster (1 server + 1 agent)

Install k3s on the **server (control plane)**:
```bash
curl -sfL https://get.k3s.io | sh -
```

On the **agent (worker)** node, get the token from the server first:
```bash
# On server
cat /var/lib/rancher/k3s/server/node-token
```

Then join the agent:
```bash
# On worker
curl -sfL https://get.k3s.io | K3S_URL=https://<server-ip>:6443 K3S_TOKEN=<token> sh -
```

Verify the cluster:
```bash
kubectl get nodes
```

Output:
```
NAME           STATUS     ROLES          AGE   VERSION
k3s-worker-01  NotReady   <none>         24h   v1.34.6+k3s1
server         Ready      control-plane  24h   v1.34.6+k3s1
```

---

### b. Create a Namespace called `iti-46`

**`iti-namespace.yaml`:**
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: iti-46
```

Apply it:
```bash
kubectl apply -f iti-namespace.yaml
# namespace/iti-46 created
```

---

### c. Edit kubectl Config — Add `iti-context`

Edit `~/.kube/config` (or `/etc/rancher/k3s/k3s.yaml`) and add the new context:

**`kube-config.yaml`:**
```yaml
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: <CA_DATA>
    server: https://127.0.0.1:6443
  name: default
contexts:
- context:
    cluster: default
    user: default
  name: default
- context:
    cluster: default
    user: default
    namespace: iti-46
  name: iti-context
current-context: default
kind: Config
users:
- name: default
  user:
    client-certificate-data: <CERT_DATA>
    client-key-data: <KEY_DATA>
```

> **Note:** The new `iti-context` uses the `default` user and points to the `iti-46` namespace.

---

## Task 2: Kubectl Plugin (5 points)

### Create `kubectl hostnames` Plugin

Create the plugin file at `/usr/local/bin/kubectl-hostnames`:

```bash
vim /usr/local/bin/kubectl-hostnames
```

**File content:**
```bash
#!/bin/bash
kubectl get nodes -o json | jq '.items[].metadata.name'
```

Make it executable:
```bash
chmod +x /usr/local/bin/kubectl-hostnames
```

Run the plugin:
```bash
kubectl hostnames
```

Output:
```
"k3s-worker-01"
"server"
```

---

## Task 3: Creating Deployments (10 points)

### a & b. Deployment with 3 Replicas + ENV Variable

**`mydeployment.yaml`:**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mydeployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx-container
        image: nginx:alpine
        env:
        - name: FOO
          value: "ITI"
```

Apply the deployment:
```bash
kubectl apply -f mydeployment.yaml
# deployment.apps/mydeployment created
```

Verify:
```bash
kubectl get deployments
```

Output:
```
NAME           READY   UP-TO-DATE   AVAILABLE   AGE
mydeployment   3/3     3            3           43s
```

Confirm the `FOO` environment variable inside a pod:
```bash
kubectl exec <pod-name> -- env | grep FOO
# FOO=ITI
```

---

## Enable kubectl Autocompletion (Bonus)

```bash
echo 'source <(kubectl completion bash)' >> ~/.bashrc
source ~/.bashrc
```
