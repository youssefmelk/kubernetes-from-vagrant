IMAGE = "bento/ubuntu-22.04"
IMAGE_VERSION = "202510.26.0"
CLUSTER_CONFIG = {
  "control1" => { :ip => "192.168.56.10", :cpus => 2, :mem => 2048 },
  "worker1" => { :ip => "192.168.56.11", :cpus => 1, :mem => 2048 },
  "worker2" => { :ip => "192.168.56.12", :cpus => 1, :mem => 2048 }
}

# method to setup containerd and kubernetes tools (kubeadm, kubelet, kubectl, kubernetes-cni)
def provision_cri_and_provision_kubernetes_tools(vm)
  vm.vm.provision "shell", inline: <<-'SCRIPT'
    echo "Configuring Linux Kernel Settings..."
    sudo cp /vagrant/files/99-kubernetes-cri.conf \
        /etc/sysctl.d/99-kubernetes-cri.conf
    sudo sysctl --system

    echo "Refreshing system's local database of available software..."
    sudo apt update
    
    echo "Installing jq..."
    sudo apt-get install -y jq
    
    mkdir -p /home/vagrant/github
    
    echo "Pulling the cka GitHub repository..."
    git clone https://github.com/sandervanvugt/cka.git /home/vagrant/github/cka
    
    echo "Installing the CRI..."
    sudo /home/vagrant/github/cka/setup-container.sh
    
    echo "Installing the tools of the SECOND-LATEST version of Kubernetes (kubeadm, kubelet, kubectl, kubernetes-cni)..."
    sudo /home/vagrant/github/cka/setup-kubetools-previousversion.sh
  SCRIPT
end

def install_cri_tools(vm)
  vm.vm.provision "shell", inline: <<-'SCRIPT'
    echo "Installing cri-tools for crictl..."
    sudo apt install -y cri-tools

    sudo cp /home/vagrant/github/cka/crictl.yaml /etc/crictl.yaml
  SCRIPT
end

def install_etcd_client(vm)
  vm.vm.provision "shell", inline: <<-'SCRIPT'
    echo "Installing etcd-client for etcdctl..."
    sudo apt-get install -y etcd-client
  SCRIPT

Vagrant.configure("2") do |config|
  # Common configuration
  CLUSTER_CONFIG.each do |name, properties|
    config.vm.define name do |node|
      node.vm.box = IMAGE
      node.vm.box_version = IMAGE_VERSION
      node.vm.hostname= name

      # Assign static IP address to each VM
      node.vm.network "private_network", ip: properties[:ip]

      # Hardware resource allocation
      node.vm.provider "virtualbox" do |vb|
        vb.memory = properties[:mem]
        vb.cpus = properties[:cpus]
      end

      # Install CRI and K8s tools on each node
      provision_cri_and_provision_kubernetes_tools(node)
      install_cri_tools(node)
    end
  end

end
