
================================================
k8s-salt-model Reclass Model
================================================

config node
===========

cfg01

Kubernetes control cluster
==========================

ctl01
ctl02
ctl03

Kubernetes computes 
===================

cmp01
cmp02

Deploy
======

1. Run 'mk-k8s-simple-deploy' jenkins job

OPENSTACK_API_PROJECT: prometheus_monitoring

HEAT_STACK_TEMPLATE: mcp_fuel_aio_support

2. Sync /srv/salt/reclass with this repo

3. Run

salt -C 'I@salt:master' state.sls reclass

salt -C '*' saltutil.refresh_pillar

salt -C '*' saltutil.sync_all

salt -C 'I@linux:system' state.sls linux,openssh,salt.minion,ntp

salt -C 'I@glusterfs:server' state.sls glusterfs.server.service

salt -C 'I@glusterfs:server' state.sls glusterfs.server.setup -b 1

salt -C 'I@docker:host' state.sls docker.host

salt -C 'I@prometheus:server' state.sls glusterfs.client

salt -C 'I@docker:swarm:role:master' state.sls docker.swarm

salt -C 'I@docker:swarm:role:master' state.sls salt

salt -C 'I@docker:swarm:role:manager' state.sls docker.swarm -b 1

salt -C 'I@linux:system' state.sls telegraf

salt -C 'I@linux:system' state.sls salt

salt -C 'I@linux:system' mine.update

salt -C 'I@docker:swarm:role:master' state.sls prometheus.server

salt -C 'I@docker:swarm:role:master' state.sls prometheus.alertmanager

# Sync docker images to docker swarm nodes

salt -C 'I@docker:swarm' cmd.run "docker pull .../bkupidura/pushgateway"

salt -C 'I@docker:swarm' cmd.run "docker pull .../bkupidura/alertmanager"

salt -C 'I@docker:swarm' cmd.run "docker pull .../bkupidura/prometheus"

salt -C 'I@docker:swarm' cmd.run "docker tag .../bkupidura/pushgateway:latest pushgateway:latest"

salt -C 'I@docker:swarm' cmd.run "docker tag .../bkupidura/alertmanager:latest alertmanager:latest"

salt -C 'I@docker:swarm' cmd.run "docker tag .../bkupidura/prometheus:latest prometheus:latest"

salt -C 'I@docker:swarm:role:master' state.sls docker
