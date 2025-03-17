
## Ping everything to see we have ssh access via ansible
ansible all -i inventory/hosts.yml -m ping

## install basic
ansible-playbook playbooks/01-common.yml

# ✅ 1. Configure DB and Galera (NO start)
ansible-playbook playbooks/02-database.yml --limit db1,db2,db3

# ✅ 2. Bootstrap Galera (db1 only)
ansible-playbook playbooks/02-database-bootstrap.yml --limit db1

# ✅ 3. Start Galera on db2 and db3
ansible-playbook playbooks/02-database-start.yml --limit db2,db3

# ✅ 4. Create Zabbix DB and user (on db1)
ansible-playbook playbooks/02-database-zabbix-user.yml --limit db1

# ✅ 5. Install and configure Zabbix Server backend
ansible-playbook playbooks/03-zabbix-server.yml --limit zbx-server1,zbx-server2

# ✅ 6. Install and configure Zabbix Web frontend
ansible-playbook playbooks/04-zabbix-web.yml --limit zbx-web1,zbx-web2

# ✅ 7. Finalize and deploy HAProxy + Keepalived for LB/HA
ansible-playbook playbooks/05-haproxy-keepalived.yml --limit lb1,lb2


