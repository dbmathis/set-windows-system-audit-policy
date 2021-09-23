> Note: Use at your own risk, this bosh release is not officially supported by VMware Tanzu. 

# Set Windows System Audit Policy on Bosh deployed Windows 2019 Server

## What does this do?

For windows 2019 server nodes deployed by Bosh, the job in this release sets windows system audit policy.

The template `pre-start.ps1.erb` executes a auditpol command to modify system audit policy on the host. 

## How do I install it?    

- Access Bosh
```
export BOSH_CLIENT=ops_manager BOSH_CLIENT_SECRET=fakesecret BOSH_CA_CERT=/var/tempest/workspaces/default/root_ca_certificate  BOSH_ENVIRONMENT=10.0.0.10
```
- Copy or clone this repository onto your BOSH CLI workstation and create+upload the BOSH release to the director

```
git clone https://github.com/dbmathis/set-windows-system-audit-policy && cd set-windows-system-audit-policy
```
> Note - Following bosh commands must be executed from within set-windows-system-audit-policy directory
```
bosh create-release --force

bosh upload-release ./dev_releases/set-windows-system-audit-policy/set-windows-system-audit-policy-0+dev.1.yml 
```

-  Configure the Bosh addon from this repo
```
bosh update-config --name=set-windows-system-audit-policy --type=runtime ./bosh-addon.yml
```

- Bosh deploy. In case of TKGI, upgrade the cluster running the windows server nodes using the TKGI cli `tkgi upgrade-cluster <cluster-name>`

## How do I remove the bosh addon?

- Get the bosh config ID
```
bosh configs | grep set-
```
Example:
> Command >  bosh configs | grep 'set-windows-system-audit-policy' | awk -F"*" '{print $1}'  
> Output > 42
- Delete the bosh config
```
bosh delete-config <ID>
```
- Recreate the Bosh deployment
- Delete the Bosh release
```
bosh delete-release set-windows-system-audit-policy/0+dev.1
```
## Credits
- [John Nelson](https://github.com/ceesco53)
- [Bosh.io - Create a release](https://bosh.io/docs/create-release/) 