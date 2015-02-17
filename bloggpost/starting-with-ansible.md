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
