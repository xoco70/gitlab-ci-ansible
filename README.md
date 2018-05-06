# Ansible CI Playbook
This playbook will install Gitlab CI for ubuntu 16.04

## Requirements

You need at minimum 2 vm
  - 1 for gitlab-ce
  - 1 for gitlab-runner  

## Quick Start

To help you to start quickly, there are few entrey point to have your first playbook working. First, you need to add the targets you want to manage:

```
vim inventory/default.ini
```
Then add your target hosts under the ```[masters]``` and ```[runners]``` section.

```
ansible-playbook playbooks/0_local_requirements.yaml -i inventory/default.ini
```
Once you get there, you are sure you have all requirements to play with Ansible. Then, we will ensure your inventory is correct by trying to connect on all hosts. Obviously, this playbook will not modify at this stage your targets, it's just to be sure your inventory is ok.
```
ansible-playbook playbooks/1_target_test.yml -i inventory/default.ini
```

Download the roles: 

```
ansible-galaxy install -r roles/requirements.yml
```

Configure the gitlab endpoint, in `playbooks/vars/gitlab_master.yml`


Now you can install gitlab-ce
```
ansible-playbook playbooks/4_target_master.yml -i inventory/default.ini
```

Test the endpoint
 
Create a repository, and get the project token in project > Settings > CI / CD 

Configure the runner with `gitlab_runner_coordinator_url` and `gitlab_runner_registration_token` in `playbooks/vars/gitlab_runner.yml` 

Now install the runner
```
ansible-playbook playbooks/5_gitlab_runner.yml -i inventory/default.ini
```

You should now see your runner in gitlab UI

Limitations:
 - It would have been great to automate gitlab project creation, getting token, and inserting it in the second playbook 
 so that you could only run the main playbook 999 instead of running 2. This is not a fully automated install :(

- Please refer on documentation role for more configuration: 
  - https://github.com/geerlingguy/ansible-role-gitlab
  - https://github.com/riemers/ansible-gitlab-runner
  - https://github.com/geerlingguy/ansible-role-docker




