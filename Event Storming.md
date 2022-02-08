# Wat is het
Een ontwerptechniek die toelaat om op een snelle en ongestructureerde manier een businessdomein uit te diepen. De focus ligt op het businessproces en de events die daarin voorkomen, *niet OO-klassen of databases*. 

Developers en domeinexperten werken samen om tot een compleet begrip van het probleemdomein te komen. 

Event Storming kan voor zowel big-picture als design-level modelleren gebruikt worden.

# Aanpak
Doen >> lezen
Als men verder in het verloop merkt nog iets te missen kan men altijd even terugspringen en toevoegen. Het moet een organisch proces blijven.
Hieronder dus in een semi-vrijblijvende volgorde.

## Domeinevents met oranje
- als een werkwoord in voltooid verleden tijd *(bvb ProductCreated)*
- In chronologische volgorde van links naar rechts
	- Events die parallel gebeuren onder elkaar

## Processen als gevolg van een domeinevent in purper
- Pijltje van domeinevent naar proces. 
- Te fine-grained processen (niet in [[DDD#^core-domain]]) niet verder uitwerken

## Commands die een domeinevent of process starten in lichtblauw
- Links van het domeinevent dat het veroorzaakt (kunnen er meerdere zijn)

## Entity/Aggregate die command zal uitvoeren en event produceert in geel
- Tussen command en domeinevent in plaatsen

## Teken lijnen en pijlen om flow aan te duiden
- Bounded contexts in volle lijnen
- Subdomains in stippellijnen

## Views in het groen

## Uit te klaren onduidelijkheden met paars of rood (Op elk moment)
Beetje tekst toevoegen met uitleg wat een probleem is en waarom. Probeer grotere problemen niet direct uit te klaren, want dat houdt het proces tegen en waarschijnlijk zijn er teveel mensen om tot een duidelijk antwoord te komen.
