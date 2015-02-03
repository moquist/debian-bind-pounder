VAGRANTFILE_API_VERSION = "2"

Vagrant.require_version ">= 1.6.3"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define "debian-bind-pounder"

  config.vm.box = "yungsang/boot2docker"

  config.vm.synced_folder ".", "/vagrant"

  # This makes --provision indempotent.
  config.vm.provision "shell", inline: <<-SHELL
    docker stop bind || true
    docker rm bind || true
  SHELL

  config.vm.provision :docker do |d|
    d.build_image "/vagrant/vmfiles",
      args: "-t moquist/debian-bind-pounder"
    d.run "bind",
      image: "moquist/debian-bind-pounder",
      args: "-p 60053:60053/udp -v /vagrant/vmfiles/bind-conf:/opt/bind-conf:ro",
      cmd: "/usr/sbin/named -c /opt/bind-conf/named.conf -g"
  end

  config.vm.network :forwarded_port, guest: 2375, host: 62375
  config.vm.network :forwarded_port, guest: 60053, host: 60053, protocol: 'udp'
end
