### Step 1: Install Ansible

On the control machine:

```bash
sudo apt update
sudo apt install ansible
ansible --version
```

### Step 2: Run the Initial Setup with Ansible

This command prepares **all nodes**:

```bash
ansible-playbook -i inventory playbook.yaml --ask-become-pass -vvv
```

### Step 3: Manually Initialize the Cluster (Master Node)

On the control-plane node:

```bash
sudo kubeadm reset
sudo kubeadm init
```

After initialization, configure `kubectl` access:

```bash
mkdir -p$HOME/.kube
sudocp /etc/kubernetes/admin.conf$HOME/.kube/config
sudochown $(id -u):$(id -g)$HOME/.kube/config
```

Then copy the **`kubeadm join` command** and run it on each worker node.

### Step 4: Install CNI and MetalLB with Ansible

Once the cluster is up and all nodes have joined:

```bash
ansible-playbook -i inventory sites.yaml --ask-become-pass -vvv
```

This final step:

- Enables Pod networking
- Adds LoadBalancer capability
- Completes the cluster setup
