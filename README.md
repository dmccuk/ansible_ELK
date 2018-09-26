![Alt text](elk1.png?raw=true)

# Ansible Playbook & Role for ELK Stack + beats deployment

This repo containts everything you need to get started with ELK Stack + beats using ansible to deploy the code on your server.

In this repo there is a playbook with all the code contained in one file (elk.yml) and an ansible role under the "role" directory that does exactly the same thing, but within an ansible role setup.

## Assumed knowledge
This repo expects you to already know how to use ansible playbooks and roles.

## Pre-requisites
Please install the following on all nodes you want to deploy ELK Stack on:

 * Server used: Ubuntu 16.04
 * Packages: wget curl git python-minimal default-jre
 * Minimum memory requirements: 4GB

## Usage
Clone the repo:

    # git clone https://github.com/dmccuk/ansible_play_role.git

Run the playbook:

    # ansible-playbook -i <server_name/ip>, elk.yml

## List out available Indexes:

Once you've deployed the stack, on your server, you can run the following command to see the indexes available.

    # curl localhost:9200/_cat/indices?v

# Grafana Setup:
As part of this role I've include a script that will install grafana. Simply run the script, then login to the grafana webpage on port 3000.

    # /data/grafana.sh

Visit http://<server_or_IP:3000

Login username and password is: admin/admin


# ISSUES:
If you get this error:

```
fatal: [34.245.169.116]: UNREACHABLE! => {"changed": false, "msg": "Failed to connect to the host via ssh: Warning: Permanently added '34.245.169.116' (ECDSA) to the list of known hosts.\r\nPermission denied (publickey).\r\n", "unreachable": true}
        to retry, use: --limit @/home/vagrant/ansible/roles/deployELK.retry

PLAY RECAP ************************************************************************************************
34.245.169.116             : ok=0    changed=0    unreachable=1    failed=0
```

Run the following commands that are in the pre_run.sh script:
```
# ssh-agent bash
# ssh-add ../you_ec2_key.pem
```
Now re-run the ansible-playbook. It should work this time.



