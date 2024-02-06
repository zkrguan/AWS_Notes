# EC2

## How to deploy a node server to the EC2 instance from the terminal?

### First access to the fresh instance and set it up

Terminal used here Git Bash.

SSH is the tool used to connect the virtual machine.

.pem was used for the public key format.

```bash
# IPV4 address also works
ssh -i {path to the .pem file}.pem ec2-user@{ec2-public-IPV4-dns}

```

This is like you bought an Alienware Gaming Laptop, but you still need to update while you first turn on that device.

```bash
# My senpai likes yum, so I will stick with yum here
sudo yum update
```

Different Linux Distributions include different pre-installed softwares. If you don't know if they installed them or not, just use which command

```bash

which vim

which nvim

which emacs

```

If you need to install tools, just use yum to install.

```bash

sudo yum install git -y

```

some softwares like node, you need to instal them in a different way like this

Firstly you install node version manager

```bash

# This is literally fetch a bash script
# Save it as shell script and run it 
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash

```

Then you install the node lts using NVM

```bash
nvm install --lts

# Downloading and installing node v18.13.0...
# Downloading https://nodejs.org/dist/v18.13.0/node-v18.13.0-linux-x64.tar.xz...
######################################################################### 100.0%
# Computing checksum with sha256sum
# Checksums matched!
# Now using node v18.13.0
# Creating default alias: default -> 18 (-> v18.13.0)


# if you have to work on some old server, example like this
nvm install 16

# If you need to switch between node versions

nvm use --lts

#Now using node v18.13.0

nvm use 16

#Now using node v16.19.0

```

### Pack the local node package and send it to the server

npm will generate a tgz file which is like a compressed zip file in windows


#### on you local device

```bash

npm pack

```

then send it to the server by using scp

```bash

# don't miss the colon :
scp -i {path-to-your-key}.pem {the-compress-file}.tgz ec2-user@{public-ipv4-dns}:{path-on-your-ec2-instance-where-you-save-the-tgz}

```

#### on your virtual machine

```bash

# change the pwd to here
cd {to the path where you upload the file}

# Check if you uploaded to the right place
ls -al 


# using tar to unpack the compressed file 
# x (extract),
# v (be verbose and print details about what's happening), 
# z (use gzip to decompress) 
# f #(specify a file):
tar -xvzf {compressed-file-name}.tgz


# The command above will generate a dir called package
mv package {your-desired-name}

```

### use scp to upload the .env file to the project dir

Just use the scp commands from the above again.

### npm install and start the server

This part you must know how !!!
