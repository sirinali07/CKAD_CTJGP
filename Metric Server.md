# ⚙️ Kubernetes Metrics Server - LAB Guide

> 💡 **Objective:**  
> This lab teaches you how to **install**, **configure**, and **verify** the **Metrics Server** in a Kubernetes cluster to monitor real-time **CPU and memory utilization** across nodes and pods.

---

## 📘 What is the Metrics Server?

The **Metrics Server** is a cluster-wide aggregator of resource usage data.  
It collects metrics like **CPU and Memory usage** from **Kubelets** running on each node and makes them available to the **Kubernetes API Server** through the **metrics.k8s.io API**.

### 🔍 Why Metrics Server is Important
- Enables commands like `kubectl top` to show live CPU and memory data.  
- Used internally by **Horizontal Pod Autoscaler (HPA)** and **Vertical Pod Autoscaler (VPA)** to make scaling decisions.  
- Lightweight, easy to deploy, and designed for short-term in-memory metrics (not a full monitoring solution like Prometheus).

📈 **In short:**  
> Metrics Server is the heart rate monitor 💓 of your Kubernetes cluster!

---

## 🧩 Task 1: Install Metrics Server

Apply the official Metrics Server manifest file:

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

## 🩺 Task 2: Verify the Deployment

Check whether Metrics Server pods are running properly in the kube-system namespace:

```bash
kubectl -n kube-system get po
```
You should see a pod named metrics-server-xxxx in the Running state.


## 🧱 Task 3: Download the Patch for Metrics Server

Some Kubernetes environments (especially self-managed clusters or cloud VMs) may require additional flags to make Metrics Server work correctly.
Download a pre-created patch file:

```bash
wget https://gist.githubusercontent.com/initcron/1a2bd25353e1faa22a0ad41ad1c01b62/raw/008e23f9fbf4d7e2cf79df1dd008de2f1db62a10/k8s-metrics-server.patch.yaml
```

## 🔧 Task 4: Apply the Patch

Apply the patch to the Metrics Server deployment to ensure proper API communication:

```bash
kubectl patch deploy metrics-server -p "$(cat k8s-metrics-server.patch.yaml)" -n kube-system
```

Recheck the pod status:

```bash
kubectl -n kube-system get po
```

## 📊 Task 5: View Resource Utilization

🔹 Node-Level Metrics

Check CPU and memory usage across all cluster nodes:

```bash
kubectl top node
```

🔹 Pod-Level Metrics

Check the usage by all running pods:

```bash
kubectl top pod
```

## 🧮 Task 6: Sort and Analyze Metrics
🔸 Sort by CPU Utilization

Find which pods consume the most CPU:

```bash
kubectl top pod -A --sort-by cpu
```
🔸 Sort by Memory Utilization

Find the pods consuming the most memory:

```bash
kubectl top pod -A --sort-by memory
```

## 📈 Task 7: Get Total Resource Utilization

Get a summary of CPU and memory utilization by all pods across namespaces:

```bash
kubectl top pod --sum=true -A
```

## 🧠 Task 8: Explore More Options

To explore all available flags and options for the kubectl top command:

```bash
kubectl top pod --help
```


