![Alt text](elk1.png?raw=true)

# ansible Playbook & Role for ELK Stack deployment

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

