## Docker deployment with ansible

### Clone the project [ansible-project]https://github.com/chriszal/ansible-project

```bash
git clone https://github.com/chriszal/ansible-project
```

Run the bellow commands to install the requirements 
```bash
python3 -mvenv venv
source venv/bin/activate
pip install -r requirements.txt
```
###  Modify the hosts.yml file 

Change ansible_host ip to the vm ip you want to deploy your project

### setup.yml description

    * Installing Docker and Docker Compose
    * Adding a special user for performing deploys
    * Generating key pairs for GitHub -> machine, and machine -> GitHub SSH access
    * Uploading deploy scripts
    * Copying SSH keys to operator’s machine to add to GitHub and CI
    * Adding GitHub’s servers to the known-hosts file for the deploy user

### clone-projects.yml description
    * Cloning the two projects
    * adding environment file

### deploy.yml description 
    * Changing into appropriate branches
    * Pulling new changes
    * Installing docker and docker-compose modules that we use
    * Down previous containers
    * Build and deploy new containers
    * Populate database
### Setup Remote machine
```bash
ansible-playbook -i ./hosts.yml -u <your SSH username> setup.yml 
```
### Add keys to github
Copy the contents of /keys/my-host/home/deploy/.ssh/referenceLetterApp.id_ed25519.pub. Add the key as a deploy key for your referenceLetterApp repository in the settings of your project at the depoy keys section. Do the same for referenceLetterAppPanel project.

Moving on to the private keys, go to Settings > Secrets of the referenceLetterApp repository to add a secret called SSH_KEY. The value should be the contents of /keys/my-host/home/deploy/.ssh/referenceLetterApp.id_ed25519. Again, do the same for referenceLetterAppPanel.
### Add .env file
Add your db information in the .env file
### Clone and deploy
```bash
ansible-playbook -i ./hosts.yml -u <your SSH username> clone-project.yml 

ansible-playbook -i ./hosts.yml -u <your SSH username> deploy-dockers.yml 

```