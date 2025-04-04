# GKE Installer Example

This folder contains working examples of the terraform modules provided by this GKE Installer.

In order to test them, you follow the instructions below.
Note all comments starting with `TASK: ` require you to run some manual action on your computer
that cannot be automated with the following script.

```bash
# First of all, export the needed env vars for the gcp provider to work
gcloud auth application-default login
export GOOGLE_APPLICATION_CREDENTIALS=~/.config/gcloud/application_default_credentials.json

# Bring up the vpc
cd examples/vpc
cp main.auto.tfvars.dist main.auto.tfvars
# TASK: fill in main.auto.tfvars with your data
terraform init
terraform apply

# Bring up the vpn
cd examples/vpn
cp main.auto.tfvars.dist main.auto.tfvars
# TASK: fill in main.auto.tfvars with your data
terraform init
terraform apply

# Create a OpenVPN client certificate using furyagent
furyagent configure openvpn-client --config=./secrets/furyagent.yml --client-name test > /tmp/fury-example-test.ovpn
# TASK: import the generated /tmp/fury-example-test.ovpn in the openvpn client of your choice and turn it on.

# Bring up the kubernetes cluster
cd ../gke
terraform init
terraform apply

# Once all the above is done you can dump the kube config to a file of your choice
terraform output -raw kubeconfig > /var/tmp/.kubeconfig

# Last but not least, you can verify your cluster is up and running
KUBECONFIG=/var/tmp/.kubeconfig kubectl get nodes
```
