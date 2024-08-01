# Kubespray Cluster Setup with Ansible

This guide provides step-by-step instructions for setting up a Kubernetes cluster using Kubespray and Ansible.

## Prerequisites

- **Python 3** installed on your local machine
- **Virtual Environment (venv)** module for Python
- **Ansible** installed via `requirements.txt`

## Setup Instructions

### 1. Clone the Kubespray Repository

First, clone the Kubespray repository and navigate into it:

```bash
git clone https://github.com/kubernetes-sigs/kubespray.git
cd kubespray
```

### 2. Create and Activate a Virtual Environment

Create a virtual environment to isolate your Python dependencies:

```bash
python3 -m venv kubespray-venv
source kubespray-venv/bin/activate
```

### 3. Install Dependencies

Install the required Python packages using the provided `requirements.txt`:

```bash
pip install -U -r requirements.txt
```

### 4. Define Node IPs

Define the IP addresses of your EC2 instances:

```bash
declare -a IPS=(54.169.49.4 54.169.202.19 18.139.217.227)
```

### 5. Generate Inventory File

Use the `inventory_builder` script to create the `hosts.yaml` file:

```bash
CONFIG_FILE=inventory/mycluster/hosts.yaml python3 contrib/inventory_builder/inventory.py ${IPS[@]}
```

### 6. Update `hosts.yaml` File

Add the `ansible_user` and `ansible_ssh_private_key_file` fields to each host entry in the `hosts.yaml` file to enable access to the EC2 instances:

Example:
```yaml
all:
  hosts:
    node1:
      ansible_host: 54.169.49.4
      ip: 54.169.49.4
      access_ip: 54.169.49.4
      ansible_user: ubuntu
      ansible_ssh_private_key_file: /path/to/your/key.pem
    node2:
      ansible_host: 54.169.202.19
      ip: 54.169.202.19
      access_ip: 54.169.202.19
      ansible_user: ubuntu
      ansible_ssh_private_key_file: /path/to/your/key.pem
    node3:
      ansible_host: 18.139.217.227
      ip: 18.139.217.227
      access_ip: 18.139.217.227
      ansible_user: ubuntu
      ansible_ssh_private_key_file: /path/to/your/key.pem
```

### 7. Deploy the Kubernetes Cluster

Run the Ansible playbook to deploy the cluster:

```bash
ansible-playbook -i inventory/mycluster/hosts.yaml --become --become-user=root cluster.yml
```

## Additional Information

- Ensure your SSH key (`key.pem`) has the appropriate permissions and is accessible.
- Customize the `hosts.yaml` file as needed for your specific configuration and security requirements.
- Consult the [Kubespray documentation](https://kubespray.io/) for further customization options and troubleshooting.

---

This README provides a structured and detailed guide to setting up a Kubernetes cluster using Kubespray and Ansible. Make sure to replace the placeholder paths and IP addresses with your actual values.# kubespray-playground
