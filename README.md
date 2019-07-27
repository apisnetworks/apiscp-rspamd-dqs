# SpamHaus DQS integration addin

This addin automatically configures SpamHaus' [Data Query Service](https://github.com/spamhaus/rspamd-dqs) for use with rspamd. Enabling DQS improves rspamd filtering sensitivity.

## Installation

### Requirements
* apnscp must be running [rspamd](https://hq.apnscp.com/filtering-spam-with-rspamd/) as its spam filter
* SpamHaus DQS key. [Registration](https://www.spamhaustech.com/dqs/) required.

```bash
# Set the key
cpcmd config:set apnscp.bootstrapper rspamd_dqs_key KEYFROMABOVE
# Setup addin
cd /usr/local/apnscp
git submodule add https://github.com/apisnetworks/apnscp-rspamd-dqs.git resources/playbooks/addins/rspamd-dqs
git submodule update --init --recursive resources/playbooks/addins/rspamd-dqs
cd resources/playbooks
ansible-playbook addin.yml --extra-vars=addin=rspamd-dqs
```

And that's it!

### Removal
```bash
cd /usr/local/apnscp/resources/playbooks
ansible-playbook addin.yml --extra-vars=addin=rspamd-dqs --tags=uninstall,all
rm -rf addins/rspamd-dqs
```

## Verifying installation
`rspamdadm configtest` will validate your configuration. *Syntax OK* indicates there are no issues with mail. After a day look for SpamHaus scores in rspamd:

```bash
zcat /var/log/rspamd/rspamd.log.1.gz | grep ZRD_ 
```

