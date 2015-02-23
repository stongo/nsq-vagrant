# NSQ in CoreOS - Vagrant

Following these simple instructions, you can have a functioning [NSQ](http://nsq.io) machine or cluster in CoreOS, ready for Development

## Instructions

1. Install Vagrant and the provider of your choice
1. Clone `https://github.com/coreos/coreos-vagrant` somewhere
1. Copy `user-data` to `coreos-vagrant`
1. Add the following port forwards to the Vagrantfile in `coreos-vagrant`
```
config.vm.network "forwarded_port", guest: 4001, host: 4001
config.vm.network "forwarded_port", guest: 4150, host: 4150
config.vm.network "forwarded_port", guest: 4151, host: 4151
config.vm.network "forwarded_port", guest: 4160, host: 4160
config.vm.network "forwarded_port", guest: 4161, host: 4161
```
1. Run `vagrant up`
1. SSH in to vagrant - `vagrant ssh` and execute `fleetctl start nsqlookupd-sidekick.service`

That should be it!

## nsqd
* tcp api on `localhost:4150`
* http api on `localhost:4151`

## nsqlookupd
* tcp api on `localhost:4160`
* http api on `localhost:4161`

## etcd
* http api on `localhost:4001`
* nsqlookupd-sidekick.service sets two keys in etcd on discovery - `/nsqlookupd` and `/nsqlookupd-http` containing the tcp and http api hostnames. Necessary when using multiple nodes in the CoreOS cluster