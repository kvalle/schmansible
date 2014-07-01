# JavaZone

### Lyntale: *Ansible: Provisjonering for programmerere*

- Summary: 
    
    > Er du lei av å google de samme kommandoene hver gang du skal sette opp et miljø? Lei av at TEST ikke er likt PROD? Mangler du god dokumentasjon av miljøene dine? Provisjonering lar deg automatisere en reproduserbar konfigurasjon, og det trenger ikke være vanskelig. Vi viser deg hvordan du kommer veldig enkelt i gang med provisjonering ved hjelp av verktøyet Ansible.

- Full abstract:
    
    > Vi skal se på hvordan du kan skrive infrastrukturen din som kode, ved hjelp av Ansible. Ansible følger en litt annerledes filosofi enn mer konvensjonelle provisjoneringsrammeverk som f.eks. Puppet. Det krever minimalt med boilerplate, og omtrent ingen eksisterende infrastruktur. Vi skal se på noen helt konkrete eksempler på hva man kan bruke det til, og vise et minimalt eksempel samt teknikker for å håndtere gjenbruk når en større infrastruktur skal provisjoneres.

- Outline:

    - Hvorfor provisjonere?
    - Hvorfor Ansible?
    - Minste mulige fungerende eksempel
    - Strukturering når vi kommer opp i litt størrelse (tasks, files, templates, variables)
    - Topp tre(ish) problemer med Ansible

### Foredrag: *Provisjonering av JavaZone*

- Summary: 
    
    > Serverne som kjører JavaZone sine systemer er provisjonert med Ansible. I denne erfaringsrapporten forteller vi hvordan vi har automatisert konfigureringen av miljøene, hva dette har gitt oss, og hvilke problemer vi har støtt på.

- Full abstract: 
    
    > Det siste året har JavaZone sine systemer blitt flyttet opp i "ze cloud". Vi kan nå, med få tastetrykk, sette opp en brand spanking new server klar til å hoste systemene JavaZone avhenger av. Tiden da vi måtte holde orden på ssh-nøkler og konfigurasjonsfiler og "apt-get" ting for hånd er forbi. Ingen flere manuelle steg! 
    > 
    > Vi vil presentere hvordan Ansible har hjulpet oss, og si noe om hvilke problemer vi har møtt på veien. Hvilken verdi gir det å provisjonere? Hvordan er Ansible sammenlignet med andre provisjoneringsverktøy? Hvilke ting kan det være greit å være klar over når en tar i bruk Ansible?
    > 
    > Vi sitter ikke med alle svarene. Vi er godt fornøyd med hva vi har fått til så langt, men har fortsatt mye vi ønsker å forbedre. Vi ranter, ehm, deler våre erfaringer, og kommer med noen tips som har hjulpet oss på veien.

- Outline
    
    - Intro: Hvem er vi, og hva er Ansible
    - Oversikt over infrastrukturen til JZ; Hva har vi provisjonert?
    - Presentasjon av provisjoneringsløsningen
    - Live-provisjonere opp en server?
    - Erfaringer med Ansible + tips og triks
