# F5 Upgrader

## Requirements

The F5 Ansible collection is required. 

To install the most recent version run `ansible-galaxy collection install f5networks.f5_modules`

See https://clouddocs.f5.com/products/orchestration/ansible/devel/usage/getting_started.html for details.

## Configuration

Configuration (as provided) is done in the hosts.ini file.

Each host needs a "reboot group" variable. This allows for clusters to be upgraded one at a time for a rolling upgrade. Set to 0 to disable the activation task, leaving the new image uploaded and installed ready for a manual activation and reboot.

Adjust the image name and location (on local system) to suit.

If you want to install the new image to a definite boot location then define the variable `dest_boot_loc` to have the name of the desired boot location. If not defined then playbook will check the first two boot locations and pick the first one which is not currently active.

## Usage

`ansible-playbook -i hosts.ini upgrade.yaml`

## Error Conditions

The playbook will exit on any failure in the softwware activation tasks. This is to avoid taking out both members of a cluster.

Investigate any hosts that have reported a problem, and re-run the playbook to continue. It will move quickly through tasks that were already done and proceed with remaining upgrades.

I have found an issue where the `bigip_software_install` tasks can fail while waiting for the management interface to be available after a reboot. Increasing the timeout in the provider did not resolve this.