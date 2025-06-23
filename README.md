# TravelTide Rewards Analyse

## Inhaltsverzeichnis

* Projektübersicht
* Technologiestack
* Projektstruktur
* Analyseziel
* Herangehensweise
* Datenquellen
* Personalisierungslogik
* E-Mail-Kampagne
* Bedeutung für das Unternehmen
* Projektzeitplan (4-Wochen-Plan)
* Weiterentwicklung

---

## Projektübersicht

"TravelTide Rewards Analyse" ist ein datenanalytisches Projekt zur Verbesserung der Kundenbindung auf der Reiseplattform TravelTide. Ziel ist es, ein personalisiertes Bonusprogramm zu entwickeln, bei dem jeder Kunde gezielt mit dem Vorteil angesprochen wird, der für ihn am relevantesten ist.

Die Erkenntnisse dieser Analyse fließen direkt in eine geplante E-Mail-Kampagne ein, die personalisierte Belohnungen kommuniziert. 
Grundlage sind echte Kundendaten und Verhaltenserkenntnisse.
Grundlage ist eine relationale PostgreSQL-Datenbank mit:

* 4 Tabellen
* 44 Spalten
* insgesamt 10.248.644 Datensätzen

---

## Technologiestack

* Programmiersprache: -
* Datenanalyse: SQL
* Visualisierung: -
* Clustering / Segmentierung: -
* Umgebung: DBeaver, Google Colab, VS Code

---

## Projektstruktur

```
traveltide-rewards-analyse/
│
├── README.md                 ← Projektbeschreibung & Zielsetzung
├── requirements.txt          ← Abhängigkeiten
│
├── data/
│   ├── raw/                  ← Rohdaten (nicht veröffentlicht)
│   └── processed/            ← Vorbereitete Datensätze für Analysen (nicht veröffentlicht)
│
├── notebooks/
│   ├── 01_exploration.ipynb         ← Erste Datenübersicht
│   ├── 02_segmentation.ipynb        ← Segmentierung nach Interessen
│   └── 03_perk_assignment.ipynb     ← Perk-Zuordnung pro Kunde
│
├── src/
│   ├── data_preparation.py          ← Bereinigung & Transformation
│   ├── modeling.py                  ← Segmentierungslogik & Scoring
│   └── utils.py                     ← Helferfunktionen
│
└── outputs/
    ├── visuals/                     ← Plots & Visualisierungen
    └── summary.md                  ← Ergebniszusammenfassung
```

---

## Analyseziel

Das Projekt liefert die Grundlage für ein personalisiertes Reward-Programm.
Zentrale Fragestellung:

> Welche Art von Belohnung spricht welche Kunden besonders an?

Das Ziel ist, für jeden Kunden einen wahrscheinlich bevorzugten Vorteil (Perk) zu identifizieren und damit die Conversion der Belohnungseinladung zu erhöhen.

---

## Herangehensweise

**1. Datenvalidierung der Hypothese**
Segmentierung der Kundenbasis nach Interessen- oder Nutzungsverhalten. Gibt es Cluster mit klaren Präferenzen für z. B. "Kostenlose Stornierung", "Zusatzmeilen" oder "VIP-Zugang"?

**2. Perk-Zuordnung pro Kunde**
Auf Basis der Segmentzugehörigkeit und Nutzungsdaten wird jedem Kunden ein favorisierter Vorteil zugewiesen.

**3. Ergebnisbereitstellung**
Erstellung eines „Scoring Sheets“ zur Weiterverarbeitung im Marketing.

---

## Datenquellen

* sessions - Daten zu Nutzer-Sessions, Rabatten und Buchungsverhalten
* user - Demografische Nutzerinformationen wie Alter, Geschlecht und Wohnort
* hotels - Angaben zu Hotelbuchungen, Aufenthaltsdauer und Kosten
* flights - Informationen zu gebuchten Flügen und Reisedetails

---

## Personalisierungslogik - !!! Nachbearbeiten !!! 

Die Zuordnung erfolgt modellbasiert (Clustering, z. B. K-Means) oder regelbasiert, je nach Datenlage. 
Bei geringer Datenverfügbarkeit werden heuristische Regeln angewendet (z. B. Vielbucher = Zusatzmeilen).

---

## E-Mail-Kampagne - !!! Nachbearbeiten !!! 

Mock-ups zeigen den Unterschied zwischen generischen und personalisierten Mails. 
Die Analyse entscheidet mit, welche Botschaft bei welchem Kunden landet – z. B.:

* "Exklusive Vorteile für Vielbucher: Extra-Meilen sichern"
* "Sorgenfrei reisen: Ihre kostenlose Stornierung wartet"

---

## Bedeutung für das Unternehmen

TravelTide hat sich technologisch stark auf Such-Performance fokussiert. 
Dieses Projekt bringt erstmals datengetriebene **Kundenbindung** in den Mittelpunkt und liefert eine direkte Verbindung zwischen Analyseergebnissen und operativer Umsetzung.

---

## Projektzeitplan (4-Wochen-Plan)

### Woche 1 – EDA: Datenexploration

* Verstehen des Geschäftsproblems und der verfügbaren Tabellen
* Erste Analysen mit SQL
* Ausschluss irrelevanter Kunden 
* Datenbereinigung und Zusammenführung auf Session-Ebene
* Output: bereinigte Session-Tabelle, erste Aggregationen, Verständnis aller Spalten

### Woche 2 – Feature Engineering

* Entwicklung neuer Metriken zur Kundentypisierung
* Kombination und Aggregation bestehender Daten zur Kundenebene
* Output: Attribute erzeugen, die für die Segmentierung relevant sind

### Woche 3 – Kunden-Segmentierung

* Clustering oder Regelbasierte Segmentierung
* Kunden werden Gruppen zugewiesen, jede Gruppe erhält passenden Vorteil (Perk)
* Verständnis und Beschreibung der Kundengruppen

### Woche 4 – Ergebnispräsentation

* Erstellung eines Executive Summary mit Empfehlungen
* Darstellung der Kundensegmente inkl. Visualisierungen
* Videoaufnahme oder Slide-Präsentation mit Personas, KPIs und Beispielen
* Empfehlungen zur Erfolgsmessung der Kampagne

---

## Weiterentwicklung - !!! Nachbearbeiten !!! 

* Integration von Echtzeitdaten aus dem Web-Tracking
* AB-Tests zur Performance der individualisierten Mails
* Automatisiertes Perk-Ranking als API
* Dashboard für das Marketing-Team zur Kampagnensteuerung
