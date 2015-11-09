require 'socket'

Vagrant.configure("2") do |config|
  config.ssh.username = "ubuntu"
  config.ssh.forward_agent = true

  nm_guest = Socket.gethostname.split('.')[0]
  config.vm.define nm_guest do
  end

  if ENV['REGION']
    config.vm.box = "ubuntu-#{ENV['REGION']}"
  else
    config.vm.box = "ubuntu"
  end
  config.vm.synced_folder '.', '/vagrant', disabled: true
  
  config.vm.provider "virtualbox" do |v, override|
    override.vm.synced_folder '.', '/vagrant'
    v.memory = 2048
    v.cpus = 2
  end

  config.vm.provider "digital_ocean" do |v, override|
    ssh_key = "#{ENV['HOME']}/.ssh/vagrant-#{ENV['LOGNAME']}"
    override.ssh.private_key_path = ssh_key
    v.ssh_key_name = "vagrant-#{Digest::MD5.file(ssh_key).hexdigest}"
    v.token = ENV['DIGITALOCEAN_API_TOKEN']
    v.size = '2gb'
    v.setup = false
    v.user_data = "#cloud-config\n\nruncmd:\n  - install -d -o ubuntu -g ubuntu -m 0700 ~ubuntu/.ssh && install -o ubuntu -g ubuntu -m 0600 ~root/.ssh/authorized_keys ~ubuntu/.ssh/authorized_keys"
  end
end
