# Opdracht selectie collectie

## Doel: selectie kunnen maken en beargumenteren.

## Inleiding.

We gebruiken een voorbeeld van een verwerking zoals dit in de praktijk gebruikt zou kunnen worden:

Gegeven een bedrijf dat mutaties in klantgegevens verwerkt. Deze verwerking gebeurt in 2 stappen:
Overdag worden call center medewerkers gebeld door klanten die een mutatie willen doorgeven. Per mutatie wordt allereerst in een aparte datastructuur voor de mutaties (de mutatie ADT) gekeken of er al een mutatie voor deze klant is. Zo ja, dan wordt die mutatie aangepast en weer opgeslagen in de mutatie ADT. Zo nee, dan wordt er een mutatie toegevoegd aan de mutatie ADT. Hiertoe moeten eerst de klantgegevens uit een centrale database opgevraagd worden, waarna ze pas toegevoegd kunnen worden aan de mutatie ADT.
Aan het eind van de werkdag worden alle mutaties uit de mutatie ADT weggeschreven naar een bestand.

's Nachts wordt dit bestand met mutaties verwerkt in een batchverwerking. In één keer wordt het bestand ingelezen en in een aparte datastructuur gezet (de nacht ADT). Deze nacht ADT datastructuur wordt eenmaal doorlopen om accounting en logging mogelijk te maken, en de klantgegevens in de centrale database te updaten.

Alle klantgegevens en mutaties gebruiken een klantnummer, waarop gezocht wordt. Gemiddeld zijn er 45.000 telefoontjes per dag, die leiden tot 40.000 mutaties van unieke klanten (dus er zijn 5.000 klanten die meermaals per dag bellen met een mutatie). Beide datastructuren samen leveren een zo efficiënt mogelijke verwerking op.

Gegeven is onderstaande tabel met verschillende datastructuren en orde van de operaties op de structuur. n is het aantal elementen in de structuur.

| operatie	| datastructuur1 (DS1)	| datastructuur2 (DS2)	| datastructuur3  (DS3)|
|:----------|:-----------------:|:-----------------:|:--------------:| 
| isEmpty()	| O(1)	| O(1)	 | O(1) |
| clear()	| O(1)	| O(1)	| O(1) |
| insert(Object)	| O(1)	| O(1)	| O(1) |
| update(Object)	| O(log(n))	| O(n)	| O(log(n)) |
| Object search(long klantnr) |	O(n)	| O(log(n)) |	O(n2) |
| delete(Object) |	O(n)	| O(n2)	| O(1) |
| Object firstObject()	| O(n)	| O(1)	| O(n) |
| Object lastObject()	| O(n)	| O(1)	| O(n) |
| previous()	| O(n)	| O(n)	| O(1) |
| next()	| O(n)	| O(n)	| O(1) | 



## Opdracht 1.1 (voor een 2)

#### Vraag a. Welke datastructuur wordt overdag gebruikt voor de mutatie ADT? 
Er zijn 3 datastructuren:
- Mutatiebestand
- Klantenbestand
- AccountingLoggingBestand

Overdag zijn de meest gebruikte functies de update en de insert van een mutatie en het lezen van klantgegevens.

Gemiddeld gezien zijn er 45000 handelingen, 5000 update mutaties en 40000 nieuwe mutaties.

Ik ga ervan uit dat altijd eerst gecontrolleerd wordt of de klant reeds aanwezig is in het mutatiebestand.
Indien dit het geval is wordt er een mutatieopdracht gestart anders een invoeropdracht.

Belangrijk is dat het zoeken in het mutatiebestand snel gaat. Hierdoor valt DS3 af als mutatiebestand omdat het O(n2) heeft voor zoeken.

Voor het invoeren en updaten is het belangrijk dat dit ook zo efficient mogelijk gaat. 
DS1 heeft O(1) voor invoeren en O(log(n)) voor updaten is is daardoor net iets geschikter dan DS2 met respectievelijk O(1) en O(n)
Vanwege de initiele zoek op klant heeft DS2 toch de voorkeur
**Antwoord: Mutatie ADT DS2**

#### Vraag b. Welke datastructuur wordt 's nachts gebruikt voor de nacht ADT? 
In de nacht ADT wordt het bestand eenmalig gelezen, belangrijk is dus dat dit efficient gebeurd.
DS3 voert dit elke leesactiviteit uit in O(1).

