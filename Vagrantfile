Vagrant.configure('2') do |config|
  config.vm.box = 'mjanser/fedora25-64-lxc'

  config.vm.define 'owncloud-fedora-25' do | vmconfig |
    vmconfig.vm.hostname = 'owncloud-fedora-25'
    vmconfig.vm.network 'forwarded_port', guest: 443, host: 8081
  end

  config.vm.define 'nextcloud-fedora-25' do | vmconfig |
    vmconfig.vm.hostname = 'nextcloud-fedora-25'
    vmconfig.vm.network 'forwarded_port', guest: 443, host: 8082
  end

  config.vm.provision 'ansible_local' do |ansible|
    ansible.playbook = 'playbook.yml'
    ansible.groups = {
        'owncloud' => [
            'owncloud-fedora-25',
        ],
        'owncloud:vars' => {'owncloud_vendor' => 'owncloud'},
        'nextcloud' => [
            'nextcloud-fedora-25',
        ],
        'nextcloud:vars' => {'owncloud_vendor' => 'nextcloud'},
    }
    ansible.sudo = true
  end

  config.vm.provision 'shell' do |s|
    s.keep_color = true
    s.inline = <<SCRIPT
VENDOR=$(hostname | cut -d- -f1)

echo "specified vendor: $VENDOR"

curl -s --insecure -H "Host: example.com" https://localhost/login | grep -qi "$VENDOR" && echo "curl request for vendor was successful" || { echo "curl request for vendor failed" && exit 1; }
curl -s --insecure -H "Host: example.com" https://localhost/login | grep -qi "username" && echo "curl request for login was successful" || { echo "curl request for login failed" && exit 1; }

mkdir /var/www/$VENDOR/.well-known
echo foo > /var/www/$VENDOR/.well-known/foo.txt
curl -s --max-redirs 0 -H "Host: example.com" http://localhost/.well-known/foo.txt | grep -q "foo" && echo "curl request for well-known was successful" || { echo "curl request for well-known failed" && exit 1; }

cd /vagrant/
ansible-playbook playbook.yml --limit $(hostname) --inventory-file /tmp/vagrant-ansible/inventory/vagrant_ansible_local_inventory 2>&1 | tee /tmp/ansible.log

# Remove colors from log file
sed -i -r "s/\\x1B\\[([0-9]{1,2}(;[0-9]{1,2})?)?[m|K]//g" /tmp/ansible.log

# Test for errors
test -n "$(grep -L 'ERROR' /tmp/ansible.log)" \
    && { echo "Errors test: pass"; } \
    || { echo "Errors test: fail" && exit 1; }

# Test for warnings
test -n "$(grep -L 'WARNING' /tmp/ansible.log)" \
    && { echo "Warnings test: pass"; } \
    || { echo "Warnings test: fail" && exit 1; }

# Test for idempotence
grep -q "changed=0.*failed=0" /tmp/ansible.log \
    && { echo "Idempotence test: pass"; } \
    || { echo "Idempotence test: fail" && exit 1; }
SCRIPT
  end
end
