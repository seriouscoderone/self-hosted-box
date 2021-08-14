# self-hosted-box

The following is the manual detailing steps for self-hosting  Zion relay cluster on AWS.

## Requirements
   1. macOS + ansible [Details](ops/ansible/README.md)
   2. Github Account + relevant keys [Details](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) [GITHUB_ACCESS_KEY] 
   3. AWS Account + relevant keys
      1. keypair for EC2 creation/access [EC2_KEYPAIR]
      2. IAM User [YOUR_AWS_KEY] [YOUR_SECRET_KEY]

## Initial Setup
1. Create /etc/environment (Ansible will use these values as addional variable for your EC2 instances and relevant docker images required by Zion relay and lnd)
```

```

2. Create ansible vars file ops/ansible/group_vars/all.yml (Ansible will use these values to set up your cluster)

```
aws_access_key: [YOUR_AWS_KEY]
aws_secret_key: [YOUR_AWS_SECRET]

key_name: [EC2_KEYPAIR]
aws_region: "us-west-2"

# obtainable in your AWS account
vpc_id: [EC2_CLUSTER_ID]

nodejs_version: "8"

# https://cloud-images.ubuntu.com/locator/ec2/
# bionic 18.04LTS hvm:ebs-ssd
ami_id: "ami-0edf3b95e26a682df"

communities_host: communities.getzion.com
communities_host_staging: null

media_host: memes.getzion.com
media_host_staging: null
```

3. Generate github SSH keys and move them to the following paths (EC2 will use this key in order to pull source from this repo.)

``` [GITHUB_ACCESS_KEY]
~/.ssh/id_rsa.github
~/.ssh/id_rsa.github.pub
```

[Details](ops/ansible/playbooks/roles/play/tasks/main.yml)

4. Modify cluster parameters to reflect the relevant domain/subdomain where you plan accessing you Zion relay.
[cluster_configs](ops/ansible/playbooks/cluster_configs.yml) 


## Deploy Procedure
1. Deploy Cluster
```
./scripts/ansible.sh

Choose Option 1 start_cluster
```

2. Go to http://yourdomain.com:8888/ for your access key.
   
WARNING:
The following deploys a box without TLS access, use this at your own risk.
