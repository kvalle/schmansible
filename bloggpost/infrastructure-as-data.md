Infrastructure as Data
======================

*Infrastructure as code* has long been a mantra in the world of provisioning. A newer trend, taking this one step further, is the idea of *infrastructure as data*.

This is hardly anything new. Separation of data and logic is an important principle in software development, and is second nature to most programmers. Lets see why it applies just as much when provisioning your infrastructure.

## Separate the data

Infrastructure as code typically saw you writing a task for each thing you wanted to provision. Examples include creating a file, starting a service, or, as in the [Ansible](http://www.ansible.com/) example below, managing users.

```yml
- name: setup user ola
  user:
    name=ola
    password=secret
    state=present
    shell=/bin/bash
- name: setup user helga
  user:
    name=helga
    password=secret
    state=present
    shell=/bin/fish
```

With infrastructure as data you extract the parts that varies from the code.

```yml
- name: set up user {{ item.username }}
  user:
    name={{ item.username }}
    password={{ item.password }}
    state={{ item.state }}
    shell={{ item.shell }}
  with_items: users
```

Instead, express those parts as pure datastructures outside the tasks.

```yml
users:
  - username: ola
    password: secret
    state: present
    shell: /bin/bash

  - username: helga
    password: secret
    state: present
    shell: /bin/fish
```


## What's the gain?

By separating the provisioning _logic_ from the provisioning _data_ you are effectively extracting the core parts of your infrastructure from the nitty gritty details of whatever framework you happen to be using. This way, the separation increases readability of the parts that matters the most.

Data and logic tend to change at different paces. By having them cluttered together you might have to change both, even if only one needs to change. Decoupling them enables you to to make changes only where needed.

Frameworks come and go. Ansible (or Puppet or any other framework) might not be the right choice for you in a month or a year. By having the framework parts separated you decrease your dependency on the framework, and thus make it easier to keep the important parts of your infrastructure if you decide to change framework. Don't throw the baby out with the bath water, keep your data.


## Who supports this?

### Ansible

[Ansible](http://www.ansible.com/) supports describing your infrastructure as data by default, as it is what Ansible was created for. With Ansible you put your data in YAML-files. You can differentiate between specific hosts or specific groups of servers (i.e. application servers or database servers).

### Salt

Salt from [SaltStack](http://saltstack.com/) also supports putting data in YAML-files. In this case using something called [Salt Pillars](https://docs.saltstack.com/en/latest/topics/tutorials/pillar.html).

To reuse the datastructure from the Ansible example we can define the following [Salt States](https://docs.saltstack.com/en/latest/topics/tutorials/starting_states.html):

```jinja
{% for user in pillar.get('users', {}) %}
{{user.username}}:
  user.{{user.state}}:
    - shell: {{user.shell}}
    - password: {{user.password}}
{% endfor %}
```

### Puppet

[Puppet](https://puppetlabs.com/) allows you to put your data in [Hiera](http://docs.puppetlabs.com/hiera/latest/), which is a lookup mechanism that deals with the differences between your hosts and environments.

Using Hiera you still get to use YAML, but it has to be restructured slightly:

```yaml
users:
  ola:
    password: secret
    ensure: present
    shell: /bin/bash

  helga:
    password: secret
    ensure: present
    shell: /bin/bash
```

Notice that we had to do two changes to conform to Puppets expectations:
- Change the name of the "state" attribute to "ensure"
- Convert the list of users to a hash of users instead with the username as key

Afterwards you can use the Puppet [user](https://docs.puppetlabs.com/puppet/latest/reference/type.html#user) resource to create the users.

```puppet
$users = hiera('users')
create_resources(user, $users)
```

## Pros and cons

Extracts the business specific parts of the code
- Business "parts" changes at different rates than technical "parts"
- More DDD. Your domain should be clean and as free from dependencies to other technologies as possible

Reduces coupling with the framework
- Makes it easier to migrate away from your current framework

More readable abstractions
- Easier for people with little experience with the framework to contribute


## Conclusion

The ratio of effort spent on **configuration files**, **file templates** and **data structures** vs **provisioning tasks** is also a good indication of how good a framework is. Provisioning tasks are merely plumbing, and should just be the linking between business specific data and concrete infrastructure.


