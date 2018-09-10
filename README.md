### This playbook currently supports builing VRF configurations for: ###
+ Cisco 4500x switches (non-sub-interface method)
+ Cisco core router sub-interface VRF transports
+ Cisco access switch VRF vlans and transports
+ Cisco 2911 routers
---
### Initial Setup ###
+ [Github Repository](https://github.com/devinbmiller/ansible-vrf)
1. Create a directory in your home directory on the jumphost or Ansible host of your choice. ex. `mkdir ansible`
2. Change to newly created directory and clone from the remote repository
   + `cd ansible`
   + `git clone https://github.com/devinbmiller/ansible-vrf.git vrf_config`
   + Change to cloned directory `cd vrf_config`

### Instructions: ###

#### 4500x Switches ####
Edit the hosts: `hosts` file
+ Under `[4500-routers]` section:
   - Add a single entry for each 4500 router
   - Please make sure one of the hosts ends in a 1 to identify it as router 1
   
Create Configs:
1. Edit the `group_vars/4500-routers.yml` file
   + This contains variables that are shared for both 4500x switches
   + The variable names should be self-describing
   + You can add as many VRF configurations as you wish
2. Generate the 4500x router configurations by specifying its tag (build-4500):
   + Run the command `ansible-playbook -i hosts create-config.yml --tags 'build-4500'`
3. Configurations will be written to the `configs/` directory
   + Check configurations for accuracy
   + Follow normal workflow for deploying them to routers
   + Previous configurations will be backed up if they already exist before new, changed ones are generated

#### Core Router Sub-Interface VRF Transport Configurations ####
Edit the hosts: `hosts` file
+ Under `[core-routers]` section:
   - Add entry for each core router.
   - The template requires that 'TX' or 'OPS' be in each hostname

Create Configs:
1. Edit the `group_vars/core-routers.yml` file
   + This contains the variables used to generate one or more VRF sub-interface configurations per core router
   + The variable names should be self-describing
2. Generate the Core router configurations by specifying its tag (build-cores):
   + Run the command `ansible-playbook -i hosts create-config.yml --tags 'build-cores'`
3. Configurations will be written to the `configs/` directory
   + Check configurations for accuracy
   + Follow normal workflow for deploying them to routers
   + Previous configurations will be backed up if they already exist before new ones are generated

#### Access Switch VRF Configurations ####
Edit the hosts: `hosts` file
+ Under `[access-switches]` section:
   - Add entry for each access-switch.

Create Configs:
1. Edit the `group_vars/access-switches.yml` file
   + This contains the variables used to generate one or more VRF configurations per access switch
   + The variable names should be self-describing
2. Generate the access switch configurations by specifying its tag (build-access):
   + Run the command `ansible-playbook -i hosts create-config.yml --tags 'build-access'`
3. Configurations will be written to the `configs/` directory
   + Check configurations for accuracy
   + Follow normal workflow for deploying them to routers
   + Previous configurations will be backed up if they already exist before new ones are generated
  
#### 2911 Routers ####
Edit the hosts: `hosts` file
+ Under `[2911-router]` section:
   - Do nothing or comment out `router2`
   - Do NOT modify the names `router1` and `router2`
+ Dual router deployments:
   - Make sure `router1` and `router2` are uncommented

Create Config:
1. Edit the `group_vars/2911-router.yml` file
   + This contains variables that are shared across single/dual router scenarios
   + The variable names should be self-describing
   + Set `dual_routers` to `True` for dual router deployments; otherwise set to `False`
2. Generate the configurations by running a task by specifying its tag (build-2911):
   + Run the command `ansible-playbook -i hosts create-config.yml --tags 'build-2911'`
3. Configurations will be written to the `configs/` directory
   + Check configurations for accuracy
   + Follow normal workflow for deploying them to routers
   + Previous configurations will be backed up if they already exist before new ones are generated
---
### Assumptions/Caveats: ###
1. This will generate vrf configurations in `configs/ `for one or two 2911 router deployments or two 4500x switch scenarios
2. In a single router scenario, if you do not uncomment `router2` from the `hosts` file, a second configuration file will be generated in `configs/` but can be deleted or ignored
---
### Warning: ###
+ These scripts will NOT check to make sure you entered the correct or accurate values for the variables.
+ It is the user's responsibility to check the output configurations to make sure they are acceptable and will not cause issues.
---
### Contributing ###
+ For any bugs, corrections, or requests submit via [Github Repository Issues](https://github.com/devinbmiller/ansible-vrf/issues)
+ Feel free to clone/fork the project, create a branch, add features, then submit pull-request
