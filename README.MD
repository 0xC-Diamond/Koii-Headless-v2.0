# Koii-Headless-v2.0
Big thanks to **/schmiatz/**, who created project: koii-headless.git, as I have been creating a multiple KOII NODES previously and running smoothly, since recently it disappeared, I am returning it back with improvements. :)

## What does this Repo?

This Repo contains an ansible-playbook that installs:
- Docker
- Docker Compose
- koii-cli (https://docs.koii.network/develop/command-line-tool/koii-cli/install-cli)
- koii-headless-node (https://github.com/koii-network/task-template)
- npm
- nodejs 21
- koii-create-task-cli

## What are the requirements to run this Playbook?

You need a Ubuntu 20 Server (22 and 24 will follow).   

You need to have ansible installed on a Machine where you want to run the Playbook from.   
### How to install ansible on Ubuntu Server 20?

```
sudo apt update
sudo apt upgrade
sudo apt -y install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
sudo apt install ansible
```

You need to have ssh access with root permissions to the Machine you want to be installed by ansible.   
You need to specify the Variable `koii_user` in the cloned Repos group_vars/all.yml file.   

## How to run the Playbook?

### Prepare the Playbook

Git Clone this Repo:      
```
git clone https://github.com/0xC-Diamond/Koii-Headless-v2.0.git
```

Switch into the Repo and add your Ubuntu username into the group_vars:  
Example: username "koii", replace it in the echo command!
```
cd Koii-Headless-v2.0
echo "koii_user: koii" > group_vars/all.yml
```

Put your Koii-Headless Server IP in the inventory.yml file. 

NB! uncomment last 2 lines, so it is according to the following:
Example for a Server Named "koii-headless" with an IP-Address "192.168.1.10":   
```
nano inventory.yml

all:
  hosts:
    koii-headless:
      ansible_host: 192.168.1.10
```
### Make sure that your User has ssh permissions to execute Playbook files!
Debug the SSH Connection
```
ssh -v user@hostname(IP)
```
Generate a new key if you do not have one already defined.
```
ssh-keygen -t rsa -b 4096
```
Check the sshd Configuration have you allowed Password authentification:

For this, check the /etc/ssh/sshd_config file for the following lines:
```
sudo nano /etc/ssh/sshd_config
```
Set these variables (uncomment if commented = remove # in beginning of the line)

PasswordAuthentication yes

PubkeyAuthentication yes

```
sudo service ssh restart
```
How do I check the permissions and ownership of the authorized_keys file
```
ls -l ~/.ssh/authorized_keys
```
If need to give correct permission: 600 means you can read and write the file or directory and other users have no access to it.
```
sudo chmod 600 ~/.ssh/authorized_keys
```
If you have configured and allowed the ssh for the user, try reboot the server first
```
sudo reboot
```


### Execute the Playbook
Switch into the Repo and exectute the command 'ansible-playbook playbook.yml'
```
cd Koii-Headless-v2.0
ansible-playbook -v playbook.yml -kK
```
## And Now?
- Update the `.env.local.example` File like its described here in the https://github.com/koii-network/task-template check the file itself: `.env.local.example` also pause on the video if needed to see what is the configuration setup.

### NB! First of all initially created file are called '.env.local.example', run the commands to rename to match the configuration and then adjust the file accordingly.
```
cd koii-node
mv .env.local.example .env-local
nano .env-local
```

## Create a wallet and top up with some KOII from the faucet OR use existing wallet key file(id.json)
Suggest to use guide here: https://docs.koii.network/run-a-node/task-nodes/Running-on-VPS#set-up-new-koii-pubkey
```
koii config set --url https://testnet.koii.live
```
Then you will have your new pubkey, **transfer some tokens to this account using [Finnie Wallet](https://chromewebstore.google.com/detail/finnie/cjmkndjhnagcfbpiemnkdpomccnjblmj)** or get couple of $KOII Tokens from the Faucet.

*Suggestion*: Use some FileManager to connect to your VPS and download id.json file and make an “Import a key file” in the Finnie Wallet settings page. In order to have your Main node wallet right in front of you if you would like to check the Balance/Transfer KOII without going into the VPS server.

**Make sure that you have a $KOII Balance in your wallet before proceeding, so the Staking account can be created and transactions can go through**

## Finally it is time for the LAUNCH: 3...2...1...GO!
Switch into the Repo and execute 'docker compose up -d'
```
cd koii-node 
docker compose up -d
```

Verify whether all is good and the task are performing action and there is no errors:
```
docker logs -f --tail 10 task_node
```

CONGRATULATIONS!

## How to interact now with the running Node?

execute the command 'create-task-cli' and you will see an interactive menu
```
create-task-cli
```
```
? Select operation › - Use arrow-keys. Return to submit.
❯   Create a new task - **DO NOT INTERACT all the changes should be made in .env-local configuration file!**
    update existing task - **DO NOT INTERACT all the changes should be made in .env-local configuration file!**
    Activate/Deactivate task - **DO NOT INTERACT all the changes should be made in .env-local configuration file!**
    Claim reward
    Fund task with more KOII - **DO NOT INTERACT all the changes should be made in .env-local configuration file!**
    Withdraw staked funds from task
    upload assets to IPFS(metadata/local vars) - **DO NOT INTERACT all the changes should be made in .env-local configuration file!**
```
### Example how to claim rewards from the "Free Token Task!"
```
Calling ClaimReward
✔ Enter the task id … 6GbpHRK3duDbo3dCEFXuJ2KD5Hg6Yo4A9LyHozeE7rjN
✔ Enter the stakePotAccount address … FnQm11NXJxPSjza3fuhuQ6Cu4fKNqdaPkVSRyLSWf14d
✔ Enter the beneficiaryAccount address (Address that the funds will be transferred to) … 5aU9fdhjpe2sZKzZ858tV1ZDGjht1Q1UQewEphdo5meZ
✔ Enter the path to Claimer wallet … /home/koii/koii-node/namespace/staking_wallet.json
Success
```
6GbpHRK3duDbo3dCEFXuJ2KD5Hg6Yo4A9LyHozeE7rjN represents the Task ID   
FnQm11NXJxPSjza3fuhuQ6Cu4fKNqdaPkVSRyLSWf14d represents the Task Creator ID   
5aU9fdhjpe2sZKzZ858tV1ZDGjht1Q1UQewEphdo5meZ represents the Wallet that will receive the claimed Coins   
/home/koii/koii-node/namespace/staking_wallet.json represents the default path to your staking wallet   

### Example how to Unstake from the "Free Token Task!"
```
1. Calling Withdraw staked funds from task
✔ Enter the task id … 6GbpHRK3duDbo3dCEFXuJ2KD5Hg6Yo4A9LyHozeE7rjN
✔ Enter the submitter wallet path address … /home/koii/koii-node/namespace/staking_wallet.json
Gives error but don't worry, follow on with next step ->

2.Calling ClaimReward
✔ Enter the task id … 6GbpHRK3duDbo3dCEFXuJ2KD5Hg6Yo4A9LyHozeE7rjN
✔ Enter the stakePotAccount address … stakepotaccountUSDX2sDSAcq8ZEvA3Tiv7tRpJ1Dv
✔ Enter the beneficiaryAccount address (Address that the funds will be transferred to) … YOUR-ADDRESS
✔ Enter the path to Claimer wallet … /home/koii/koii-node/namespace/staking_wallet.json
Success
```
6GbpHRK3duDbo3dCEFXuJ2KD5Hg6Yo4A9LyHozeE7rjN represents the Task ID   
stakepotaccountUSDX2sDSAcq8ZEvA3Tiv7tRpJ1Dv represents the Stake Pot Account    
YOUR-ADDRESS represents the Wallet that will receive the claimed Coins   
/home/koii/koii-node/namespace/staking_wallet.json represents the default path to your staking wallet 

## Additional helpful commands you should know
```
koii -u https://testnet.koii.live balance
koii --version
koii address
```

## Tasks IDs for different tasks running on nodes:

Task name	Task ID

*Free Token Task!*	~6GbpHRK3duDbo3dCEFXuJ2KD5Hg6Yo4A9LyHozeE7rjN~ -> NEW Task ID from 15.02.2024= 4mFgWD5GwGTrhc7vBXQnNLkFFrsMduSEa8aBTWwtfLdk

*Arweave Verifier*	BwwkRHB1GVWvVDbdXfUobRFLfz7wz6tz2u2W8mKCUjsj

*Inflation Monitoring*	EN4CA8EuLzUJGDC8p6WwCqCniwGB99sXh7Ma2WBKDL9o 

*Big Big Task*	tqhFcPKvcWPagNWNEBC6LtpFujboi7FigVoVUw2Ph8n 

*Adot Search AI*	5hoMYrrdPQneqsQyRGzmoHvKEbR5kA1dN6jdUWFnhgVH 

*#koii Twitter Archive*	9Vfa7Jowd3q7xdiV4hSZt1t5L9PWXjFWb6VuLbUXPwUS

*Archive Twitter*	Hcpv6q57mcG9fGca9xMoivMuCVNWZd9atxRQfcde6Edb

*Mask Social Feeds*	7NvPT3RC24NhgMZZSW1Ke5WKcVLwQgwRbYfVYsvkJobH



Thats it, have fun!
