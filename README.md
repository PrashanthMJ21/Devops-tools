# Docker Installation 
```bash

sudo yum update -y
sudo yum search docker
sudo yum info docker
sudo yum install -y docker
sudo systemctl enable docker.service
sudo systemctl start docker.service
sudo systemctl status docker.service
docker --version
sudo yum install git -y
sudo curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

# Ansible Installation
## Master node 
```bash
curl -sL https://raw.githubusercontent.com/sakit333/ansible_insta/refs/heads/main/master_ansible_node.sh | bash
```
## Worker node
```bash
curl -sL https://raw.githubusercontent.com/sakit333/ansible_insta/refs/heads/main/worker_ansible_node.sh | bash
```

# Kubernetes's Installation

### Master node
```bash
curl -sL https://raw.githubusercontent.com/sakit333/kubernetes-v1.30.10-cluster-kubeadmdm/refs/heads/main/master_kube.sh | bash
```
### Worker node
```bash
curl -sL https://raw.githubusercontent.com/sakit333/kubernetes-v1.30.10-cluster-kubeadmdm/refs/heads/main/sak_worker_kube.sh | bash
```
