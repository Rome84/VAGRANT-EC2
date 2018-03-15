# VAGRANT-EC2
Vagrant is an open source tool for building and distributing virtual development environments. It provides framework to manage and create complete portable development environments.

Vagrant machines are provisioned on the top of VirtualBox, VMware, AWS, or any other provider supported by vagrant.This blog illustrates how we can launch and provision instances in EC2 using AWS provider supported by Vagrant.

Major advantage of using Vagrant to deploy AWS EC2 is that we can test our provisioning scripts in the actual environment where it will be deployed for production before deploying on actual EC2 machines.

Follow the following steps to configure an EC2 machine using vagrant:

1. Install vagrant-aws plugin.

1
$vagrant plugin install vagrant-aws
2. Fetch a Vagrant box image
Box images vary depending on the Vagrant “provider” that we use. Run the following command to download the dummy box which is provided by Vagrant-aws plugin:

$vagrant box add dummy https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box
3.Configure Vagrant file
Make a directory to hold your Vagrant machine metadata.

# Run the following commands:
$ mkdir aws-demo
$ cd aws-demo
$ vagrant init

This will create a default Vagrant file in the present working directory which will be used to configure the vagrant machine. Edit this file as follows to specify the provider and configuration
require 'vagrant-aws'

Vagrant.configure('2') do |config|
    config.vm.box = 'dummy'
    config.vm.provider 'aws' do |aws, override|
    aws.access_key_id = “xxxxxxxxxxxxxxxxxxxxxxxxxxx”
    aws.secret_access_key = “xxxxxxxxxxxxxxxxxxxxxxxxxx”
    aws.keypair_name = 'ssh-keypair-name'
    aws.instance_type = "t2.micro”
    aws.region = 'us-east-1'
    aws.ami = 'ami-20be7540'
    aws.security_groups = ['default']
    override.ssh.username = 'ubuntu'
    override.ssh.private_key_path = '~/.ssh/ssh-keypair-file'
  end
end

4. Launch the instance that is configured by the Vagrant file by running the following command:

$vagrant up --provider=aws
5. Login to the AWS EC2 instance:
$vagrant ssh

The above setup will successfully provision a t2.micro EC2 machine via vagrant.
You can use this setup and integrate with other Vagrant provisioners such as Chef, Ansible etc. which will automatically install software and alter configurations directly on EC2 machines.
