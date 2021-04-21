![Alt text](pics/elk1.png?raw=true)

# Ansible Role for Elastic Stack + beats [Lab] deployment
This repo contains everything you need to get started with Elastic Stack + beats using ansible to deploy the code on your server. The goal is to create a working ELK stack LAB environment.

## Pre-requisites
For this demo I use 1 x t2.medium for the Elastic Search master & 2 x t2.micro as my remote servers. I run Ansible from a control node.

Local:
 * Installation of Ansible: Latest version.

Remote:
 * tested on a Ubuntu 18.04 AMI (ami-0e0090d3db396e90d in region eu-west-3)
 * Install these Packages: wget curl git python-minimal default-jre *
 * Minimum memory requirements: 4GB (t2.medium in AWS)
 * Open Firewall ports:
   * (Elastic Master) Ingress: 22(ssh), 5601(kibana),3000(grafana),5044(logstash)
   * (Everything else) Ingress: 22(ssh)
   * Egress: ALL

## Usage
Clone the repo:
```
$ git clone https://github.com/dmccuk/ansible_ELK.git
$ cd ansible_ELK
```

### Deploy Elastic Stack on the master node:
Once you have a server to install the Elasticsearch, kibana and logstash on, get the IP address or servername and replace it in the comman below.

  * I don't supply a username or password. I'm using the Ubuntu user and an SSH key. You can set up an SSH key yourself id you need to or add ````-u <username> -k```` and you will be prompted for your password.
  * 
```
$ cd ansible_ELK/roles
& ansible-playbook -i <server_name/ip>, deployELK.yml
```

If you get an error about Permission denied UNREACHABLE try this:
```
fatal: [34.245.169.116]: UNREACHABLE! => {"changed": false, "msg": "Failed to connect to the host via ssh: Warning: Permanently added '34.245.169.116' (ECDSA) to the list of known hosts.\r\nPermission denied (publickey).\r\n", "unreachable": true}
        to retry, use: --limit @/home/vagrant/ansible/roles/deployELK.retry

PLAY RECAP ************************************************************************************************
34.245.169.116             : ok=0    changed=0    unreachable=1    failed=0
```
Run the following commands that are in the pre_run.sh script. Replace with your .pem key name:
```
# ssh-agent bash
# ssh-add ../you_ec2_key.pem
```
Now re-run the ansible-playbook. It should work this time.

## Open Kibana
Once the playbook has completed, you should be able to go to access Kibana.
Go to the URL:

http://<server_or_IP>:5601

## Add index In Kibana
The Index should already be available because Ansible setups up metricbeat on the Elastic Master server and forwards on metrics to logstash which in turn forwards to Elatsicsearch. Follow these instructions to add the index.

On your Kibana webpage, click on "Discover" or "Management" and add in your index as below. Click next:
![Alt text](pics/kibana1.PNG?raw=true)

Select @timestamp:
![Alt text](pics/kibana2.PNG?raw=true)

Select Discover and see the messages:
![Alt text](pics/kibana3.PNG?raw=true)

## List out available Indexes:
Once you've deployed the stack, on your server command line, you can run the following command to see the indexes available.

    # curl -s http://localhost:9200/_cat/indices?v
    health status index                     uuid                   pri rep docs.count docs.deleted store.size pri.store.size
    green  open   .kibana                   2UuCqV84Rb-u7rpUXHWDWg   1   0          2            0     20.9kb         20.9kb
    yellow open   logstash-daily-2021.04.10 tbeKCDg_QOCB-7B-oQUiTQ   5   1     126642            0     77.4mb         77.4mb

# Grafana Setup:
If you run the whole ansible role, Grafana will be installed by default. If you want to run it manually, it looks like this:

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
Visit http://<fqdn_or_IP>:3000

Login username and password is: admin/admin

![Alt text](pics/grafana1.PNG?raw=true)

## Grafana setup
Add example grafana dashboard file. This can be downloaded to the PC and imported into Grafana (Grafana_basic_dashboard):

Now setup Elasticsearch as your Datasource:
![Alt text](pics/grafana1.1PNG?raw=true)
![Alt text](pics/grafana4.PNG?raw=true)

And use these settings:
![Alt text](pics/grafana5.PNG?raw=true)

Save and Test.

Now import the dashboard. Download this file from this GitHub to you PC (Grafana_basic_dashboard):
![Alt text](pics/grafana2.PNG?raw=true)
![Alt text](pics/grafana3.PNG?raw=true)

Select the file to import and set these settings:
![Alt text](pics/grafana6.PNG?raw=true)

The dashboard will appear and you will see basic CPU and Memory utilisation.
![Alt text](pics/grafana7.PNG?raw=true)

# Test the metrics!
Install the "stress" program on Ubuntu to make the metrics move and watch them on grafana.

    # sudo apt install stress -y
To test the CPU:

    # stress --cpu 3
    
To test CPU, memory & IO:

    # stress --cpu 3 -m 2 -d 1

Now go back to grafana and watch the server resources change.

# Next Steps?

## Monitor additional servers and send your data back to logstash.
If you run the deployBEATS.yml Ansible role, you can add more servers in and see their metrics appear in Grafana.

### To install beats on other nodes (Back on the control node):
First, updated the ```group_vars/all``` file with the IP address of the Elastic master server.

```
$ cat group_vars/all
---
elastic_master: IP/SERVERNAME
```
Add in the IP address and save the file. This variable will be used in the filebeat.yml config and send the metrics to Elasticsearch.
** At this point, you may need to open the FW for port 9200 **

Now you can run the following Ansible role to setup Beats on the new servers.

    # cd ~/ansible_ELK/roles
    # ansible-playbook -i inventory_file deployBEATS.yml

The Ansible coniguration takes care of the IP address being added to the correct file.

## Add in new Metrics
Try adding your own dashboards using the metricbeat data. For my information, go you www.grafana.com

Tip
===
If you want to use this setup you need to be mindful of how much free space you have on the server. I recommend creating a seperate LV to store the ES data on some large disks. You need to correctly size your environment by working out how much data one server brings in over 24 hours. Then do the math and times that number by the total number of servers AND how many days worth of data you want to keep. Then add 25% for good measure.

ES data can be archived and stored in a different location for later retrieval. You'll need to automate this process to ensure you don't run out of space and lose data.

Another option is to add Redis infront of ES. That way, Redis can manage the peaks and troughs of traffic and server as a buffer for when you get a storm of message due to an issue on one of your servers. If ES goes down for any reason, redis can buffer the messages for a period of time (while ES is down) so you can recover ES and not lose data.

I hope this has been useful.
Thanks for taking a look - Dennis
