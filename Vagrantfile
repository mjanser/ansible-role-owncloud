Vagrant.configure('2') do |config|
  config.vm.box = 'obnox/fedora23-64-lxc'

  config.vm.synced_folder '.', '/vagrant', disabled: true
  config.vm.synced_folder '.', '/vagrant/ansible-role-owncloud'

  config.vm.define 'fedora-23' do | vmconfig |
    vmconfig.vm.hostname = 'ansible-role-owncloud-fedora-23'
    vmconfig.vm.network "forwarded_port", guest: 443, host: 8080

    vmconfig.vm.provision 'shell' do |s|
      s.keep_color = true
      s.path = 'tests/init.sh'
    end
  end
end