**Antwoord: Nacht ADT DS3**
#### Vraag c. Hoe lang duurt de verwerking overdag (in ms)? 
Ga ervan uit, dat een O(1) actie in een datastructuur 1 ms (milli seconde) duurt. En het opzoeken of updaten in de centrale database 10 ms. Het wegschrijven en lezen van het bestand,  de accounting en logging wordt buiten beschouwing gelaten.

Ik ga ervan uit dat de mutatie ADT DS2 is zoals hierboven aangegeven.
De verwerking duurt dan:

| Symbool | Betekenis | Voor x elementen |
|:---:|:----|:-----|
| U | Aantal updates |  5000 |
| N | Nieuwe mutaties | 40000 |
| Z | Klanten zoeken  O(log(U+N) per actie | N * log(N) = (U+N) log(U+N ) |
| I | Invoeren O(1) + O(10) per actie | N * 11 |
| A | Aanpassen / Muteren O(n) per actie | U*U) = U^2 |

| Formules |
|-------|
| DS2(T) = Z + (N * I) + (U*A) |
| DS2(T) = (45000 * log(45000) + (40000*11) + (5000*5000)  |
| DS2(T) = 210000 + 440000 + 25000000 |
| DS2(T) = 25650 seconden

#### Vraag d. Hoe lang duurt de verwerking 's nachts (in ms)? 

Voor de nacht ADT wordt DS3 gebruikt. Hierbij zijn er totaal 40000 mutaties (aangenomen wordt dat het laatste mutatie record de overige vervangt)
Voor het te lezen bestand wordt DS2 gebruikt.

| Symbool | Betekenis | Voor x elementen |
|:---:|:----|:-----|
| L | Aantal leesacties DS2 O(n) |  40000 |
| S | Aantal schrijfacties DS3 O(1) | 40000 |

| Formules |
|-------|
| DS2(T) = L * O(L) = L * L = 160000000 | 
| DS3(T) = S * O(1) = S = 40000 |
| Totaal lezen + schrijven = 160000 seconden  |

## Opdracht 1.2 (voor een 3)
Idealiter zouden alle operaties orde 1 zijn, maar helaas kan dat in de praktijk niet, we zullen dus een keus moeten maken.
Kies uit het JAVA collection framework een datastructuur uit, die het best voor de mutatie ADT gebruikt kan worden. Licht je antwoord toe. Geef ook de verwerking in msec, uitgaande van bovenstaande uitgangspunten bij vraag c en d.

Een mogelijke oplossing is de keuze van een HashMap. Een HashMap heeft afhankelijk van de keuze van de capaciteit een BigO van O(1)
De kans op een collision is n/capaciteit. Hoe groter we de capaciteit kiezen hoe kleiner de kans op collisions. 

Als we uitgaan van een Hashmap met O(1) voor Insert, Zoeken en Update krijgen we de volgende resutaten:

De verwerking duurt dan:

| Symbool | Betekenis | Voor x elementen |
|:---:|:----|:-----|
| U | Aantal updates |  5000 |
| N | Nieuwe mutaties | 40000 |
| Z | Klanten zoeken  O(1) per actie | N  |
| I | Invoeren O(1) + O(10) per actie | N * 11 |
| A | Aanpassen / Muteren O(1) per actie | U |

| Formules |
|-------|
| DS2(T) = Z + (N * I) + (U*A) |
| DS2(T) = (45000) + (40000*11) + (5000)  |
| DS2(T) = 45000 + 440000 + 5000 |
| DS2(T) = 490 seconden

## Opdracht 1.3 (voor een 4)
Verandert je keuze voor de mutatie ADT, als je ervan uitgaat, dat toegang tot deze datastructuur gelijktijdig kan plaatsvinden (immers, helpdeskmedewerkers kunnen gelijktijdig wijzigingen aanbrengen). Licht je antwoord toe.

Mijn keuze zou veranderen om gelijktijdige toegang te kunnen garanderen. Een mogelijk oplossing zou de ConcurrentHashmap kunnen zijn die ook in de meeste gevallen BigO O(1) is
## Opdracht 1.4 (voor een 5)
Kies uit het JAVA collection framework een datastructuur uit, die het best voor de nacht ADT gebruikt kan worden. Licht je antwoord toe. Geef ook de verwerking in msec, uitgaande van bovenstaande uitgangspunten bij vraag c en d.

Voor de nacht ADT zit het grootste probleem (zie opgave 1.1) is het lezen van het mutatiebestand. Dit is door de keuze van HashMAp of ConcurrentHashMap al sterk verbetert. 
Met gebruikmaking van de Hashmap wijzigt de tijd van de operaties als volgt


| Symbool | Betekenis | Voor x elementen |
|:---:|:----|:-----|
| L | Aantal leesacties DS2 O(1) |  40000 |
| S | Aantal schrijfacties DS3 O(1) | 40000 |

| Formules |
|-------|
| DS2(T) = O(L) = L * L = 40000 | 
| DS3(T) = S * O(1) = S = 40000 |
| Totaal lezen + schrijven = 80 seconden  |

## Opdracht 1.5 (voor een 6)
Verandert je keuze voor de nacht ADT, als je ervan uitgaat, dat het inlezen van de data uit de file door een aantal threads uitgevoerd wordt, die ieder steeds een record uit de file lezen en deze verwerken. Licht je antwoord  toe.

Als we gebruik maken van een (onbeperkt) aantal threads kan (theoretisch) O(1) worden gehaald. Bij gebruik van 'n' threads
Dit geeft de volgende resultaten

| Symbool | Betekenis | Voor x elementen |
|:---:|:----|:-----|
| U | Aantal updates |  5000 |
| N | Nieuwe mutaties | 40000 |
| Z | Klanten zoeken  O(1) per actie | N  |
| I | Invoeren O(1) + O(1) per actie | N * 2  |
| A | Aanpassen / Muteren O(1) per actie | U |

| Formules |
|-------|
| DS2(T) = Z + (N * I) + (U*A) |
| DS2(T) = (45000) + (2*40000) + (5000)  |
| DS2(T) = 45000 + 80000 + 5000 |
| DS2(T) = 170 seconden

## Opdracht 1.6 (voor een 8)
Maak een eigen implementatie van een datastructuur voor de mutatie ADT, die beter (lees: sneller) is dan een standaard collectie uit het JCF. Hiervoor zul je ofwel via compositie een eigen datastructuur moeten maken, ofwel gebruik maken van de abstracte collecties uit het JCF. Implementeer deze in JAVA. Licht je keuze toe.

Het is erg moeilijk om een betere structuur te vinden voor de mutatie ADT dan de Hashmap of de ConcurrentHashmap die hierboven is aangegeven.
Wat wel mogelijk is is een efficientere manier van het verwerken van de data. Een mogelijk hulpmiddel hierbij is de Reactive Java modules in de <code>java.util.concurrent.Flow</code> bibliotheek.
Dit package kent een <code>Publisher</code>, <code>Producer</code>, <code>Subscriber</code>

Een <code>Publisher</code> kan een bericht versturen en een <code>Subscriber</code> kan aangeven dat hij een bericht wil ontvangen. Tot slot kan een <code>Producer</code> zowel een bericht ontvangen en versturen.
Een mogelijke implementatie is dan zoals hieronder aangegeven. 
 
![Integration scenario](./img/flow.jpg)

In dit voorbeeld wordt een mutatie ingevoerd in de <code>MutationPublisher</code>. Het resultaatbericht wordt uitgestuurd naar de <code>UpdateProducer</code>. De <code>UpdateProducer</code> raadpleegt eventueel de <code>SearchProducer</code> om specifieke klantgegevens op te vragen als deze nog niet beschikbaar zijn. De <code>SearchProducer</code> stuurt twee verschillende berichten uit:
- een <code>DurationMessage</code> die aangeeft dat het een 'x' aantal ms kost voor deze operatie.
- een <code>CustomerDetailMessage</code> die additionele informatie van de klant geeft

De <code>UpdateProducer</code> heeft nu alle informatie en stuurt meerdere berichten uit:
- een <code>Durationbericht</code> uit met het aantal ms dat de operatie kost.
- een <code>UpdateLogMessage</code> met informatie over de transactie voor logging
- een <code>UpdateAuditMessage</code> met informatie voor auditing en controle.

De <code>DurationSubscriber</code>, <code>AuditingSubscriber</code> en de <code>LoggingSubscriber</code> geven bij de <code>UpdateProducer</code> aan dat ze de specifieke berichten willen ontvangen.

De oplossing is schaalbaar, eventueel kan er per klant/product een producer/subsrciber worden gekozen waardoor de oplossing afhankelijk is van het aantal resources.
 
## Opdracht 1.7 (voor een 10)
Idem, maar dan voor de nacht ADT. Licht je keuze toe.

