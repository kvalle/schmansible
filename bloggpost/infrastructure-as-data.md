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
- name: setup user kari
  user:
    name=kari
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

  - username: kari
    password: secret
    state: present
    shell: /bin/fish
```

## What's the gain?

By separating the provisioning logic from the provisioning data you are effectively extracting the core parts of your infrastructure—the parts that matters—from the nitty gritty details of whatever framework you happen to be using. This has several benefits:

- It will increase readability of the parts that actually describe your infrastructure. This will make it easier for people with little experience with the framework to contribute, or at least to understand what's going on.
- Removing the "how" from the "what" gives you a cleaner expression of your infrastructure. Design your solution [in terms of your domain](https://en.wikipedia.org/wiki/Domain-driven_design)—the infrastructure—rather than whatever technologies you use to realize it.
- Allow data and logic to change at different paces. You should not have to touch the logic in order to add a new user, or to change the data if you found a better way to perform some task. So let them live as separately as possible.
- It will reduce your dependency on the framework. Frameworks come and go: Ansible (or Puppet or any other framework) might not be the right choice for you in a month or a year. By having the framework specific parts separated you decrease your dependency on the framework, and thus make it easier to keep the important parts of your infrastructure if you decide to change it. Don't throw the baby out with the bathwater, keep your data!


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

To reuse our datastructure of users in Hiera, you would use something like the following.

```puppet
$users = hiera('users')
$users.each |$user| {
  user {$user['username']:
    password => sha1($user['password']),
    shell => $user['shell'],
    ensure => $user['state'],
  }
}
```

These are really simple examples though, and in the wild you might have to do a migration. But that is still far better than having to start from scratch!

## Conclusion

When writing provisioning code, the lion's share of your time should be spent on **data** such as configuration files, file templates and data structures. The **provisioning tasks** are merely plumbing, and should just be the linking between the data and a concrete infrastructure. (As a side note, the time spent on the former vs the latter is good indication of how good a framework is.)

Separating out the data will give you several benefits, and have no real disadvantages. And really, it's just about keeping your code (as well as your data) as clean as possible.
