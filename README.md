# Basic Install of Kubernetes and Contiv Networking
## standalone kubeadm deployment
### Orignal code credit goes to https://github.com/ben-st/ansible-kubeadm. I used it as a base and changed the CNI to Contiv (by Cisco).

- Requires Ansible 2.4 or newer
- Expects 3 ubuntu nodes (at least 16.04)
- Expects passwordless sudo

These playbooks deploy a very basic installation of kubeadm.
To use them, first edit the "inventory" file to contain the
hostnames of the machines on which you want kubeadm deployed, and edit the
group_vars/ file to set any kubeadm configuration parameters you need.

Then run the playbook, like this:

	`ansible-playbook -i inventory site.yml`


This is a very simple playbook. It deploys the following:

1. Installs basic components (docker, kubeadm, contiv) 
2. Basic kubernetes cluster using kubeadm (3 nodes is recommended)
3. Contiv CNI in vxlan mode
4. Adds three networks (see group_vars/all)
5. Adds two sample applications (guestbook and wordpress)

### Start istio dashboard with access from ec2 instance public ip 

EC2 instances don't know their public ip address (it's managed by AWS infrastructure), so you have to get it from metadata.

From *Master* node:

```
echo "http://$(curl -s http://169.254.169.254/latest/meta-data/public-ipv4):20001/kiali"  # true public url to open dashboard
istioctl dashboard kiali --browser=false --address $(curl -s http://169.254.169.254/latest/meta-data/local-ipv4) # istioctl listening on local ip
```
