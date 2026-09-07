[webuser@control-node web_ansible]$ sudo yum install ansible-core -y

[webuser@control-node ~]$ tree web_ansible/
web_ansible/
├── ansible.cfg
├── inventory
└── web-target.yml

0 directories, 3 files
[webuser@control-node ~]$
[webuser@control-node web_ansible]$ cat ansible.cfg
[defaults]
inventory=./inventory
remote_user=webuser
ask_pass=false

[privilege_escalation]
become=yes
become_method=sudo
become_user=root
become_ask_pass=false
[webuser@control-node web_ansible]$

[webuser@control-node web_ansible]$ cat inventory
[webserver]
192.168.1.20

[webuser@control-node web_ansible]$ ansible all -m ping
192.168.1.20 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}

[webuser@control-node web_ansible]$ cat web-target.yml
---
- name: Automated Web Server Deployment
  hosts: webserver
  become: yes
  tasks:
    - name: Install Apache HTTPD
      yum:
        name: httpd
        state: latest

    - name: Start and Enable Apache Service
      service:
        name: httpd
        state: started
        enabled: yes

    - name: Create Custom Landing Page
      copy:
        content: "<h1>The webserver deployed successfully</h1>"
        dest: /var/www/html/index.html

    - name: Configure Firewall for HTTP
      firewalld:
        service: http
        permanent: yes
        state: enabled
        immediate: yes

[webuser@control-node web_ansible]$ ansible-playbook web-target.yml --syntax-check
[WARNING]: Collection ansible.posix does not support Ansible version 2.14.2

playbook: web-target.yml

[webuser@control-node web_ansible]$ ansible-playbook web-target.yml
[WARNING]: Collection ansible.posix does not support Ansible version 2.14.2

PLAY [Automated Web Server Deployment] *********************************************************************************

TASK [Gathering Facts] *************************************************************************************************
ok: [192.168.1.20]

TASK [Install Apache HTTPD] ********************************************************************************************
ok: [192.168.1.20]

TASK [Start and Enable Apache Service] *********************************************************************************
ok: [192.168.1.20]

TASK [Create Custom Landing Page] **************************************************************************************
ok: [192.168.1.20]

TASK [Configure Firewall for HTTP] *************************************************************************************
ok: [192.168.1.20]

PLAY RECAP *************************************************************************************************************
192.168.1.20               : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

[webuser@control-node web_ansible]$ curl 192.168.1.20
<h1>The webserver deployed successfully</h1>[webuser@control-node web_ansible]$


