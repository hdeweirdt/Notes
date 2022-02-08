# Wat is het

Modelleringstechniek die toelaat snel de verschillende actoren en interacties binnen een domein te leren kennen.
Grafisch weergeven van die elementen aan de hand van symbolen verbonden met genummerde pijlen.

## Voorbeeld
![[Domain Stories - Direct order placement.jpg]]
bron: [[https://miro.com/app/board/uXjVOT6VK2w=/?invite_link_id=430380222432]]

## Relatie met [[Event Storming]]
- Het systeem op zich blijft een black box, in tegenstelling tot [[Event Storming]] waarbij wel dieper wordt gekeken.
- Event Storming is iets complexer, heeft meer materiaal nodig
- Voor documentatie is Domain Storytelling iets beter

### Wanneer Domain Storytelling ipv [[Event Storming]]
- Groot aantal actoren
- Nadruk ligt op het uitzoeken hoe de actoren communiceren en samenwerke
- Voor "to-be" processen lijkt de zin-per-zin aanpak eenvoudiger te werken
- Als documentatie het doel is
- Als de bedrijfscultuur meer fan is van de gestructureerdere aanpak van Domain Storytelling, tegenover de brainstormaanpak bij [[Event Storming]]

## Gebruik binnen [[DDD]]
Verschillende van de termen die in het diagram voorkomen zullen rechtstreeks overeenkomen met de [[DDD#Ubiquitous language]] of [[DDD#Ubiquitous language]].

De work objects zullen waarschijnlijk de [[DDD#Entities]] of [[DDD#Aggregates]] worden. De activities worden [[DDD#Operations]] op die Entities.

### Heuristieken voor [[DDD#Strategical Design]]
- activities die vanuit het standpunt van een actor samenhoren uit Domain Storytelling vormen [[DDD#Bounded Context]]s
- work objects die voorkomen in verschillende activiteiten (== meerdere keren zichtbaar zijn op het diagram) komen waarschijnlijk terug in meerdere [[DDD#Bounded Context]]s

## Bronnen
https://domainstorytelling.org/
https://www.youtube.com/watch?v=MPQfb7fsw3I
https://kalele.io/why-eventstorming-practitioners-should-try-domain-storytelling/