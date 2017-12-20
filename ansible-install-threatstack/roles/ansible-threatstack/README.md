Threat Stack Ansible Role
=========

Ansible Role to deploy the Threat Stack server agent.

Platforms
---------

* Amazon Linux
* CentOS
* RedHat
* Ubuntu

Role Variables
--------------
The following variables are available for override.
```
threatstack_deploy_key:         # Required. Your Cloud Sight API Key
threatstack_feature_plan:       # Set value to reflect your feature plan. https://www.threatstack.com/plans
                                # * 'agent_type="i"' - Investigate or Legacy (Basic, Advanced, Pro)
                                # * 'agent_type="m"' - Monitor
threatstack_ruleset:            # The Agent's rule set, will default to "Default Rule Set".
                                # Define multiple rule sets using a comma seperated list.
threatstack_pkg_url:            # Location of package repo. Only change if you mirror your own.
threatstack_pkg:                # name of package. Specify package version using "threatstack-agent=X.Y.Z"
threatstack_hostname:           # The display hostname in the Threat Stack UI
threatstack_configure_agent:    # Optionally do not configure the host, just install package
threatstack_agent_config_args:  # Pass optional configuration arguments during agent registration.
```

Examples
----------------
1) Install Threat Stack agent with the default rule set and reports system hostname to threatstack. This is the most basic configuration
```
- hosts: all
  roles:
    - { role: threatstack.threatstack-ansible, threatstack_deploy_key: XXXXXXXXXXXXX}
```

2) Install Threat Stack agent with custom security rules set and custom hostname:
```
- hosts: web-servers
  roles:
    - role: threatstack.threatstack-ansible
      threatstack_deploy_key: XXXXXXXXXXXXX
      threatstack_ruleset: "Base Rule Set, Custom Rule Set"
      threatstack_hostname: dev_web01_us-east-1c
```

3) Install the Threat Stack agent but do not configure it.  __NOTE: Useful for configuring a base image to be repeatedly deployed with the agent pre-installed.__
```
- hosts: aws-image
  roles:
    - role: threatstack.threatstack-ansible
      threatstack_configure_agent: false
```

4) Install a particular version of the Threat Stack agent.  Use in situations where you perform controlled rollouts of all new package versions.
```
- hosts: hosts
  roles:
    - role: threatstack.threatstack-ansible
      threatstack_deploy_key: XXXXXXXXXXXXX
      threatstack_pkg: threatstack-agent=1.4.4.0ubuntu14.0
```


