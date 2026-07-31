# method to setup containerd and kubernetes tools (kubeadm, kubelet, kubectl, kubernetes-cni)
def provision_cri_and_kubernetes_tools(vm)
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

Vagrant.configure("2") do |config|
  # Common configuration
  config.vm.box = "bento/ubuntu-22.04"
  config.vm.box_version = "202510.26.0"
  
  # control1
  config.vm.define "control1" do |control1|
    control1.vm.hostname= "control1"
    provision_cri_and_kubernetes_tools(control1)
  end

  # worker1
  config.vm.define "worker1" do |worker1|
    worker1.vm.hostname = "worker1"
    provision_cri_and_kubernetes_tools(worker1)
  end
  
  # worker2
  config.vm.define "worker2" do |worker2|
    worker2.vm.hostname = "worker2"
    provision_cri_and_kubernetes_tools(worker2)
  end
  
end
