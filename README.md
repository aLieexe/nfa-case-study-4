# casestudy-jenkins

```
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt -y install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo usermod -aG docker alie
newgrp docker
```

Install java
```
curl -fsSL https://packages.adoptium.net/artifactory/api/gpg/key/public | sudo gpg --dearmor -o /usr/share/keyrings/adoptium.gpg
echo "deb [signed-by=/usr/share/keyrings/adoptium.gpg] https://packages.adoptium.net/artifactory/deb bookworm main" | sudo tee /etc/apt/sources.list.d/adoptium.list

sudo apt update
sudo apt install temurin-21-jdk
java -version

```

```

sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins

sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Kubectl Installation
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check


sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl


kubectl version --client
```


Minikube Installation
```
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64

minikube start
kubectl get po -A
```

Helm Install
```
sudo apt-get install curl gpg apt-transport-https --yes
curl -fsSL https://packages.buildkite.com/helm-linux/helm-debian/gpgkey | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/helm.gpg] https://packages.buildkite.com/helm-linux/helm-debian/any/ any main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```

```
sudo cp ~/.kube/config /var/lib/jenkins/kubeconfig 
sudo chown jenkins:jenkins /var/lib/jenkins/kubeconfig



# copy minikube certificates to jenkins accessible location
sudo mkdir -p /var/lib/jenkins/.minikube/profiles/minikube
sudo mkdir -p /var/lib/jenkins/.minikube

# copy certificates
sudo cp -r /home/alie/.minikube/ca.crt /var/lib/jenkins/.minikube/
sudo cp -r /home/alie/.minikube/profiles/minikube/client.crt /var/lib/jenkins/.minikube/profiles/minikube/
sudo cp -r /home/alie/.minikube/profiles/minikube/client.key /var/lib/jenkins/.minikube/profiles/minikube/

# fix ownership
sudo chown -R jenkins:jenkins /var/lib/jenkins/.minikube

# update kubeconfig paths
sudo sed -i 's|/home/alie/.minikube|/var/lib/jenkins/.minikube|g' /var/lib/jenkins/kubeconfig

```