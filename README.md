# Ansible Playbooks for installing Openshift Container Platform 4.X

The playbooks are used for installation of OCP on Power and other post installation steps. Before running the playbooks on the bastion node, ensure that all the required services are configured.

## Assumptions:

 - A bastion node is already created where the playbooks would run.
 - The required services are configured on the bastion node eg: HTTP, HAProxy, DNS, DHCP, etc. This can also be achieved by using [ocp4-helpernode](https://github.com/RedHatOfficial/ocp4-helpernode) playbook.
 - The cluster nodes are already created.
 - The playbooks are supported to use with https://github.com/ocp-power-automation/ocp4_upi_powervm only.

## Set up variables

Make use of the sample file at `examples/install_vars.yaml`. Modify the values as per your cluster.

```
cp examples/install_vars.yaml .
```

## Use install_vars.yaml

This section sets the variables for the install-config.yaml template file.

```
install_config: 
   cluster_domain: < Cluster domain name. Match to the baseDomain in install-config.yaml.>
   cluster_id: < Cluster identifier. Match to the metadata.name in install-config.yaml.>
   pull_secret: '<pull-secret json content>'
   public_ssh_key: '<SSH public key content to access the cluster nodes>'
```

Below variables will be used by the OCP install playbook.

```
workdir: <Directory to use for creating OCP configs>
storage_type: <Storage type used in the cluster. Eg: nfs (Note: Currently NFS provisioner is not configured using this playbook. This variable is only used for setting up image registry to EmptyDir if storage_type is not nfs)>
log_level: <Option --log-level in openshift-install commands. Default is 'info'>
release_image_override: '<This is set to OPENSHIFT_INSTALL_RELEASE_IMAGE_OVERRIDE while creating ign files. If you are using internal artifactory then ensure that you have added auth key to the pull_secret>'
rhcos_kernel_options: <List of kernel options for RHCOS nodes eg: ["slub_max_order=0","loglevel=7"]>
```

## Setting up inventory

Make use of sample file at `examples/inventory`. Modify the host values as per your cluster.

```
cp examples/inventory .
```


## Run the playbook

Once you have configured the vars & inventory file, run the install playbook using:

```
ansible-playbook  -i inventory -e @install_vars.yaml playbooks/install.yaml
```


License
-------

See LICENCE.txt
