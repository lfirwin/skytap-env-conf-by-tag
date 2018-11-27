# Get Skytap Configuration for an Environment by a Tag

## Problem
Get the environment configuration in Skytap without having to store and track the environments by config id, and do it using only native Ansible modules.

## Assumption(s)
The automation that creates the environment adds a tag to it with a given name, and that tag is unique among all environments.  This means there is a controlled way to create and delete the environment that ensures that environment tag uniqueness.

If that control isn't in place, then the first environment encountered with the tag will be returned.

It is also assumed the credentials used to access Skytap via REST API has visibility to the desired enviornment through either direct ownership or as a member of a project to with the environment has been added.  In other words, the environment is visible to the credentials executing the REST API calls.

## Solution
Using the Ansible *uri* module, get all the metadata for the environments and iterate over each environment matching the environment name against the list of tags defined for that enviornment.

When the environment name matches a tag, set the *env_id* variable with the environment's id.

When there is no match, the play will fail.

With the config id for the environment, pull the configuration data structure for it.

The environment name must be defined in host, or group vars, set as a fact before including this task file, or passed in via --extra-vars (--extra-vars "env_name=dev1").

## Notes
The credential variables are in *vars/skytap.yml*.  Update them to access your Skytap infrastructure.

The task file can be extended for whatever purpose necessary.  For exmaple, this set of tasks was taken from a set of tasks to find NAT IPs for a given environment.

And as always if storing credentials in source code repos, consider using ansible-vault to encrypt sensitive information.
