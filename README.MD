# Koii-Headless-v2.0
Big thanks for /schmiatz/koii-headless.git created project, as I have been creating a multiple KOII NODES previously, since recently it disappeared, I am returning it back with improvements.

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
git clone https://github.com/schmiatz/koii-headless.git
```

Switch into  the Repo and add your Ubuntu username into the group vars:  
Example for the Username "koii"
```
cd koii-headless
echo "koii_user: koii" > group_vars/all.yml
```

Put your Koii-Headless Server in the inventory.yml file.    
Example for a Server Named "koii-headless" with an IP-Address "192.168.1.10":   
```
all:
  hosts:
    koii-headless:
      ansible_host: 192.168.1.10
```

### Execute the Playbook
'cd' into the Repo and exectute the command 'ansible-playbook playbook.yml'

## And Now?

- Update the '.env.local File like its described here https://docs.koii.network/run-a-node/task-nodes/Running-on-VPS
- 'cd' into the Repo and execute 'docker compose up -d'

## How to interact now with the running Node?

execute the command 'create-task-cli' and you will see an interactive menu
```
? Select operation › - Use arrow-keys. Return to submit.
❯   Create a new task
    update existing task
    Activate/Deactivate task
    Claim reward
    Fund task with more KOII
    Withdraw staked funds from task
    upload assets to IPFS(metadata/local vars)
```
### Example how to claim rewards from the "Free Token Task!"
```
Calling ClaimReward
✔ Enter the task id … AXcd6MctmDUQo3XDeBNa4NBAi4tfBYDpt4Adxyai3Do3
✔ Enter the stakePotAccount address … FnQm11NXJxPSjza3fuhuQ6Cu4fKNqdaPkVSRyLSWf14d
✔ Enter the beneficiaryAccount address (Address that the funds will be transferred to) … 913eaKzZJNMNCEj3KPSJqjXRiDH5WMngXSnLfhgDZSAE
✔ Enter the path to Claimer wallet … /home/koii/koii-node/namespace/staking_wallet.json
Success
```
AXcd6MctmDUQo3XDeBNa4NBAi4tfBYDpt4Adxyai3Do3 represents the Task ID   
FnQm11NXJxPSjza3fuhuQ6Cu4fKNqdaPkVSRyLSWf14d represents the Task Creator ID   
913eaKzZJNMNCEj3KPSJqjXRiDH5WMngXSnLfhgDZSAE represents the Wallet that will receive the claimed Coins   
/home/koii/koii-node/namespace/staking_wallet.json represents the default path to your staking wallet   

Thats it, have fun
