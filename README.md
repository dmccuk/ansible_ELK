![Alt text](pics/elk1.png?raw=true)

# Ansible Playbook & Role for ELK Stack + beats deployment

This repo containts everything you need to get started with ELK Stack + beats using ansible to deploy the code on your server.

In this repo there is a playbook with all the code contained in one file (elk.yml) and an ansible role under the "role" directory that does exactly the same thing, but within an ansible role setup.

## Assumed knowledge
This repo expects you to already know how to use ansible playbooks and roles.

## Pre-requisites
Please install the following on all nodes you want to deploy ELK Stack on:

 * Server used: Ubuntu 16.04
 * Packages: wget curl git python-minimal default-jre
 * Minimum memory requirements: 4GB (t2.medium in AWS)

## Usage
Clone the repo:

    # git clone https://github.com/dmccuk/ansible_play_role.git

Run the playbook:

    # ansible-playbook -i <server_name/ip>, elk.yml

If you get an error about Permission denied please check the bottom of this page.

## Open Kibana

Go to the URL for Kibana:

http://<server_or_IP:5601

## Add index In Kibana

On your Kibana webpage, click on "Discover" and add in your index as below. Click next:
![Alt text](pics/kibana1.png?raw=true)

Select @timestamp:
![Alt text](pics/kibana2.png?raw=true)

Select Discover and see the messages:
![Alt text](pics/kibana3.png?raw=true)

## List out available Indexes:

Once you've deployed the stack, on your server command line, you can run the following command to see the indexes available.

    # curl localhost:9200/_cat/indices?v

# Grafana Setup:
As part of this role I've include a script that will install grafana. Simply run the script, then login to the grafana webpage on port 3000.

<details>
 <summary>Grafana setup</summary>
  <p>

```
# /data/grafana.sh

--2018-09-27 09:11:58--  https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana_5.1.4_amd64.deb
Resolving s3-us-west-2.amazonaws.com (s3-us-west-2.amazonaws.com)... 54.231.185.64
Connecting to s3-us-west-2.amazonaws.com (s3-us-west-2.amazonaws.com)|54.231.185.64|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 52631282 (50M) [application/x-debian-package]
Saving to: ‘grafana_5.1.4_amd64.deb’
grafana_5.1.4_amd64.deb  100%[=======================================>]  50.19M  7.04MB/s    in 8.1s

2018-09-27 09:10:24 (6.23 MB/s) - ‘grafana_5.1.4_amd64.deb’ saved [52631282/52631282]

Reading package lists... Done
Building dependency tree
Reading state information... Done
Note, selecting 'libfontconfig1' instead of 'libfontconfig'
adduser is already the newest version (3.113+nmu3ubuntu4).
libfontconfig1 is already the newest version (2.11.94-0ubuntu1.1).
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
(Reading database ... 60521 files and directories currently installed.)
Preparing to unpack grafana_5.1.4_amd64.deb ...
Unpacking grafana (5.1.4) over (5.1.4) ...
Setting up grafana (5.1.4) ...
Restarting grafana-server service... OK
Processing triggers for systemd (229-4ubuntu21.4) ...
Processing triggers for ureadahead (0.100.0-19) ...
● grafana-server.service - Grafana instance
   Loaded: loaded (/usr/lib/systemd/system/grafana-server.service; disabled; vendor preset: enabled)
   Active: active (running) since Thu 2018-09-27 09:10:27 UTC; 393ms ago
     Docs: http://docs.grafana.org
 Main PID: 2281 (grafana-server)
   CGroup: /system.slice/grafana-server.service
           └─2281 /usr/sbin/grafana-server --config=/etc/grafana/grafana.ini --pidfile=/var/run/grafana/gra

Sep 27 09:10:27 ip-172-31-25-50 grafana-server[2281]: t=2018-09-27T09:10:27+0000 lvl=info msg="Executing mi
Sep 27 09:10:27 ip-172-31-25-50 grafana-server[2281]: t=2018-09-27T09:10:27+0000 lvl=info msg="Skipping mig
Sep 27 09:10:27 ip-172-31-25-50 grafana-server[2281]: t=2018-09-27T09:10:27+0000 lvl=info msg="Executing mi
Sep 27 09:10:27 ip-172-31-25-50 grafana-server[2281]: t=2018-09-27T09:10:27+0000 lvl=info msg="Skipping mig
Sep 27 09:10:27 ip-172-31-25-50 grafana-server[2281]: t=2018-09-27T09:10:27+0000 lvl=info msg="Starting plu
Sep 27 09:10:27 ip-172-31-25-50 grafana-server[2281]: t=2018-09-27T09:10:27+0000 lvl=info msg="Initializing
Sep 27 09:10:27 ip-172-31-25-50 grafana-server[2281]: t=2018-09-27T09:10:27+0000 lvl=info msg="Initializing
Sep 27 09:10:27 ip-172-31-25-50 grafana-server[2281]: t=2018-09-27T09:10:27+0000 lvl=info msg="Initializing
Sep 27 09:10:27 ip-172-31-25-50 grafana-server[2281]: t=2018-09-27T09:10:27+0000 lvl=info msg="Initializing
Sep 27 09:10:28 ip-172-31-25-50 systemd[1]: Started Grafana instance.
```
</p></details>

### Grafana Web URL

Visit http://<server_or_IP:3000

Login username and password is: admin/admin

## Grafana setup
Add example grafana dashboard file. This can be downloaded to the PC and imported into Grafana. It should work out of the box.
Screen shot here.

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

