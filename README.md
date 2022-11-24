# mastodon-ansible (l3ib edition)

This playbook is forked from the official [mastodon ansible](https://github.com/mastodon/mastodon-ansible) playbook. It is intended to be used on the free server that you can get from Oracle Cloud Infrastructure. You should be able to go from nothing to a working mastodon server in about an hour, even if you've never installed or used any of the tools before, assuming you have some sysadmin experience. All for $0.

## Audience

This playbook is intended for experienced sysadmins who understand and accept the risks of deploying a social media server on free-tier hosting. It has only been tested against the server provided by the terraform repo in the first step.

## Setup

### Getting a free server

Follow the steps (and pay attention to the disclaimer) from here: https://github.com/Fitzsimmons/oracle-always-free-vps

### Domain

You need a domain for your mastodon server. Get one and point the DNS at the IP address returned from the previous step.

### Installing and running ansible

```sh
virtualenv -p /usr/bin/python3 env
source env/bin/activate
git clone https://github.com/mastodon/mastodon-ansible.git
cd mastodon-ansible
pip install -r requirements.txt
ansible-galaxy collection install community.general
```
## Running the playbook


```sh
$ ansible-playbook playbook.yml -i <your-host-here>, -u <remote-user> --extra-vars="mastodon_host=<your-host-here>"
```

After the playbook has finished its execution, Mastodon now should be available at the hostname you defined and you're not required run the Mastodon setup wizard. As Email servers differ widely from configuration to configuration **you must edit the .env.production file and add your own email server details followed by restart of Mastodon services.**

To edit `.env.production`, follow these steps:

```bash
ssh yourmachine
su - mastodon
cd ~/live
nano .env.production
systemctl restart mastodon-*.service
```

To see a list of available environment variables for your Mastodon installation, please refer to [the Mastodon documentation](https://docs.joinmastodon.org/admin/config/).

## Post-playbook steps

An an exercise for the reader, consider implementing the following on the server manually or by augmenting this playbook:

* offsite backups (https://docs.joinmastodon.org/admin/backups/)
* monitoring/metrics
	* Mastodon is very storage-hungry, so watch out for that.
	* Going over the 10TB of outbound traffic provided by the OCI free tier could end up costing you if it's left unchecked. Try running `vnstat` for inspiration.

## WIP

I have cleaned up some but not all of the cross-distro cruft from the main playbook. It doesn't slow things down much but it is a little busy.
