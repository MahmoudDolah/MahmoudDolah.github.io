---
layout: post
title:  "Writing Clean and Repeatable Ansible Playbooks"
date:   2020-02-07 06:39:32 -0400

categories: DevOps Ansible Automation Configuration-Management
---

While Ansible is no longer on the cutting edge, it still maintains a significant degree of [popularity][stack-overflow]. Personally, it is one of my favorite tools and was one of the first SRE-related skills that I acquired.     

I worked on a myriad of different projects, from creating playbooks from scratch to automate tasks, to building on top of and debugging playbooks required for multi-tiered application deployments

While I still have a lot to learn, here are my main takeaways so far for writing good Ansible:

## Use the Ansible Modules

A lot of people (like myself), first get introduced to Ansible as a way of automating Bash scripts. While this is a great entryway into showing the power of the tool, it can lead to over-reliance of the [`command`][ansible-command] or [`shell`][ansible-shell] modules     
While these modules have their value, it's better practice to use a dedicated module for the task       

For example, let's say you need to download a file and update some permissions in it
Here's an example with `shell`:

```yaml
- name: Download latest package
  shell:
      chdir: /tmp
      cmd: wget https://super-awesome-site/super-awesome-file.tar.gz

- name: Change owner of tar file
  shell:
      chdir: /tmp
      cmd: chown admin:admin super-awesome-file.tar.gz
```

The module is extensible enough where you can change the directory to run the command, but updating the permissions requires a second task. Alternatively, you can combine the commands using the `&&` operator, but it makes the `cmd` line very long and a bit unwieldy

Here's what it looks like when you use the [`get_url`][ansible-get-url] module:

```yaml
- name: Download latest package
  get_url:
      dest: /tmp
      url: https://super-awesome-site/super-awesome-file.tar.gz
      owner: admin
      group: admin
      timeout: 10
```

Not does the module give you the ability to download the file and update the permissions in one task, but you can also set a timeout, the http-agent used to download the file, and even validate the file against a checksum all in one task

## Take Advantage of Error Handling

Let's say you need to check that a service is running

```yaml
- name: Check service is running
  service_facts:
  register: services_state
```

The above task will get all the facts about all of the services on a server and store them in the variable `services_state`    
It can certainly get the job done, but let's try and be a bit more thorough

```yaml
- name: Check service is running
  service_facts:
  register: services_state
  until: services_state.ansible_facts.services[my_service].state == 'running'
  retries: 5
  delay: 30
  when: services_state.ansible_facts.services[my_service].state != 'running'
  ignore_errors: yes
```

The updated task not only stores the state of the services in a variable, but it will retry 5 times (with a 30 second delay in between) if the service is not running on the instance.    
This can be helpful if you're in a situation where the playbook will run repeatedly in an automated fashion. An engineer might be on the instance and temporarily stop the service to perform some quick maintenance or debugging and adding in the `retry` and `delay` conditions provides some leeway

## Make Playbooks Repeatable

For the final takeaway, let's say you're writing a playbook to download a compressed file and then extract it

```yaml
- name: Create directory for project
  file:
    path: /home/admin/work
    state: directory
    owner: admin
    group: admin

- name: Download latest package
  get_url:
      dest: /home/admin/work
      url: https://super-awesome-site/super-awesome-project.tar.gz
      timeout: 10

- name: Extract tar file
  unarchive:
    src: /home/admin/work/super-awesome-project.tar.gz
    dest: /home/admin/work
    remote_src: yes
    owner: admin
    group: admin

- name: Run make on the project
. . .
```

If you run the above tasks multiple times and have it fail on a task that changes the timestamp on the files, you'll notice that the new tar files that get downloaded won't be extracted because, by default, it will check the timestamp and not replace all the files     
There could also be an instance where an engineer could have manually performed some actions    

To cover these bases, you could start the playbook with something as simple as deleting the `work` directory

```yaml
- name: Clean up directory
  file:
    state: absent
    path: /home/admin/work

- name: Create directory for project
  file:
    path: /home/admin/work
    state: directory
    owner: admin
    group: admin
. . . 
```


[stack-overflow]: https://insights.stackoverflow.com/survey/2019#technology-_-other-frameworks-libraries-and-tools
[ansible-command]: https://docs.ansible.com/ansible/latest/modules/command_module.html#command-execute-commands-on-targets
[ansible-shell]: https://docs.ansible.com/ansible/latest/modules/shell_module.html
[ansible-get-url]: https://docs.ansible.com/ansible/latest/modules/get_url_module.html
