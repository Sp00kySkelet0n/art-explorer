Vagrant.configure("2") do |config|
  config.vm.box = "perk/ubuntu-2204-arm64"
  config.vm.network "forwarded_port", guest: 5555, host: 5555
  config.vm.provision "file", source: "./id_rsa.pub", destination: "/tmp/id_rsa.pub"
  config.vm.provision "shell", inline: <<-SHELL
    cat /tmp/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys
  SHELL

  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y apt-transport-https ca-certificates curl gnupg lsb-release
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update
    sudo apt-get install -y docker-ce docker-ce-cli containerd.io
    sudo usermod -aG docker vagrant
    sudo apt-get install -y openjdk-17-jdk
  SHELL
  
  config.ssh.insert_key = false
  config.vm.provider "qemu" do |qe|
    qe.ssh_port = "50022"
  end
end