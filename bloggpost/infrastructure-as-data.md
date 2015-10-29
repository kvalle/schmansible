Infrastructure as Data
======================

*Infrastructure as code* has long been a mantra in the world of provisioning. A newer trend, taking this one step further, is the idea of *infrastructure as data*.

Infrastructure as code typically saw you writing a task for each thing you wanted to provision. E.g. creating a file, starting a service, or as in the Ansible example below, managing a user.

```
- name: setup user ola
  user:
    name=ola
    password=secret
    state=present
    shell=/bin/bash
```

With infrastructure as data you extract the parts that varies

```
- name: setup user {{ item.username }}
  user:
    name={{ item.username }}
    password={{ item.password }}
    state=present
    shell=/bin/bash
  with_items: users
```

and instead put them in datastructures outside the tasks

```
users:
  - username: ola
    password: secret
  - username: helga
    password: secret
```

Pros and cons
=============

Extracts the business specific parts of the code
- Business "parts" changes at different rates than technical "parts"
- More DDD. Your domain should be clean and as free from dependencies to other technologies as possible

Reduces coupling with the framework
- Makes it easier to migrate away from your current framework

More readable abstractions
- Easier for people with little experience with the framework to contribute

Conclusion
==========

The ratio of effort spent on **configuration files**, **file templates** and **data structures** vs **provisioning tasks** is also a good indication of how good a framework is. Provisioning tasks are merely plumbing, and should just be the linking between business specific data and concrete infrastructure.
