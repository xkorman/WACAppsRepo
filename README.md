# App repozitár pre cvičenia WAC 2022

Applikácie v tomto repozitári sa automaticky nasadia do AKS klastra a mali by byť prístupné na adrese:
https://wac-2023.germanywestcentral.cloudapp.azure.com/ui/

- Každý študent by mal mat RW práva na tento repozitár.

- Aplikácia by sa mala skladať z dvoch častí: Webcomponent a Webapi. Davajte ich do oddelenych podadresarov.

- Nedávajte sem manifesty na "infraštruktúru", napr. `mongodb`, `ufe-controller`,... Tie sú nasadené centrálne.

- Uistite sa, že mená komponentov sa nezhodujú so žiadnym, ktoré sú už nasadené. **Keďže sa všetky nasadzujú do toho istého namespace, musia mať iné mená.**

- Každý commit by mal byť schválený "maintainerom", čiže jedným z cvičiacich.

> **Nemeňte adresáre, ktoré nie sú vaše a ich obsah !!!**

## Pridanie aplikácie 

1. Ak nemáte, vyžiadajte si prístup na `Apps-repo` u cvičiaceho.

2. Vytvorte adresár v root priestore s názvom vašej aplikácie. Štruktúra podadresárov v ňom nie je predpísaná, odporúča sa mať podadresár na webkomponent a druhý na webapi.

3. V hlavnom adresári vašej aplikácie musí byť súbor kustomization.yaml referencujúci vaše komponenty.

4. Do súboru `\clusters\prod\kustomization.yaml` pridajte odkaz na adresár s vašou aplikáciou.

5. Spravte komit.

   Po komite Flux automaticky nasadí komponenty do AKS. Overte to po pár minútach na stránke aplikácie, i.e. https://wac-2023.germanywestcentral.cloudapp.azure.com/ui/. 

### Aktualizácia verzie docker obrazu

Ako nakonfigurovať automatickú aktualizáciu verziu docker obrazu v klastri sme si ukázali v cvičeniach ku predmetu WAC v kapitole [Kontinuálne nasadenie pomocou nástroja Flux](http://wac-fiit.milung.eu/v2/01.Web-Components/dojo/008-flux). Vytvorili sme 3 Flux-ové komponenty a upravili deployment (kustomization) pre daný pod. Analogicky by sme to mohli nakonfigurovať aj pre produkčný klaster, ale nebudeme to robiť. Dôvodov je viac: 

- Bolo by to časovo náročné. Administrátor klustra by musel nasadiť 3 flux komponenty pre každý webkomponent. 
- V našom nastavení projektu (CI build) nemáme dostatočne zaručenú kvalitu. Nemáme testy v builde ani manuálny krok schválenia kvality komponentu pred "pushom" do docker repozitára.

Preto budeme zmenu verzie docker obrazu robiť **manuálne**. Po vyrobení verzie docker obrazu, ktora obsahuje novú funkcionalitu a je dostatočne pretestovaná na lokálnom klastri, zmente verziu v súbore `deployment.yaml` svojho komponentu v spoločnom repozitári `Apps repo` na gitlabe.

Po komite aktualizovaného súboru, sa Flux postará o jeho nasadenie do klastra.Overte to po pár minútach na stránke aplikácie https://wac-2023.germanywestcentral.cloudapp.azure.com/ui/. 
