# Example of Combining Terraform and Ansible

In this example, we cover a simple use case that uses Terraform and Ansible together to deploy and provision five virtual machines in vSphere and then install an Apache Web Server on each host. The virtual machine deployment operates by cloning a virtual machine template running CentOS 8.2 as the guest OS. Terraform uses the VMware provider to clone the template five times, copy the service account's SSH keys, and then a provisioner is used to call an Ansible playbook that opens port 80 on the firewall, installs an Apache Web Server, and copies a unique `index.html` page containing the hostname of the machine. The web page is built using a Jinja template so that the unique page is created for each host.

## Requirements

Below is a list of requirements to make this work in your environment:

- vSphere 6.7 or higher
- Terraform v0.15.2 or higher
- Ansible 2.10.2
- A virtual machine template with CentOS 8.2 installed as the guest OS

## Credentials

We purposely did not add credentials and other sensitive information to the repo by including them in the `.gitignore` file. As such, if you clone this repo, you will need to create two files. The first file named `secret.tfvars` contains sensitive Terraform variables. The second file named `variables.yml` is used by Ansible. In this scenario, we encrypted `variables.yml` using the command `ansible-vault` command and decrypt it as needed locally. You could take the same approach or leave the file unencrypted if you are confident it won't be shared or inadvertently uploaded to a repo.

Here is a list of variables you'll need to include and define for each file

- `secret.tfvars` in HCL format (file is in the same directory as the `terraform.tfvars` file):
  - vsphere_user
  - vsphere_password
  - vsphere_server (the IP address or FQDN)
  - vsphere_vm_firmware (default is `vsphere_vm_firmware = bios`)
  - ssh-pub-key (an SSH key used with a service account that allows Ansible to connect over SSH)
  - service_account_username
  - service_account_password
- `.vault_pass.txt` file (optional):
  - A single line of text with the `ansible-vault` password should you decide to encrypt the file with `ansible-vault`


## What Terraform Provisions

In this example, Terraform uses the `vsphere` provider and a `vsphere_virtual_machine` resource to:

- Create 5 virtual machines from virtual machine template
- Add the SSH key of a service account to each host
- Run an Ansible playbook that performs the steps in the next section

IP addresses are assigned statically and are hard-coded into the Terraform provisioner. This approach is simplistic and only meant to demonstrate how you can start with a static address and add to it as needed so each address is unique. Addresses could also be assigned dynamically with DHCP and you can  modify that section of code as needed.

## What Ansible Installs and Configures

After Terraform creates 5 virtual machines, the Ansible playbook installs and configures:

- Apache Web Server
- Firewall with port 80 opened
- Docker (we won't make use of it in this example but may in a new usecase)
- DNS (resolv.conf is configured)

Each Apache Web Server is configured with a custom (using a Jinja template) `index.html` page that displays the hostname. The `apache-web-server` role contains a Jinja template `index.html` file that inserts the hostname the machine before copying it to the target host.

## Creating and Applying the Terraform Plan

Here are the steps needed to run Terraform along with examples of each:

1. Initialize Terraform:
    
`terraform init  -var-file="secret.tfvars"`

2. Create a Terraform Plan:

`terraform plan -out da-compute-apache-web-server.tfplan -var-file="secret.tfvars"`

3. Apply the Terraform Plan:

`terraform apply -var-file="secret.tfvars"`

Each of these commands includes the `secret.tfvars` containing the sensitive variables needed to connect to the different resources as described in the previous section. Also the 

## Results

You will see 5 virtual machines created with static IP addresses in vSphere, each running an Apache Web Server with a unique page displaying its hostname.

![List of vSphere virtual machines](images/vsphere-virtual-machines.png)

Open a web-browser and navigate to the IP address of any of the virtual machine's IP addresses to see the resulting HTML page.

![Data Reported by the Machine Agent](/images/apache-server-result.png)

## Related Sandbox

Get hands on experience with Intersight Service for Terraform in DevNet's Sandbox environment.

[Cisco Intersight Service for HashiCorp Terraform](https://devnetsandbox.cisco.com/RM/Diagram/Index/055e2dce-fdfd-4d26-a112-72b884ddd7c7?diagramType=Topology)

## Links to DevNet Learning Labs

Learn how to provision virtual machines in vSphere using Intersight Service for Terraform.

[Introduction to Intersight Service for Hashicorp Terraform](https://developer.cisco.com/learning/lab/intersight-01-ist-introduction/step/1)
[Provisioning VMs using Intersight Terraform Service for Hashicorp](https://developer.cisco.com/learning/lab/intersight-02-ist-vm-automation/step/1)



