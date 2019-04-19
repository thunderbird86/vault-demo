# Vault Demo

## Commands
- vault server -dev > /dev/null 2>&1 &
- vault secrets enable -path=ssh ssh
- vault write ssh/config/ca generate_signing_key=true
- vault write ssh/role/demo -<<"EOH" 
- vault write -field=signed_key ssh/sign/demo public_key="@${HOME}/.ssh/bastion.pub" > ~/.ssh/bastion-cert
- vault ssh -mode=ca -role=devops user@bastion_ip

#### vagrantfile
```
Vagrant.configure(2) do |config|
    config.vm.define "ubuntu" do |ubuntu|
        ubuntu.vm.box = "ubuntu/xenial64"
        ubuntu.vm.hostname = "ubuntu"
    end
    config.vm.define "centos" do |centos|
        centos.vm.box = "centos/7"
        centos.vm.hostname = "centos"
    end
end
```

#### policy.json
```json
{
  "allow_user_certificates": true,
  "allowed_users": "*",
  "default_extensions": [{
    "permit-pty": "",
    "permit-port-forwarding": "",
    "permit-agent-forwarding": ""
  }],
  "key_type": "ca",
  "default_user": "ubuntu",
  "valid_principals": [
    "devel",
    "vagrant",
    "ops"
  ],
  "ttl": "2m0s"
}
```
# Additional Info
## AWS Bastion 
https://github.com/thunderbird86/tf-aws-ca-bastion
## VPN over SSH
https://github.com/apenwarr/sshuttle
https://sshuttle.readthedocs.io/en/stable/