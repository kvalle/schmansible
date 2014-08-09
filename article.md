# Provisioning a Conference

> TODO: Introduction about who we are, what JavaZone and Ansible is, and what we want to communicate with this article.


## The JavaZone Systems

First off, lets have a look at the systems needed to make JavaZone happen.

- javazone.no frontend: The actual javazone.no frontpage is the part which most people see. It consists of pretty simple HTML/CSS/JS.
- javazone.no backend: The frontend talks to a small Java webapp responsible for caching tweets, the program, and some other stuff.
- Event Management System (EMS): EMS is one of the core systems in that it stores the actual program and suggested talks. It is written in Scala, and uses MongoDB to store the events.
- SubmitIT: SubmitIT is the portal people use to submit talks to the conference. It looks just like any other part of the webpage, but is actually a separate Clojure webapp responsible for updating EMS with new content.
- Cake: This the administration portal for EMS, used by the program comitee for prioritizing and selecting talks for the conference.

There are other systems as well, but these are the ones essential for the webpage and program info, and the only ones we have elevated to the clouds by the power of Ansible.

All of these systems, of course, depend on each other and a host of other software. They also need to be configured correctly in different environments (test and production), and there of course need to be supporting tools like backup and system healthchecks in place.

A few years ago this was all mostly done manually. Now, by pressing a relatively low number of keys, it is possible to set it all up automatically.


## The Infrastructure

Lets have a quick overview of the infrastructure we are going to provision. The aim is to fully automate the process of going from nothing (well, there need to be a few empty servers) to all this configured correctly:

> TODO: diagram of how the major components interact on the different servers

The diagram shows the major components that need to be present and configured correctly. The systems all need a place to reside, their environment specific configuration must be present, and stuff like JDKs, mail servers, upstart scripts and databases must be installed. In addition, basic configuration like users, locale, hostnames, etc, must be set up. We want all of this as two separate environments, `test` and `production`, which should be identical except for some application properties and hostname and such.

> TODO: Simple diagram showing the servers

On top of this, we want to set up a third server for storing backups. This is also the server where we plan to setup other common components such as Jenkins, Nexus, etc.


## The Ansible setup

> TODO: Description of how we split the provisioning up into playbooks for each system, and one common playbook to rule them all. Describe how we separate roles for "systems" and "infrastructure", where we have (roughly) one playbook per system role, and that the infrascruture roles are dependencies of these.


## TODO: More stuff

> TODO: Sections it might make sense to have:

	- Dev environment: describe how we use Vagrant for dev environment
	- How to reuse roles. Ex: role for upstart script with parameters
	- Where to put the secrets
	- Using templates
	- Using variables
