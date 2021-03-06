---
layout: post
title:  "Einnahmen - Ausgaben vorarlberger Gemeinden 2011"
date:   2013-12-02 20:30
categories: projects
tags: osm open-data
published: true
excerpt_short: Visualisierung der der Einnahmen und Ausgaben der vorarlberger Gemeinden
author: Christoph Klocker
blog_image: einnahmen-ausgaben.jpg
---

Vor 3 Wochen habe ich mir mal die veröffentlichten Open Government Daten von Vorarlberg angeschaut und dachte mir es
wäre interessant gewisse Daten auf einer Karte zu veröffentlichen. Ich wusste vorerst gar nicht welche Daten ich
da verwenden möchte und habe mich mal umgeschaut. Leider musste ich feststellen, dass es gar nicht so einfach ist, einfach zu
verwendende Daten zu finden.

Leider sind die Daten oft "schlecht" aufbereitet, bzw sind diese zum Teil gar nicht aufbereitet (Excel Sheets mit mehreren Tabellen)
oder werden über ein Uralt-System (Cognos) zur Verfügung gestellt, d.h. es ist nicht einfach die Daten, ohne größeren Aufwand,
automatisiert weiter zu verarbeiten.

Hier fehlt es wohl noch an einer Open Government Strategie um die Daten in eine Entwicklerfreundliche Struktur zu bringen.
Andere Bundesländer wie [Wien](https://open.wien.at/site/open-data/) und [Linz](http://data.linz.gv.at/) machen es vor. Hoffen wir es ändert sich bald sodass engagierte Entwickler
auf diesen Daten innovative Applikationen bauen können.

Nichtsdestotrotz habe ich Schlussendlich einen Datensatz gefunden, den ich auf eine Open Street Map Karte projezieren konnte, und
zwar die Einnahmen und Ausgaben der Vorarlberger Gemeinden. Auch hier zu bemängeln, der Datensatz enthält nur die Daten für
2011, keine historischen und auch keine für 2012. Der Datensatz für die Gemeindegrenzen war hervorragend und wurde zwecks Performance
 vereinfacht.

Auf der Karte sind alle Gemeinden die negativ budgetieren rot eingefärbt (mit Abstufungen, je nach Grad des Defizites), bzw
 grün bei einem Budgetüberschuss.

Hier der Link zum Projekt:

<a href="http://open-data-vorarlberg.herokuapp.com/einnahmen-ausgaben" target="_blank">Einnahmen - Ausgaben vorarlberger Gemeinden</a>



Technologien für dieses Projekt: Open Street Map (Mapbox), Leaflet.js.