# Meeting Four (More Ansible)

## Questions That Will be Answered
* What is an Ansible role?
* Can we break our project into roles?
* What is Jinja2 and how do templates work? 
* What are Ansible conditionals? (when, ignore errors, etc.)
* What is Ansible Galaxy?

## Ansible Roles

Roles provide a framework for fully independent, or interdependent collections of variables, tasks, files, templates, and modules.

In Ansible, the role is the primary mechanism for breaking a playbook into multiple files. This simplifies writing complex playbooks, and it makes them easier to reuse. The breaking of playbook allows you to logically break the playbook into reusable components

Lets break out the playbook we used last time into a role
```
- hosts: ansibleproject
  become: yes
  tasks:
   - name: Upgrade all packages to the latest version
     package:
      name: "*"
      state: latest

   - name: Install packages
     package:
      name: "{{ packages }}"
     vars:
      packages:
       - vim
       - git
       - nginx 
       - lolcat

   - name: Ensure SSH Protocol is set to 2
      lineinfile:
        state: present
        dest: /etc/ssh/sshd_config
        regexp: '^Protocol'
        line: 'Protocol 2'
     notify: reload sshd

  handlers:
    - name: reload sshd
      service:
        name: ssh
        state: restart

```

To make the Ansible role directory structure run

    ansible-galaxy init <role-name>
    tree <role-name>

Ansible will always run the main.yml first in each directory. Place the respective parts into each directory of the role. 

Now we need to call the role into our playbook. 

```

- hosts: server1.example.com
  user: ansible
  become: true

  roles:
    - <role-name>
      
```

## Templating with Ansible
Ansible uses Jinja2 templates to add some modularity to templating. We have see that we can call variables using {{}}, but we can also do this inside of template files. Here is an example of what that would look like.

```
{% if security_sshd_disallow_empty_password | bool %}
# V-71939 / RHEL-07-010440
PermitEmptyPasswords no
{% endif %}
{% if security_sshd_disallow_environment_override | bool %}
# V-71957
PermitUserEnvironment no
{% endif %}
{% if security_sshd_disallow_host_based_auth | bool %}
# V-71959
HostbasedAuthentication no
{% endif %}
# V-72221
Ciphers {{ security_sshd_cipher_list }}
# V-72225
Banner {{ security_sshd_banner_file }}
# V-72237
ClientAliveInterval {{ security_sshd_client_alive_interval }}
# V-72241
ClientAliveCountMax {{ security_sshd_client_alive_count_max }}
{% if security_sshd_print_last_log | bool %}
# V-72245
PrintLastLog yes
{% endif %}
{% if security_sshd_permit_root_login | string in ['False', 'True', 'without-password', 'prohibit-password', 'forced-commands-only', 'no', 'yes' ] %}
{%   if security_sshd_permit_root_login | string in ['False', 'True'] %}
{%     set _security_sshd_permit_root_login = ((security_sshd_permit_root_login | bool) | ternary('yes','no')) %}
{%   else %}
{%     set _security_sshd_permit_root_login = security_sshd_permit_root_login %}
{%   endif %}
# V-72247
PermitRootLogin {{ _security_sshd_permit_root_login }}
{% endif %}
{% if security_sshd_disallow_known_hosts_auth | bool %}
# V-72249 / V-72239
IgnoreUserKnownHosts yes
{% endif %}
{% if security_sshd_disallow_rhosts_auth | bool %}
# V-72243
IgnoreRhosts yes
{% endif %}
{% if security_sshd_enable_x11_forwarding | bool %}
# V-72303
X11Forwarding yes
{% endif %}
# V-72251
Protocol {{ security_sshd_protocol }}
# V-72253
MACs {{security_sshd_allowed_macs }}
{% if security_sshd_enable_privilege_separation | bool %}
# V-72265
UsePrivilegeSeparation sandbox
{% endif %}
# V-72267
Compression {{ security_sshd_compression }}
{% if security_sshd_disable_kerberos_auth | bool %}
# V-72261
KerberosAuthentication no
{% endif %}
{% if security_sshd_enable_strict_modes| bool %}
# V-72263
StrictModes yes
{% endif %}

```

Just for example, make a variable in the vars main file that says Hello World.

    hw = 'Hello World'

Now create a file within templates and call it hello_world.j2. the contents should look like this

    "{{ hw }}"

Now add this to a playbook and run it.
```
tasks:
    - name: Ansible Template Example
      template:
        src: hello_world.j2
        dest: ~/hello_world.txt
```

Now log in and the contents of the file should say Hello World. Jinja2 also allows us to use loops within templates to paste multiple things in a line. Here is an example.

```
Jinja_loop.j2
-------------
Ansible template for loop example
{% for i in range(3)%}
  This is the {{ i }}th variable
{% endfor %}

output
------
cat hello_world.txt
Ansible template for loop example
  This is the 0th variable
  This is the 1th variable
  This is the 2th variable
```

## Exercise 1 and Friday
* Take 20 CIS Benchmarks and create tasks for them
* Make a jinja2 template for a file
* Use at least one conditional statement
* Make this into a role

