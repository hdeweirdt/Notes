# Wat is het

## Building Blocks

### Entities
Entities zijn objecten binnen het domein met volgende eigenschappen
- Ze hebben een identiteit die hen definieert en hen onderscheidbaar maakt van andere objecten
- Die identiteit blijft dezelfde gedurende hun volledige levensduur
- Ze kunnen andere entities of value objects bevatten
- Ze zijn verantwoordelijk voor alle operaties die uitgevoerd worden op hun objectenx

Vanuit andere delen van het model wordt de de identiteit (identifier) van een enitity gebruikt om er naar te verwijzen.
Op basis van hoe ruim de context is waarbinnen een identifier uniek is worden er verschillende soorten gedefinieerd: 
- **externally unique identifier**: uniek zowel binnen als buiten het systeem ^externally-unique-id
- **globally unique identifier**: uniek binnen het syteem ^globally-unique-id
- **locally unique identifier**: uniek binnen de entititeit die deze entiteit bevat. ^locally-unique-id

Een entiteit bevat best enkel attributen en gedrag die essentieel zijn voor zijn bestaan of identiteit, anders is deze niet specifiek genoeg meer. 

Een entiteit is ook verantwoordelijk voor alle operaties op zowel zichzelf als de objecten die hij bezit. Het doel is om de entiteit altijd in een consistente staat te houden. Dit komt overeen met *encapsulatie* binnen OO.
	
