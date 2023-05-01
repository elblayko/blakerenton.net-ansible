# About

Ansible configuration files to do the following tasks for my website:
- Configure timezone
- Configure UFW
- Configure Fail2Ban
- Configure SSH server
- Create user accounts
- Update system packages
- Install Docker
- Install Docker Compose
- Install Elastic Agent
- Deploy a web application
- Update TLS certificates

# Installation

1. Build the Docker image.
```powershell
docker build -t ansible:0 .
```

2. Create configuration files from templates.
```powershell
Copy-Item -Path .\hosts\hosts.reference.yml -Destination .\hosts\hosts.yml
Copy-Item -Path .\vars\encrypted_vars.reference.yml -Destination .\vars\encrypted_vars.yml
```

3. Copy SSH keys to the keys directory.
```powershell
Copy-Item -Path C:\Users\Blake\.ssh\id_rsa -Destination .\keys\id_rsa
```

4. Modify variables within `hosts`, `vars.yml`, and `encrypted_vars.yml` as needed.

5. Run the Docker container mounting the playbooks, hosts, keys, and vars directories.
```powershell
docker run -it --rm `
    -v C:\Users\Blake\Desktop\Projects\blakerenton.net-ansible\playbooks:/ansible/playbooks `
    -v C:\Users\Blake\Desktop\Projects\blakerenton.net-ansible\hosts\hosts.yml:/etc/ansible/hosts `
    -v C:\Users\Blake\Desktop\Projects\blakerenton.net-ansible\keys:/home/ansible/.ssh `
    -v C:\Users\Blake\Desktop\Projects\blakerenton.net-ansible\vars:/ansible/vars `
    ansible:0 ansible-playbook main.yml
```

6. Encrypt the encrypted variables file.
```powershell
ansible-vault encrypt /ansible/vars/encrypted_vars.yml
```

7. Run the playbooks.
```powershell
ansible-playbook /ansible/playbooks/main.yml --ask-vault-pass
```
