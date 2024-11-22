# Blog Summary: Running Lightweight Kubernetes on AWS EC2 Instances with K3s

---

## **Overview**
K3s, a lightweight Kubernetes distribution by Rancher Labs, offers a resource-efficient alternative to standard Kubernetes. This blog explores how to deploy K3s on AWS EC2 instances, providing a streamlined solution for small-scale, edge, or cost-sensitive environments.

## **Key Highlights**

### What is K3s?
- **Lightweight Kubernetes**: Fully compliant, open-source Kubernetes distribution less than 100 MB in size.
- **Designed for Edge and IoT**: Ideal for lightweight, fast-starting environments.
- **K3d Support**: Can run inside a Docker container for even faster setup and minimal size (10 MB).

### What is Amazon EC2?
- **On-demand compute capacity**: AWS's Elastic Compute Cloud (EC2) offers scalable virtual servers for fast deployment.
- **Flexibility**: EC2 instances can scale up or down based on workload needs, optimizing cost-efficiency.

## **Deployment Steps**

### 1. Launch EC2 Instances:
- Use t2.micro instances for lightweight testing.
- Configure security groups to allow Kubernetes-related traffic (e.g., SSH, Kubernetes API server, Flannel VXLAN, kubelet, and node ports).

### 2. Install K3s:
- Install K3s on the master node with a single command:
  ```bash
  curl -sfL https://get.k3s.io | sh -


### Retrieve the node-token from the master node and install K3s on the worker node, pointing it to the master.

---

### Verify Cluster:
Ensure the master and worker nodes are registered and active in the K3s cluster:

### sudo k3s kubectl get nodes

---

### Deploy Applications:
Deploy an Nginx application and expose it via NodePort. Access the app using the master node’s public IP and assigned port.

---

### Remote Management:
Download the Kubeconfig file from the master node to your local machine for remote cluster management with kubectl.

---

### Clean Up:
Terminate EC2 instances to avoid unnecessary charges.

---

## **Conclusion:**
K3s is a lightweight, efficient Kubernetes distribution perfect for small-scale deployments on AWS EC2. By following this guide, developers can:
- Quickly deploy a Kubernetes cluster.
- Add worker nodes to scale their cluster.
- Test Kubernetes environments with minimal overhead.

K3s is ideal for cost-sensitive projects, edge computing, or scenarios requiring fast and straightforward Kubernetes setups.