### Value Objects
Value objects zijn objecten die voorkomen als attributen van entiteiten of andere value objects binnen het domein. Ze hebben volgende eigenschappen:
- Ze worden gedefinieerd volgens hun waarde, niet hun identiteit (want die hebben ze niet). Dit is het omgekeerde van [[#Entities]].
- Ze zijn immutable.
- Ze vormen een conceptueel geheel
- Ze kunnen verwijzingen naar entiteiten bevatten (niet entiteiten zelf)
- Ze definieren en dwingen specifieke constraints af.
- Ze kunnen een korte levensduur hebben

Een value object is niet zomaar een verzameling attributen, maar stelt een coherent geheel voor. Een persoon heeft bijvoorbeeld niet gewoon een verzameling attributen, maar een adres dat op zich uit meerdere attributen bestaat, en contactinformatie die op zich ook weer uit meerdere attributen bestaat.

### Aggregates
Aggregates zijn conceptuele opdelingen van de objecten binnen het model. Ze geven een eenheid van verandering aan: alle objecten binnen een aggregate worden tijdens aanpassingen als een geheel aanzien. Zo is men gegarandeerd dat objecten binnen een aggregate altijd voldoen aan de opgelegde business rules.

Een aggregate heeft volgende eigenschappen:
- Elke aggregate heeft een boundary en een root ^root-entity
	- De root is een enkele specifieke entiteit binnen de aggregate
	- De root is de enige entiteit waarnaar objecten buiten de aggregate kunnen verwijzen
	- De root heeft dus (op zijn minst) een [[#^globally-unique-id]]
	- Andere entiteiten binnen de aggregate hebben een [[#^locally-unique-id]]
	- De root mag referenties naar interne entiteiten doorsturen naar andere objecten, maar die referenties zijn *transient*: ze mogen niet bijgehouden worden
	- De root mag referenties naar value objectes doorgeven naar andere objecten
- Alle toegang tot objecten binnen de aggregate verlopen via de root
- Invarianten tussen delen van de aggregate moeten altijd gerespecteerd worden binnen elke transactie -> *strict consistency*
- Objecten binnen een aggregate mogen referenties bevatten naar andere aggregates

Vaak vallen de begrippen [[#^root-entity]] en [[#Aggregates]] samen en duiden ze eenzelfde object binnen het domein aan.

### Bounded Context
De bounded context is de grens waarbinnen de semantiek van de [[#Ubiquitous language]] consistent is. 
Dit impliceert dat wanneer men een andere betekenis aan een term binnen een bounded context geeft, men eigenlijk over een andere context aan het spreken is.

## Ubiquitous language
De verzameling termen, werkwoorden,... die door elke partij actief binnen een domein gebruikt worden. Op elk moment en op elke plaats moet dezelfde woordenschat gebruikt worden.
De ubiquitous language is strikt, exact en duidelijk.

De ubiquitous language is meer dan een woordenboek met daarin de zelfstandige naamwoorden uit het domein. Zie het meer als een reeks scenario's die voorstellen wat het domein moet doen. Een goede manier om dit te doen is met behulp van [[Domain Storytelling]].

Developers en domeinexperten moeten de neiging om de ubiquitous language volledig vast te leggen in documenten en deze te laten primeren over natuurlijke gespreksvoering. De hoogste kwaliteit wordt behaald door de collaboratieve feedback loop die onstaat tijdens zo'n gesprekken. 
De tekening en diagrammen die gebruikt worden om tot een domeinmodel te komen zijn **niet** het domeinmodel.


## Strategical Design
Strategisch design is het in grote lijnen opstellen van het domeinmodel. In deze stap worden [[#Bounded Context]]s en de [[#Ubiquitous language]] vastgelegd.

Het businessdomein/probleemdomein bestaat vanwege zijn omvang vaak uit meerdere *subdomains*. Zo'n subdomein is vaak een specifiek vak -of expertisegebied. Een van de subdomeinen is het belangrijkste voor de core business en wordt het **core domain** genoemd. De andere subdomeinen zijn van minder strategisch belang voor het bedrijf.
Er zijn twee soorten subdomeinen:
- **Supporting**: vereist custom development omdat er geen off-the-shelf oplossing bestaat. Zal echter niet dezelfde investering vereisen als het *core domain*
- **Generic**: is beschikbaar als off-the-shelf oplossing, of kan outsourced worden.

Het doel van onze ontwerpinspanningen is om voor elk subdomein een [[#Bounded Context]] te definieren. 

### Context Mapping
Wanneer een concept in verschillende [[#Bounded Context]]s voorkomt zullen deze vaak integreren met elkaar. Dit wordt Context Mapping genoemd. De lijn die getekend wordt tussen twee Bounded Contexts stelt de vertaling tussen de twee semantische invullingen van het concept voor. 

Er zijn verschillende soorten mappings:
- **Partnership**: wanneer 2 bounded contexts en hun teams een verzameling doelen delen en ze samen slagen of falen gaan ze een partnership aan. Dit vraagt een sterke commitment, en moet enkel aangehouden worden zolang beide partijen er effectief voordeel aan hebben. Zodra dit niet meer het geval is gaat men best over naar een ander soort mapping.
- **Shared kernel**: wanneer 2+ teams een klein maar veelvoorkomend model delen. Mogelijks is zelfs maar 1 team verantwoordelijk voor alle ontwikkelde artifacten.
- **Customer-Supplier**: de *supplier* zit upstream van de *customer* en bepaalt wat de customer zal krijgen en wanneer. De supplier zwaait dus de plak. Deze moet natuurlijk wel nog blijven voldoen aan de noden van de customer, anders ondermijnt hij zijn eigen nut.
- **Conformist**: variant op customer-supplier waar de supplier weinig tot geen reden heeft om met de noden van hun downstream rekening te houden. Die heeft bvb de resources niet om het model naar zijn eigen noden te zetten en moet het model van de supplier rechtstreeks gebruiken.
- **Anticorruption layer**: de downstream bounded context installeert een *translation layer* die beide modellen duidelijk van elkaar scheidt. Deze mapping wordt best zo vaak als mogelijk toegepast om te zorgen dat men binnen een context altijd met modellen kan werken die specifiek zijn voor de noden binnen die context.
- **Open Host Service**: definieert een protocol of interface die toegang biedt aan de bounded context als een verzameling services. Deze services zijn voldoende gedocumenteerd en eenvoudig te gebruiken. Dit maakt hem eenvoudiger te gebruiken dan de *conformist* mapping.
- **Published Language**: een gedocumenteerde en gestructureerde taal die toelaat om gebruik en vertaling van bounded contexts te vereenvoudigen. Cfr OpenAPI spec opstellen met DTO's ipv uw eigen domeinobjecten direct publiceren.
- **Separate Ways**: als er geen meerwaarde is aan integreren met andere bounded contexts


## Tactical Design
Bij tactical design worden de details van het domeinmodel ingevuld.

## Problem Space - Solution Space
De *problem space* is waar men op hoog niveau strategische analyses doet en de constraints van het project vastlegt.
In de *solution space* implementeer je dat deel van je domein wat in de de problem space als het *core domain* werd aangeduid.

# Bronnen
- Secure by Design, Dan Bergh Johnsson
- Domain-Driven Design Distilled -Vaughn Vernon