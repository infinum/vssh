# vssh

vssh is a bash script that logs you into servers using vault as a certificate authority for the signing of ssh keys. Vault signs your key if you are a part of the correct Github team.

## Prerequisites

Github token with read:org permissions in environment variable GITHUB_TOKEN (It can also be added as an argument to the script)

Tools:
```
bash
curl
jq
```

## Installation

Clone the repo and move the vssh script somewhere in your $PATH. For example:

```sh
$ git clone git@github.com:infinum/vssh.git
$ sudo cp -a vssh/vssh /usr/local/bin/
```
... and you're good to go!

## Usage example

Use -h flag for help

```sh
$ vssh -h

    Usage: vssh [OPTIONS] USER@SERVER

    Options:
    -a, --address                Vault address. Default is https://vault.infinum.co:8200
    -k, --key                    Path to ssh key. Default is ~/.ssh/id_rsa. Script uses .pub key pair for signing and private key to initiate connection to the server
    -t, --token                  Github token used for auth. By default it is pulled from variable GITHUB_TOKEN
    -p, --port                   SSH port
    -s, --sign                   Sign a certificate and write it to temporary file. Don't login
    -h, --help                   Print this message

    Example:
    vssh user@123.45.67.89
    
```

## How it works

Script uses your Github auth token for authentication to Vault. When it authenticates you it tries to sign an ssh key which will let you log in to the server.
Your GitHub user needs to be a part of the project user team for you to be able to sign a key. Contact your PM to add you if you are not added already as a member. 
If the script fails to sign a key it will try to log you in using your private key (which will probably not work as we will get rid of all authorized_keys one day). If it succeeds it uses both your private key and signed key for logging you into the server. Every project user needs to have a signing role created on the Vault server. If that is not the case, contact DevOps in DevOps-hotline.