# Nokre idear

### Mulig oppdeling:

- Lettvekts intro til Ansible; "ansible for programmers"
- Case study: Hvordan vi provisjonerte opp JavaZone-miljøene
- Erfaringer (les: rants) + tips

### Ansible for programmers

- Super easy!
- No installation on server!
    - krever ingen preprvisjoneringsinfrastruktur
    - Pull vs push
    - Puppet henter conf fra et eller annet sted, mens ansible pusher config til serveren.
- Naturlig rekkefølge på tasks gjør det enkelt å forstå hva som foregår.

### Erfaringer

- Lett å komme i gang med, men gjør litt vondt når en får en viss størrelse.
    - Mangler en god forklaring på hvordan en "vokser" med verktøyet.
    - Mange muligheter, ikke alltid enklelt å vite hva som er gode "patterns"
- Shitty dokumentasjon
    - Noen ganger tvetydig
    - Veldig mye, men ikke spesielt fokusert
- Missvisende API
    - `file` kopierer ikke filer, og aksepterer både "source" og "destination" parametere
- Vanskelig å lage gjenbrukbare moduler med gode interface
- Vanskelig å vite hvor variabler kommer fra. Spredd over alt.
    - ? Hva skjer hvis en definerer samme variabel flere steder? 
- Dårlige feilmeldinger
    - Lite tilbakemeldinger, vanskelig 
- Cowsay.  

### Tips

- Skrive dato for sist provisjonering til server
- Bruke `creates` og slikt for å bare kjøre `shell`-kall én gang
- `changed_when` kan hindre tasks som er "changed" hver eneste gang
