Vagrant.configure("2") do |config|
  config.vm.box = "perk/ubuntu-2204-arm64"
  # Use a local SSH key for authentication
  config.vm.provision "file", source: "./id_rsa.pub", destination: "/tmp/id_rsa.pub"
  config.vm.provision "shell", inline: <<-SHELL
    cat /tmp/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys
  SHELL

  config.vm.provision "shell", inline: <<-SHELL
    # Update package list
    sudo apt-get update

    # Install dependencies for Docker
    sudo apt-get install -y apt-transport-https ca-certificates curl gnupg lsb-release

    # Add Docker’s official GPG key and repository
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

    # Install Docker Engine
    sudo apt-get update
    sudo apt-get install -y docker-ce docker-ce-cli containerd.io

    # Add vagrant user to docker group
    sudo usermod -aG docker vagrant

    # Install Java (OpenJDK 17)
    sudo apt-get install -y openjdk-17-jdk
  SHELL
  config.ssh.insert_key = false
  config.vm.provider "qemu" do |qe|
    qe.ssh_port = "50022"
  end
end