### This playbook currently supports building VRF configurations for: ###
* Cisco 2911 Routers

---
### Instructions: ###

Hosts: `hosts` file
+ Single router deployments:
   - Do nothing or comment out `router2`
   - Do NOT modify the names `router1` and `router2`
+ Dual router deployments:
   - Make sure `router1` and `router2` are uncommented

Create Config:
1. Edit the `group_vars/2911-router.yml` file
   + This contains variables that are shared across single/dual router scenarios
   + The variable names should be self-describing
   + Set `dual_routers` to `True` for dual router deployments; otherwise set to `False`
2. Edit the `host_vars/router1.yml / host_vars/router2.yml` files
   + This contains variables that are unique to each router depending on scenario
   + The variable names should be self-describing
   + The `router_name` variable will be used in the resulting configuration(s) file names
3. Generate the configurations:
   + Run the command `ansible-playbook -i hosts create-config.yml`
4. Configurations will be written to the `configs/` directory
   + Check configurations for accuracy
   + Follow normal workflow for deploying them to routers

Push Config:
1. This has not yet been implemented

---
### Assumptions/Caveats: ###
1. This will generate vrf configurations in `configs/ `for one or two 2911 router deployment scenarios
2. In a single router scenario, if you do not uncomment `router2` from the `hosts` file, a second configuration file will be generated in `configs/` but can be deleted or ignored
3. If you do not change the `router_name` variables for each router then previous configuration outputs might be overwritten

---
### Warning: ###
These scripts will NOT check to make sure you entered the correct or accurate values for the variables.

It is the user's responsibility to check the output configurations to make sure they are acceptable and will not cause issues.

---

TODO

*Add Section about runnning playbook for specific tags*
