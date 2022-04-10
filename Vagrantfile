# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<-SCRIPT
sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
sudo yum update -y
sudo yum install -y httpd
sudo systemctl enable httpd
sudo systemctl start httpd
sudo yum install git -y
cd / && cd /var/www
sudo rm -d html 
sudo git clone https://github.com/4linux/4542-site.git
sudo mv 4542-site html
SCRIPT

vms = {
  'webserver' => {'memory' => '2048', 'cpus' => 2, 'ip' => '150', 'box' => 'centos/8'}
}

Vagrant.configure("2") do |config|

  config.vm.box_check_update = false

  vms.each do |name, conf|
    config.vm.define "#{name}" do |k|
      k.vm.box = "#{conf['box']}"
      k.vm.hostname = "#{name}.4542-site.com"
      k.vm.network "private_network", ip: "172.27.11.#{conf['ip']}", virtualbox_intnet: true
       
      # k.vm.provider 'virtualbox' do |vb|
      #   vb.memory = conf['memory']
      #   vb.cpus = conf['cpus']
      # end
      k.vm.provision 'shell', inline: $script
    end
  end
end
