$ansible_bootstrap = <<SCRIPT
sudo yum install -y epel-release
sudo yum install -y ansible
cp /vagrant/ssh/ansible_id_rsa /home/vagrant/.ssh/id_rsa
cp /vagrant/ssh/ansible_id_rsa.pub /home/vagrant/.ssh/id_rsa.pub
sudo sed -i s/#host_key_checking/host_key_checking/g /etc/ansible/ansible.cfg
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "bento/centos-7.1"

  config.vm.define "ansible" do |ansible|
    ansible.vm.hostname = "ansible"
    ansible.vm.network "private_network", ip: "192.0.3.4"
    ansible.vm.provision "shell", inline: $ansible_bootstrap
  end

    (1..3).each do |i|
     config.vm.define "web#{i}" do |web|
     web.vm.hostname = "web#{i}"
     web.vm.network "private_network", ip: "192.0.3.10#{i}"
     web.vm.provision "shell", inline: "cat /vagrant/ssh/ansible_id_rsa.pub >> /home/vagrant/.ssh/authorized_keys"
     end
    end

end
