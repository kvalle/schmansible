# Plan your provisioning with Ansible

Noe om hvilke ting det kan være viktig å ha i bakhodet når en går i gang med Ansible-provisjonering.
Kanskje med javaBin-infrastrukturen som case?

Dette er den artikkelen som er minst sikker på, så foreslår at vi tar den sist.

- Presentere overblikk over serverne/appene
- Tanker rundt valget av hva en vil provisjonere
- Hvilke type rolle har de ulike "tingene" --> lagdelingen vår
- Kort gjennomgang av task vs rolle vs playbook
- Fokus på de funksjonelle tingene, og deres avhengigheter, og deretter roller for ting "nedover" i hierarkiet.
- Tenke på hvilke servere en har, i forbindelse med hosts-filer og playbooks for de ulike funksjonelle rollene
- Noe om å vurere å kutte eksplisitte avhengigheter
- Separasjon av variabler og logikk, og håndtering av miljøer
- Gjenbruk vs lage selv. Hvilke moduler gir det verdi å hente ned fra Ansible Galaxy?
- Avslutningsvis, oppsummering og noen tips

Using a layered architecture
====================

As your architecture gets larger having some sort of layering to separate concerns gets more and more attractive. We propose three layers:
- A *basic layer* containing those cross-cutting concerns everything else depends on. E.g. creating system users, installing ssh keys and setting up home folders.
- A *support layer* containing re-usable roles. If a role could be open-sourced and moved out of your project, it belongs here. E.g. roles for webserver proxies, databases, and artifact repositories.
- A *functional layer* provisioning those things that are specifically needed by _your_ own applications. In some cases, you might even want to deploy your applications in this layer.


Reuse?
======

Maybe.

In the Ansible ecosystem there is something called [Ansible Galaxy](https://galaxy.ansible.com), which is a central repository for third-party roles. Think [NPM](https://www.npmjs.com/) for those of you who are familiar with Node.js.

The great thing about Galaxy is that you don´t have to re-invent the wheel. The bad thing is that many technologies are exceptionally hard no abstract behind a role. Think about the DSLs of Nginx and Apache HTTPD. A third party role above Nginx would have to have the same power of expression as the Nginx DSL has.

> TODO: Illustrasjon og kanskje et kodeeksempel

This means that a generic third-party role would have to be ... well, extremely generic. And in most cases you don´t need generic. A _narrow_ role written solely for your use case would in most cases be enough. It would be focused on your use case, and you wouldn´t have to deal with all those parameters that you´re never going to use anyway. Remember that you also have to learn the technology abstracted by the role, and writing your own role is a great way to learn that technology from first-hand experience.
