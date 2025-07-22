# pssid-provisioning-umich
> Instructions for UMich specific user account creation and management
#### Ansible Bootstrapping - UMich Specific
> Follow the instructions in the [pSSID Daemon](https://github.com/UMNET-perfSONAR/pssid-daemon) repo for the rest of the provisioning.
1. Create a working directory that will hold the ansible repos and cd into that working directory
2. Bootstrapping playbook and corresponding role
```
git clone https://github.com/UMNET-perfSONAR/ansible-playbook-bootstrap.git 
cd ansible-playbook-bootstrap/roles 
git clone https://github.com/UMNET-perfSONAR/ansible-role-bootstrap-hardware.git
```
3. Install roles with `ansible-galaxy install -f -r requirements.yml --ignore-errors`
4. cd out of the playbook bootstrap directory (back into the working directory you created earlier), and clone the inventory and daemon repos
```
git clone https://github.com/UMNET-perfSONAR/ansible-inventory-pssid-ilab.git
git clone https://github.com/UMNET-perfSONAR/ansible-playbook-pssid-daemon.git
```
5. The resulting file tree (omitting some files and directories) should look something like this:
```
ansible-working/
├── ansible-inventory-pssid-ilab
├── ansible-playbook-bootstrap
│   └── roles
│       └── ansible-role-bootstrap-hardware
└── ansible-playbook-pssid-daemon
```
6. Edit the `hosts` files located at `ansible-inventory-pssid-ilab/inventory/hosts` and `ansible-playbook-pssid-daemon/inventory/hosts` to contain the IPs of your probes
7. cd into the `ansible-playbook-bootstrap` directory and run the following command (if you want to limit the hosts down to one IP on the list for bootstrapping, add the --limit XXX.XXX.XXX.XXX parameter after the inventory path)
```
ansible-playbook --ask-pass --ask-become-pass --user usernamehere --become --become-user root --become-method su --inventory ../ansible-inventory-pssid-ilab/inventory bootstrap.yml
```
7. cd into the `ansible-playbook-pssid-daemon` directory and run the following command to set up the daemon on each of the probes (NOTE: pscheduler will take a VERY long time to install, also sometimes cloning fails for no reason, just run it again)
```
ansible-playbook --ask-vault-pass --ask-pass --ask-become-pass --user usernamehere --become --become-user root --become-method su --inventory inventory/ playbook.yml
```
