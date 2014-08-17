# Nokre idear


Ting vi må teste:
    - hva skjer hvis vi provisjonerer fra to maskiner samtidig?



### Mulig oppdeling:

- Lettvekts intro til Ansible; "ansible for programmers"
- Case study: Hvordan vi provisjonerte opp JavaZone-miljøene
- Erfaringer (les: rants) + tips

### Ansible for programmers

- Super easy!
- Hensikt: Kunne reprodusere oppsett
    + Mange servere?
    + Ville kunne sette opp server på nytt raskt hvis noe skulle skje
- Infrastruktur som kode
- Eksplisitt hvordan servere er satt opp
    + Enelt for nye utviklere å sette seg inn i hvordan miljøer henger sammen
- No installation on server!
    - krever ingen preprvisjoneringsinfrastruktur
    - Pull vs push
    - Puppet henter conf fra et eller annet sted, mens ansible pusher config til serveren.
- Med enkelte hederlige(?) unntak, god mapping mellom kommandolinja og ansible.
- Naturlig rekkefølge på tasks gjør det enkelt å forstå hva som foregår.
- Mini "case" som viser én enkelt yaml som provisjonerer stuff
    + Så enkelt kan det gjøres!


### Hvordan vi provisjonerte JZ

- Infrastrukturarkitekturoversikt
    + Hvilke servere, apper, databaser, etc, skulle vi provisjonere
- Hvordan har vi gått frem?
    + Hvilke script har vi?
    + Hvordan har vi delt opp i roles?
    + Strukturering av filer
    + Templates
- Hvorfor har vi delt opp i 4 script
- Vise en provisionering live? (Video?)

### Erfaringer

#### Rants

- Lett å komme i gang med, men gjør litt vondt når en får en viss størrelse.
    - Mangler en god forklaring på hvordan en "vokser" med verktøyet.
    - Mange muligheter, ikke alltid enklelt å vite hva som er gode "patterns"
- Shitty dokumentasjon
    - Noen ganger tvetydig
    - Veldig mye, men ikke spesielt fokusert
- Misvisende API
    - `file` kopierer ikke filer, og aksepterer både "source" og "destination" parametere
- Vanskelig å lage gjenbrukbare moduler med gode interface
- Vanskelig å vite hvor variabler kommer fra. Spredd over alt.
    - ? Hva skjer hvis en definerer samme variabel flere steder? 
    - Hvilken rekkefølge resolves variabler i? Default-mappe, host_vars, argument i meta, etc?
- Dårlige feilmeldinger
    - Lite tilbakemeldinger, vanskelig å vite hva som foregår
    - Glemt å oppgi sudo-passord? Ansible henger.
    - Kjipt å debugge når noe ikke funker.
- Cowsay.
- Struktur:
    - Hvordan bør ting henge sammen? Referere roller i `meta` eller i playbook?

#### Tips

- Start med vagrant fra dag 1, det er piss å provisjonere mot en faktisk server.
- Skrive dato for sist provisjonering til server
- Bruke `creates` og slikt for å bare kjøre `shell`-kall én gang
- `changed_when` kan hindre tasks som er "changed" hver eneste gang
- Lag et script, så slipper du å glemme `--ask-sudo-pass` hver gang
- Nøkler
    - Authorized_keys for å kopiere ut nøkler
    - Mellomserverkommunikasjon:
        + Hvis nøkkel ikke finnes, opprett nøkkel (eller gjør dette ved hjelp av `user`-modulen)
        + Hvis nøkkel opprettet, ssh-copy-id til andre servere

- Hacks
    - Sette passord ved vagrant up

### Annet vi kan prate om (hvis vi får sett på det)

- [Tower](http://www.ansible.com/tower)
