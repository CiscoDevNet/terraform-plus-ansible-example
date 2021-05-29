Example of Combining Terraform and Ansible
=====================================
Combining Terraform and Ansible

In this example, we cover a simple use case that uses Terraform and Ansible together to deploy and provision five virtual machines in vSphere and then install an Apache Web Server on each host. The virtual machine deployment operates by cloning a virtual machine template running CentOS 8.2 as the guest OS. Terraform uses the VMware provider to clone the template five times, copy the service account's SSH keys, and then a provisioner is used to call an Ansible playbook that opens port 80 on the firewall, installs an Apache Web Server, and copies a unique `index.html` page containing the hostname of the machine. The web page is built using a Jinja template so that the unique page is created for each host.

## White Paper

[Ansible and HashiCorp: Better Together](https://www.hashicorp.com/resources/ansible-terraform-better-together)
[Hashicorp Terraform and RedHat Ansible Automation](https://www.redhat.com/cms/managed-files/pa-terraform-and-ansible-overview-f14774wg-201811-en.pdf)

## Related Sandbox

[Cisco Intersight Service for HashiCorp Terraform](https://devnetsandbox.cisco.com/RM/Diagram/Index/055e2dce-fdfd-4d26-a112-72b884ddd7c7?)

## Links to DevNet Learning Labs

[Introduction to Intersight Service for Hashicorp Terraform](https://developer.cisco.com/learning/lab/intersight-01-ist-introduction/step/1)
[Provisioning VMs using Intersight Terraform Service for Hashicorp](https://developer.cisco.com/learning/lab/intersight-02-ist-vm-automation/step/1)

## Solutions on Ecosystem Exchange

[Intersight Service for HashiCorp Terraform](https://developer.cisco.com/ecosystem/spp/solutions/189099/)