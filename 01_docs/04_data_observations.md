- Table: users
No non-values
No duplicates
1020926 entries
11 columns

Jeder User hat eine unique user_id
Es gibt in keiner Spalte einen fehlenden Wert 
Es gibt keinerlei Duplikate

DISTINCT:
Married: True or False 
has_children: True or False
home_country: usa, canada 
home_city: 105 cities
home_airport: 159 airports

!!!!!! No Data Cleaning in users Table requiert

----------




- Table: sessions

- Non-values: trip_id: 3072218 -> Not every session has a Trip_ID. Conclusion: not every session has a booking
5408063 entries
13 columns

Es gibt 5408063 Sessions, von denen keine 3072218 keine trip_id haben. 
Bedeutet: 2335845 trip_id = 2335845 Buchungen

Es gibt in keiner Spalte einen fehlenden Wert 
Es gibt keinerlei Duplikate

4522267 Entries haben keinen flight-discount 
Bedeutet: 885.796 flight-discounts wurden vergeben

4716683 Entries haben keinen hotel-discount
Bedeutet: 691380 hotel discounts wurden vergeben

Gesamtanzahl Klicks: 5.408.063

Data Cleansing : 
-- Null-Werte in hotel_discount_amount -> wurde ein Discount gewährt hotel_discount (true) oder nicht? 
-- Selbige gilt für flight_discount_amount


- Table: flights

1901038 entries
13 columns

Es fehlen 88734 return_time angeben. Ggf. wurden keine Rückflüge gebucht, bzw. angetreten.
Es gibt keine Duplikate

Es gibt viele sehr kurze Sessiondauer. Die kürzeste ist 7 Sekunden. Springen also direkt wieder ab. 


Nachverfolgen: Woran liegt das? 
---
Es gibt 90670 Einträge, in denen gebucht wurde, Discount aktiviert wurde, und dann auch wieder stoniert wurde. 
2021	3132
2022	32700
2023	54838
---

88.734 Leute haben keinen Rückflug gebucht

Anomalie: 
Es gibt sehr viele Flugdistanzen die unter 20km liegen. Das ist schon sehr unwahrscheinlich für so eine Strecke zu fliegen. 
MOrgens um 7 Uhr in der Früh fliegen ca. 5x mehr Leute, als eine Stunde später.
9851 departure_time = return_time (die Zeiten sind gleich - mit Uhrzeit) Es gibt keine Werte die kleiner sind. Sie sind nur gleich. Ggf. sind die Flüge ausgefallen und haben deshalb für eine Storno gesorgt? 

Es gibt Auffälligkeiten in den LON und LAT: 
destination_airport_lat	destination_airport_lon	destination_count
0	        40.640	            -73.779	              256813
1	        33.942	            -118.408	            132630
Keine Ahnung was man daraus interpretieren soll. 

##### 18. Are one-way flights priced differently than round trips?
	return_flight_booked	flight_count	avg_base_fare
0	      False	        88734	          306.00
1	      True	        1812304	        660.95
Flüge, bei denen ein Rückflug mit gebucht wurde, sind im Durchschnitt fast doppelt so günstig 

Es gibt große Ausreißer: 
##### 20. Is there a relationship between checked bags, base fare, and seat availability?
<img width="415" height="490" alt="Bildschirmfoto 2025-07-12 um 00 42 40" src="https://github.com/user-attachments/assets/b93eadb9-d151-4add-be69-31c396198f62" />



- Table: hotels

Table: hotels

1918617 entries
7 columns
Es gibt keine Null-Werte
Es gibt keine Dupliakte

---
Data Cleansing: 
-- Es gibt negative Hotelnächte // es gibt aber auch Nächte in extremen Werten. Ab 50 Übernachtungen+ bis 107
-- Es gibt check-in / check-out Zeitfenster von 0.0004375 Sekunden. 



In der Tabelle hotels gab es 12.067 nights Einträge, die im negativen Bereich lagen. 
Mit einer Query wird geschaut, ob check_in_time und check_out_time verkehrt herum eingefügt wurden und dreht diese wieder um - auf den korrigierten Werten wird dann eine Aufenthaltsdauer berechnet. 
Es werden alle Hotelbesuche, die unter 24h (also 23:59h) liegen, seperat aufgeführt. 
Alles darüber wird als volle Nächte betrachtet. 

Nachdem die Korrektur für alle 1.918.617 Einträge durchgelaufen ist, wurde ersichtlich, dass es extrem viele Aufenthaltsdauer von sehr kurzer Dauer gibt. 38483 Einträge mit einer Dauer von unter 2h - tendenziell eher weniger. Mögliche Gründe: Fehlbuchungen, Tests, Bots, Stornos. Für weitere Analysen werden nur duration_hours berücksichtigt, die >= 2h sind.

-- Alle Werte, ohne >= 2h Filter -- </br>
![Bildschirmfoto 2025-07-07 um 00 54 32](https://github.com/user-attachments/assets/4a1a759a-6025-4c6b-99bb-56d968854c5f)

-- Nur Werte, mit einem Filter von Werten mit >= 2h </br>
![Bildschirmfoto 2025-07-07 um 00 55 13](https://github.com/user-attachments/assets/dd1c7ac2-33d7-40c3-a30f-dbb7062b1e98)


===========

Es gibt in jeder Stadt immer genau 20 Hotels. 
Das ist schon ungewöhnlich. Da Paris beispielsweise größer ist, als irgendein 

________________________________________________________

Die Hotelauswahl in der Datenbank richtet sich primär an Geschäftsreisender, Luxusreisende 
Alle unterbeteiligten Gruppen fühlen sich ggf. nicht angesprochen, weil es keine Hotels für ihren Geschmack gibt. 


Hotelmarke	Kundensegment
  
Best Western	Geschäftreisender / Budgetreisender
Extended Stay	Geschäftreisender / Budgetreisender
Choice Hotels	Budgetreisender


Crowne Plaza	Geschäftsreisender
Accor	Geschäftsreisender / Budgetreisender (abhängig von Submarke)
Wyndham	Budgetreisender / Familienreisender
Radisson	Geschäftsreisender / Kulturreisender


Conrad	Luxusreisender
Marriott	Geschäftsreisender / Luxusreisender
Fairmont	Luxusreisender 
Aman Resorts	Luxusreisender
Starwood	[Unbestätigt] Luxusreisender / Geschäftsreisender
Shangri-La	Luxusreisender
Hilton	Luxusreisender / Geschäftsreisender
Rosewood	Luxusreisender
Banyan Tree	Luxusreisender
NH Hotel	Geschäftsreisender
Four Seasons	Luxusreisender
InterContinental	Geschäftsreisender / Luxusreisender
Hyatt	Geschäftsreisender / Luxusreisender



Hilton	Leisure & Business	Beschreibung auf Businessmodel-Analyst: Leisure + Business Travelers 
botshot.ai
+2
Business Model Analyst
+2
Placer
+2
InterContinental (IHG)	Corporate & MICE	Teil des IHG-Portfolios mit starker MICE-Ausrichtung (Corporate Rates, Konferenzen) – bestätigt durch Segmentmodell von Cvent/Canary
Marriott/Starwood	Wie Hilton + Extended	Vergleichbare Struktur: Corporate, Transient, Group
Waldorf Astoria	Luxus & Familien	Familiär durch Haushaltsdaten aus Placer.ai
Ritz-Carlton	Luxus & Singles/Empty-nesters	Identifiziert von Placer.ai durch trade-area Analyse
Hyatt Grand/Regency	Business & MICE	Grand/Regency Marken haben große Meeting-Center – laut Hyatt Wikipedia



Beispiel: 
Beobachtung: Alle rabattierten Buchungen in der Tabelle bookings wurden storniert.
Interpretation: Der Rabatt scheint in seiner aktuellen Form keinen loyalitätsfördernden Effekt zu haben.
Relevanz: Die Variable discount_used wird als kritisches Merkmal für zukünftige Segmentierung markiert.

Beobachtung: 
- 
Interpretation:
-
Relevanz: 
-



## Zuwachsraten der neuen Nutzer im Jahresvergleich

| Jahr | Nutzerzahl | Veränderung zum Vorjahr  |
|------|------------|--------------------------|
| 2021 | 75.555     | Startwert                |
| 2022 | 427.441    | +465,84 %                |
| 2023 | 517.930    | +21,16 %                 |

## Beobachtung: 
- Im Jahr 2022 gab es einen temporären, schnellen Wachsstumsschub, der nicht im selben Ausmaß aufrechterhalten werden konnte. 
## Interpretation:
- Es gab einen zeitweiligen Hype, der zu einem überproportional starkem Wachstum geführt hat, ohne dabei die Kunden langfristig ans Unternehmen zu binden. 
- Vielleicht wurde das Wachstum mit irgendwelchen Aktionen künstlich befeuert. 
## Relevanz: 
- Da nur Daten von Nutzern in die Analyse einfließen sollen, die mehr als 7 Sessions haben und nach dem 4. Januar 2023 entstanden sind, könnte diese Beobachtung ggf. unrelevant sein.

## Zuwachsraten nach Kategorie (Jahresvergleich)

| Kategorie | 2021 → 2022 | 2022 → 2023 |
|-----------|-------------|-------------|
| Kids      | +464,00 %   | +21,09 %    |
| No Kids   | +466,58 %   | +21,21 %    |


## Heimatländer

home_country|count |
------------+------+
usa         |848354|
canada      |172572|



--

## Stonierung vs. Nicht stoniert 

new_label     |count_cancellation|
--------------+------------------+
Stoniert      |             90670|
Nicht stoniert|           5317393|

## Nutzerverweildauer auf der Website:

time_category |session_count|
--------------+-------------+
<= 5 Sekunden |            7|
<= 10 Sekunden|        40920|
<= 15 Sekunden|       107770|
<= 30 Sekunden|       505797|
<= 45 Sekunden|       515682|
<= 1 Minute   |       415290|
<= 5 Minuten  |      3468207|
<= 10 Minuten |       238954|
<= 15 Minuten |        24430|
<= 20 Minuten |         6609|
<= 25 Minuten |         3565|
<= 30 Minuten |         2788|
<= 45 Minuten |         6506|
<= 60 Minuten |         5483|
<= 90 Minuten |         8697|
<= 120 Minuten|        57358|


## Nutzer, die stonieren vs. Nutzer die nicht stonieren, gemessen an der Verweildauer auf der Website: 

ew_label     |time_category |session_count|
-------------+--------------+-------------+
icht stoniert|<= 5 Sekunden |            7|
icht stoniert|<= 10 Sekunden|        40920|
icht stoniert|<= 15 Sekunden|       107770|
icht stoniert|<= 30 Sekunden|       505797|
icht stoniert|<= 45 Sekunden|       515682|
icht stoniert|<= 1 Minute   |       415290|
icht stoniert|<= 5 Minuten  |      3468207|
icht stoniert|<= 10 Minuten |       235728|
icht stoniert|<= 15 Minuten |        21931|
icht stoniert|<= 20 Minuten |         4178|
icht stoniert|<= 25 Minuten |         1157|
icht stoniert|<= 30 Minuten |          422|
icht stoniert|<= 45 Minuten |          258|
icht stoniert|<= 60 Minuten |           35|
icht stoniert|<= 90 Minuten |            8|
icht stoniert|<= 120 Minuten|            3|
toniert      |<= 10 Minuten |         3226|
toniert      |<= 15 Minuten |         2499|
toniert      |<= 20 Minuten |         2431|
toniert      |<= 25 Minuten |         2408|
toniert      |<= 30 Minuten |         2366|
toniert      |<= 45 Minuten |         6248|
toniert      |<= 60 Minuten |         5448|
toniert      |<= 90 Minuten |         8689|
toniert      |<= 120 Minuten|        57355|

## Wie verhalten sich die Stornos gegenüber Discounts? 

year_name|new_label      |total_sessions|flight_discount_true|flight_discount_false|flight_discount_null|hotel_discount_true|hotel_discount_false|hotel_discount_null|
---------+---------------+--------------+--------------------+---------------------+--------------------+-------------------+--------------------+-------------------+
2021     |Nicht storniert|        270929|               45275|               225654|                   0|              35265|              235664|                  0|
2022     |Nicht storniert|       1942854|              323476|              1619378|                   0|             252069|             1690785|                  0|
2023     |Nicht storniert|       3103610|              517045|              2586565|                   0|             404046|             2699564|                  0|
2021     |Storniert      |          3132|                3132|                    0|                   0|               3132|                   0|                  0|
2022     |Storniert      |         32700|               32700|                    0|                   0|              32700|                   0|                  0|
2023     |Storniert      |         54838|               54838|                    0|                   0|              54838|                   0|                  0|

Beobachtung: 
- Ausnahmslos alle Stornos hatten einen Flug und Hoteldiscount.
Interpretation:
- Vielleicht erfüllen die Discounts nicht ihren Zweck. Andererseits könnte es eine vorherige Marketingaktion gewesen sein. 
- Vielleicht gibt es Anbieter auf unserer Plattform die nicht mehr die Qualiätsstandarts erfüllen und es zu immer mehr Reklamationen kommt, weil Online zuviele schlechte Bewertungen exisitieren.
Relevanz: 
- Das Stornoverhalten hat einen massiven Einfluss und ist daher sehr relevant. 

## Männer und Frauen stonieren gleichermaßen die Flüge und Hotels 

year_name|gender|new_label      |total_sessions|flight_discount_true|flight_discount_false|flight_discount_null|hotel_discount_true|hotel_discount_false|hotel_discount_null|
---------+------+---------------+--------------+--------------------+---------------------+--------------------+-------------------+--------------------+-------------------+
2021     |O     |Nicht storniert|          2128|                 366|                 1762|                   0|                277|                1851|                  0|
2021     |F     |Nicht storniert|        131629|               22017|               109612|                   0|              16999|              114630|                  0|
2021     |M     |Nicht storniert|        137172|               22892|               114280|                   0|              17989|              119183|                  0|
2022     |O     |Nicht storniert|         14310|                2460|                11850|                   0|               1904|               12406|                  0|
2022     |F     |Nicht storniert|        947515|              157418|               790097|                   0|             123184|              824331|                  0|
2022     |M     |Nicht storniert|        981029|              163598|               817431|                   0|             126981|              854048|                  0|
2023     |O     |Nicht storniert|         23186|                3953|                19233|                   0|               3032|               20154|                  0|
2023     |M     |Nicht storniert|       1559011|              259698|              1299313|                   0|             203748|             1355263|                  0|
2023     |F     |Nicht storniert|       1521413|              253394|              1268019|                   0|             197266|             1324147|                  0|
2021     |M     |Storniert      |          1603|                1603|                    0|                   0|               1603|                   0|                  0|
2021     |F     |Storniert      |          1500|                1500|                    0|                   0|               1500|                   0|                  0|
2021     |O     |Storniert      |            29|                  29|                    0|                   0|                 29|                   0|                  0|
2022     |F     |Storniert      |         16324|               16324|                    0|                   0|              16324|                   0|                  0|
2022     |M     |Storniert      |         16145|               16145|                    0|                   0|              16145|                   0|                  0|
2022     |O     |Storniert      |           231|                 231|                    0|                   0|                231|                   0|                  0|
2023     |O     |Storniert      |           394|                 394|                    0|                   0|                394|                   0|                  0|
2023     |M     |Storniert      |         27041|               27041|                    0|                   0|              27041|                   0|                  0|
2023     |F     |Storniert      |         27403|               27403|                    0|                   0|              27403|                   0|                  0|


## Stornos / Nicht Stornos zwischen Nutzer mit Kindern / ohne Kindern

year_name|has_children|new_label      |total_sessions|flight_discount_true|flight_discount_false|flight_discount_null|hotel_discount_true|hotel_discount_false|hotel_discount_null|
---------+------------+---------------+--------------+--------------------+---------------------+--------------------+-------------------+--------------------+-------------------+
2021     |true        |Nicht storniert|         85322|               14335|                70987|                   0|              11224|               74098|                  0|
2022     |true        |Nicht storniert|        610221|              101529|               508692|                   0|              79244|              530977|                  0|
2023     |true        |Nicht storniert|        973835|              161976|               811859|                   0|             126871|              846964|                  0|
2021     |false       |Nicht storniert|        185607|               30940|               154667|                   0|              24041|              161566|                  0|
2022     |false       |Nicht storniert|       1332633|              221947|              1110686|                   0|             172825|             1159808|                  0|
2023     |false       |Nicht storniert|       2129775|              355069|              1774706|                   0|             277175|             1852600|                  0|
2021     |true        |Storniert      |          1037|                1037|                    0|                   0|               1037|                   0|                  0|
2022     |true        |Storniert      |         10810|               10810|                    0|                   0|              10810|                   0|                  0|
2023     |true        |Storniert      |         18165|               18165|                    0|                   0|              18165|                   0|                  0|
2021     |false       |Storniert      |          2095|                2095|                    0|                   0|               2095|                   0|                  0|
2022     |false       |Storniert      |         21890|               21890|                    0|                   0|              21890|                   0|                  0|
2023     |false       |Storniert      |         36673|               36673|                    0|                   0|              36673|                   0|                  0|

Beobachtung: 
- Nutzer ohne Kinder haben im Zeitverlauf eine erhöhte Stornoquote. Fast 3/4 der Stornos kommen aus diesem Gruppe.
Interpretation:
Relevanz: 

## Wie häufig werden bestimmte Discounts für Flüge gegeben?

flight_discount_amount|count_flight_discount_amount|
----------------------+----------------------------+
                      |                        0.00|
                  0.85|                        1.00|
                  0.75|                        7.00|
                  0.70|                       36.00|
                  0.65|                      110.00|
                  0.60|                      318.00|
                  0.55|                      895.00|
                  0.50|                     2043.00|
                  0.45|                     4470.00|
                  0.40|                     9476.00|
                  0.35|                    17994.00|
                  0.30|                    33697.00|
                  0.25|                    59984.00|
                  0.20|                   104010.00|
                  0.15|                   172989.00|
                  0.10|                   281044.00|
                  0.05|                   198722.00|

Beobachtung: 
- Es gibt extrem viele Discounts in den unteren Rabatt-Aktionen bei Flügen.  
Interpretation: 
- Vielleicht stonieren soviele Nutzer, weil sie ein besseres Angebot gefunden haben. 
Relevanz: 


## Wie häufig werden bestimmte Discounts für Hotels gegeben?

hotel_discount_amount|count_hotel_discount_amount|
---------------------+---------------------------+
                     |                       0.00|
                 0.65|                       1.00|
                 0.60|                       2.00|
                 0.55|                      39.00|
                 0.50|                     131.00|
                 0.45|                     455.00|
                 0.40|                    1327.00|
                 0.35|                    3646.00|
                 0.30|                    9496.00|
                 0.25|                   23515.00|
                 0.20|                   55051.00|
                 0.15|                  121033.00|
                 0.10|                  257620.00|
                 0.05|                  219064.00|

Beobachtung: 
Interpretation: 
Relevanz: 

## In welchen Monaten wird die Website am meisten geklickt? 

month_name|sum_clicks|
----------+----------+
Mär       |  12624406|
Jun       |  12352464|
Mai       |  11232776|
Jan       |  10883017|
Jul       |  10816922|
Feb       |  10153591|
Apr       |   9296871|
Dez       |   6632347|
Nov       |   6080697|
Aug       |   4460434|
Okt       |   3738864|
Sep       |   3215488|

## Sessions mit Klicks über die Jahre verteilt, in den jeweiligen Verweildauern kategorisiert

e_category |year_name|total_sessions|sessions_with_clicks|total_clicks|
-----------+---------+--------------+--------------------+------------+
5 Sekunden |2021     |             2|                   2|           0|
10 Sekunden|2021     |          2072|                2072|        2072|
15 Sekunden|2021     |          5567|                5567|       11134|
30 Sekunden|2021     |         25924|               25924|       82741|
45 Sekunden|2021     |         26197|               26197|      133213|
1 Minute   |2021     |         21153|               21153|      150179|
5 Minuten  |2021     |        176599|              176599|     3408865|
10 Minuten |2021     |         12092|               12092|      625532|
15 Minuten |2021     |          1195|                1195|      106309|
20 Minuten |2021     |           299|                 299|       30454|
25 Minuten |2021     |           127|                 127|       11488|
30 Minuten |2021     |            93|                  93|        6977|
45 Minuten |2021     |           242|                 242|       12852|
60 Minuten |2021     |           169|                 169|        9910|
90 Minuten |2021     |           290|                 290|       21321|
120 Minuten|2021     |          2040|                2040|      368683|
5 Sekunden |2022     |             4|                   4|           0|
10 Sekunden|2022     |         14986|               14986|       14986|
15 Sekunden|2022     |         39486|               39486|       78971|
30 Sekunden|2022     |        184667|              184667|      587659|
45 Sekunden|2022     |        188214|              188214|      958009|
1 Minute   |2022     |        152058|              152058|     1078771|
5 Minuten  |2022     |       1267043|             1267043|    24497402|
10 Minuten |2022     |         87340|               87340|     4500234|
15 Minuten |2022     |          8812|                8812|      768078|
20 Minuten |2022     |          2458|                2458|      232752|
25 Minuten |2022     |          1314|                1314|      100850|
30 Minuten |2022     |           997|                 997|       56634|
45 Minuten |2022     |          2353|                2353|      108335|
60 Minuten |2022     |          1935|                1935|      105355|
90 Minuten |2022     |          3205|                3205|      236914|
120 Minuten|2022     |         20682|               20682|     3701449|
5 Sekunden |2023     |             1|                   1|           0|
10 Sekunden|2023     |         23862|               23862|       23862|
15 Sekunden|2023     |         62717|               62717|      125434|
30 Sekunden|2023     |        295206|              295206|      940808|
45 Sekunden|2023     |        301271|              301271|     1532747|
1 Minute   |2023     |        242079|              242079|     1717600|
5 Minuten  |2023     |       2024565|             2024565|    39144110|
10 Minuten |2023     |        139522|              139522|     7192242|
15 Minuten |2023     |         14423|               14423|     1246813|
20 Minuten |2023     |          3852|                3852|      351214|
25 Minuten |2023     |          2124|                2124|      147142|
30 Minuten |2023     |          1698|                1698|       93642|
45 Minuten |2023     |          3911|                3911|      182465|
60 Minuten |2023     |          3379|                3379|      182524|
90 Minuten |2023     |          5202|                5202|      389137|
120 Minuten|2023     |         34636|               34636|     6210008|


## Nutzer-Session Onlinezeit - pro Uhrzeit und Tag. 

weekday|weekday_index|time_block |session_count|hour_index|
-------+-------------+-----------+-------------+----------+
So     |            0|00:00–00:59|         4182|         0|
So     |            0|01:00–01:59|         8806|         1|
So     |            0|02:00–02:59|         9774|         2|
So     |            0|03:00–03:59|        10683|         3|
So     |            0|04:00–04:59|        12105|         4|
So     |            0|05:00–05:59|        13303|         5|
So     |            0|06:00–06:59|        14943|         6|
So     |            0|07:00–07:59|        16700|         7|
So     |            0|08:00–08:59|        18823|         8|
So     |            0|09:00–09:59|        21378|         9|
So     |            0|10:00–10:59|        24169|        10|
So     |            0|11:00–11:59|        27378|        11|
So     |            0|12:00–12:59|        31004|        12|
So     |            0|13:00–13:59|        35505|        13|
So     |            0|14:00–14:59|        40758|        14|
So     |            0|15:00–15:59|        46355|        15|
So     |            0|16:00–16:59|        52862|        16|
So     |            0|17:00–17:59|        59750|        17|
So     |            0|18:00–18:59|        67077|        18|
So     |            0|19:00–19:59|        72456|        19|
So     |            0|20:00–20:59|        74459|        20|
So     |            0|21:00–21:59|        64664|        21|
So     |            0|22:00–22:59|        35669|        22|
So     |            0|23:00–23:59|         5115|        23|
Mo     |            1|00:00–00:59|         4181|         0|
Mo     |            1|01:00–01:59|         9016|         1|
Mo     |            1|02:00–02:59|         9971|         2|
Mo     |            1|03:00–03:59|        10856|         3|
Mo     |            1|04:00–04:59|        12278|         4|
Mo     |            1|05:00–05:59|        13559|         5|
Mo     |            1|06:00–06:59|        15124|         6|
Mo     |            1|07:00–07:59|        16985|         7|
Mo     |            1|08:00–08:59|        19086|         8|
Mo     |            1|09:00–09:59|        21457|         9|
Mo     |            1|10:00–10:59|        24415|        10|
Mo     |            1|11:00–11:59|        27427|        11|
Mo     |            1|12:00–12:59|        31367|        12|
Mo     |            1|13:00–13:59|        35920|        13|
Mo     |            1|14:00–14:59|        41012|        14|
Mo     |            1|15:00–15:59|        46680|        15|
Mo     |            1|16:00–16:59|        53197|        16|
Mo     |            1|17:00–17:59|        60455|        17|
Mo     |            1|18:00–18:59|        67679|        18|
Mo     |            1|19:00–19:59|        73988|        19|
Mo     |            1|20:00–20:59|        75454|        20|
Mo     |            1|21:00–21:59|        65727|        21|
Mo     |            1|22:00–22:59|        35790|        22|
Mo     |            1|23:00–23:59|         5138|        23|
Di     |            2|00:00–00:59|         4081|         0|
Di     |            2|01:00–01:59|         8886|         1|
Di     |            2|02:00–02:59|         9770|         2|
Di     |            2|03:00–03:59|        10906|         3|
Di     |            2|04:00–04:59|        12364|         4|
Di     |            2|05:00–05:59|        13578|         5|
Di     |            2|06:00–06:59|        15335|         6|
Di     |            2|07:00–07:59|        17077|         7|
Di     |            2|08:00–08:59|        19157|         8|
Di     |            2|09:00–09:59|        21519|         9|
Di     |            2|10:00–10:59|        24372|        10|
Di     |            2|11:00–11:59|        27671|        11|
Di     |            2|12:00–12:59|        31080|        12|
Di     |            2|13:00–13:59|        36224|        13|
Di     |            2|14:00–14:59|        40860|        14|
Di     |            2|15:00–15:59|        47058|        15|
Di     |            2|16:00–16:59|        53870|        16|
Di     |            2|17:00–17:59|        60219|        17|
Di     |            2|18:00–18:59|        67911|        18|
Di     |            2|19:00–19:59|        73639|        19|
Di     |            2|20:00–20:59|        75193|        20|
Di     |            2|21:00–21:59|        65866|        21|
Di     |            2|22:00–22:59|        35803|        22|
Di     |            2|23:00–23:59|         5213|        23|
Mi     |            3|00:00–00:59|         4142|         0|
Mi     |            3|01:00–01:59|         8853|         1|
Mi     |            3|02:00–02:59|         9947|         2|
Mi     |            3|03:00–03:59|        10782|         3|
Mi     |            3|04:00–04:59|        12297|         4|
Mi     |            3|05:00–05:59|        13531|         5|
Mi     |            3|06:00–06:59|        14897|         6|
Mi     |            3|07:00–07:59|        17073|         7|
Mi     |            3|08:00–08:59|        19021|         8|
Mi     |            3|09:00–09:59|        21822|         9|
Mi     |            3|10:00–10:59|        24397|        10|
Mi     |            3|11:00–11:59|        27590|        11|
Mi     |            3|12:00–12:59|        31145|        12|
Mi     |            3|13:00–13:59|        36159|        13|
Mi     |            3|14:00–14:59|        41345|        14|
Mi     |            3|15:00–15:59|        46928|        15|
Mi     |            3|16:00–16:59|        53695|        16|
Mi     |            3|17:00–17:59|        60768|        17|
Mi     |            3|18:00–18:59|        68074|        18|
Mi     |            3|19:00–19:59|        73672|        19|
Mi     |            3|20:00–20:59|        75220|        20|
Mi     |            3|21:00–21:59|        65673|        21|
Mi     |            3|22:00–22:59|        36276|        22|
Mi     |            3|23:00–23:59|         5231|        23|
Do     |            4|00:00–00:59|         4112|         0|
Do     |            4|01:00–01:59|         9008|         1|
Do     |            4|02:00–02:59|         9984|         2|
Do     |            4|03:00–03:59|        11068|         3|
Do     |            4|04:00–04:59|        12461|         4|
Do     |            4|05:00–05:59|        13730|         5|
Do     |            4|06:00–06:59|        15127|         6|
Do     |            4|07:00–07:59|        16973|         7|
Do     |            4|08:00–08:59|        19412|         8|
Do     |            4|09:00–09:59|        21644|         9|
Do     |            4|10:00–10:59|        24676|        10|
Do     |            4|11:00–11:59|        27946|        11|
Do     |            4|12:00–12:59|        31557|        12|
Do     |            4|13:00–13:59|        35812|        13|
Do     |            4|14:00–14:59|        41374|        14|
Do     |            4|15:00–15:59|        47252|        15|
Do     |            4|16:00–16:59|        54002|        16|
Do     |            4|17:00–17:59|        60981|        17|
Do     |            4|18:00–18:59|        67913|        18|
Do     |            4|19:00–19:59|        74391|        19|
Do     |            4|20:00–20:59|        74974|        20|
Do     |            4|21:00–21:59|        66023|        21|
Do     |            4|22:00–22:59|        36266|        22|
Do     |            4|23:00–23:59|         5228|        23|
Fr     |            5|00:00–00:59|         4155|         0|
Fr     |            5|01:00–01:59|         8621|         1|
Fr     |            5|02:00–02:59|         9593|         2|
Fr     |            5|03:00–03:59|        10690|         3|
Fr     |            5|04:00–04:59|        11795|         4|
Fr     |            5|05:00–05:59|        13479|         5|
Fr     |            5|06:00–06:59|        14859|         6|
Fr     |            5|07:00–07:59|        16681|         7|
Fr     |            5|08:00–08:59|        18933|         8|
Fr     |            5|09:00–09:59|        20856|         9|
Fr     |            5|10:00–10:59|        23921|        10|
Fr     |            5|11:00–11:59|        27333|        11|
Fr     |            5|12:00–12:59|        30884|        12|
Fr     |            5|13:00–13:59|        35110|        13|
Fr     |            5|14:00–14:59|        40507|        14|
Fr     |            5|15:00–15:59|        45698|        15|
Fr     |            5|16:00–16:59|        52440|        16|
Fr     |            5|17:00–17:59|        59245|        17|
Fr     |            5|18:00–18:59|        66277|        18|
Fr     |            5|19:00–19:59|        72893|        19|
Fr     |            5|20:00–20:59|        74021|        20|
Fr     |            5|21:00–21:59|        64759|        21|
Fr     |            5|22:00–22:59|        35260|        22|
Fr     |            5|23:00–23:59|         5116|        23|
Sa     |            6|00:00–00:59|         4191|         0|
Sa     |            6|01:00–01:59|         8739|         1|
Sa     |            6|02:00–02:59|         9746|         2|
Sa     |            6|03:00–03:59|        10813|         3|
Sa     |            6|04:00–04:59|        11918|         4|
Sa     |            6|05:00–05:59|        13161|         5|
Sa     |            6|06:00–06:59|        14795|         6|
Sa     |            6|07:00–07:59|        16841|         7|
Sa     |            6|08:00–08:59|        18906|         8|
Sa     |            6|09:00–09:59|        20974|         9|
Sa     |            6|10:00–10:59|        23854|        10|
Sa     |            6|11:00–11:59|        27239|        11|
Sa     |            6|12:00–12:59|        30974|        12|
Sa     |            6|13:00–13:59|        35364|        13|
Sa     |            6|14:00–14:59|        40160|        14|
Sa     |            6|15:00–15:59|        46001|        15|
Sa     |            6|16:00–16:59|        52553|        16|
Sa     |            6|17:00–17:59|        59071|        17|
Sa     |            6|18:00–18:59|        66589|        18|
Sa     |            6|19:00–19:59|        72195|        19|
Sa     |            6|20:00–20:59|        73621|        20|
Sa     |            6|21:00–21:59|        63982|        21|
Sa     |            6|22:00–22:59|        35338|        22|
Sa     |            6|23:00–23:59|         5128|        23|


Beobachtung: Die meisten Nutzer sind Abends zwischen 18 und 23 Uhr Online. 
Interpretation: 
Relevanz: 

## Zu welcher Uhrzeit wird am häufigsten und am wenigsten stoniert 

time_block |new_label      |session_count|hour_index|
-----------+---------------+-------------+----------+
00:00–00:59|Nicht storniert|        28565|         0|
01:00–01:59|Nicht storniert|        60924|         1|
02:00–02:59|Nicht storniert|        67590|         2|
03:00–03:59|Nicht storniert|        74521|         3|
04:00–04:59|Nicht storniert|        83840|         4|
05:00–05:59|Nicht storniert|        92760|         5|
06:00–06:59|Nicht storniert|       103349|         6|
07:00–07:59|Nicht storniert|       116357|         7|
08:00–08:59|Nicht storniert|       131142|         8|
09:00–09:59|Nicht storniert|       147229|         9|
10:00–10:59|Nicht storniert|       166976|        10|
11:00–11:59|Nicht storniert|       189304|        11|
12:00–12:59|Nicht storniert|       214349|        12|
13:00–13:59|Nicht storniert|       245951|        13|
14:00–14:59|Nicht storniert|       281241|        14|
15:00–15:59|Nicht storniert|       320595|        15|
16:00–16:59|Nicht storniert|       366409|        16|
17:00–17:59|Nicht storniert|       413548|        17|
18:00–18:59|Nicht storniert|       463645|        18|
19:00–19:59|Nicht storniert|       504533|        19|
20:00–20:59|Nicht storniert|       514080|        20|
21:00–21:59|Nicht storniert|       448986|        21|
22:00–22:59|Nicht storniert|       246106|        22|
23:00–23:59|Nicht storniert|        35393|        23|
00:00–00:59|Storniert      |          479|         0|
01:00–01:59|Storniert      |         1005|         1|
02:00–02:59|Storniert      |         1195|         2|
03:00–03:59|Storniert      |         1277|         3|
04:00–04:59|Storniert      |         1378|         4|
05:00–05:59|Storniert      |         1581|         5|
06:00–06:59|Storniert      |         1731|         6|
07:00–07:59|Storniert      |         1973|         7|
08:00–08:59|Storniert      |         2196|         8|
09:00–09:59|Storniert      |         2421|         9|
10:00–10:59|Storniert      |         2828|        10|
11:00–11:59|Storniert      |         3280|        11|
12:00–12:59|Storniert      |         3662|        12|
13:00–13:59|Storniert      |         4143|        13|
14:00–14:59|Storniert      |         4775|        14|
15:00–15:59|Storniert      |         5377|        15|
16:00–16:59|Storniert      |         6210|        16|
17:00–17:59|Storniert      |         6941|        17|
18:00–18:59|Storniert      |         7875|        18|
19:00–19:59|Storniert      |         8701|        19|
20:00–20:59|Storniert      |         8862|        20|
21:00–21:59|Storniert      |         7708|        21|
22:00–22:59|Storniert      |         4296|        22|
23:00–23:59|Storniert      |          776|        23|

## Stonierung an Wochentagen

weekday|weekday_index|new_label      |session_count|
-------+-------------+---------------+-------------+
So     |            0|Nicht storniert|       755061|
Mo     |            1|Nicht storniert|       763813|
Di     |            2|Nicht storniert|       764744|
Mi     |            3|Nicht storniert|       765545|
Do     |            4|Nicht storniert|       768866|
Fr     |            5|Nicht storniert|       750039|
Sa     |            6|Nicht storniert|       749325|
So     |            0|Storniert      |        12857|
Mo     |            1|Storniert      |        12949|
Di     |            2|Storniert      |        12908|
Mi     |            3|Storniert      |        12993|
Do     |            4|Storniert      |        13048|
Fr     |            5|Storniert      |        13087|
Sa     |            6|Storniert      |        12828|

Beobachtung: Insofern der Syntax nicht verkehrt ist, gibt es kein außergewöhnliches Verhalten zu bestimmten Stonierungsverhalten an Wochentagen.
Interpretation: 
Relevanz: 





Annahme: Kündigen die Leute, weil ihnen ein Rabbat versprochen wurde, den man dann nicht gewährt hat bzw. verrechnet wurde?


-- Flug und Hotel wurde gebucht und dann wieder stoniert. Es 

select count(*)
from sessions
where cancellation = false					
and flight_booked = true					
and hotel_booked = true	
and flight_discount = true 
and hotel_discount = true
and flight_discount_amount is null 			
and hotel_discount_amount is null  

========
Output: 0
========


-- Flug und Hotel wurden nach der Buchung nicht stoniert. Es gab auch keinen Rabatt. 

select *
from sessions
where cancellation = false					
and flight_booked = true					
and hotel_booked = true						
and flight_discount_amount is null 			
and hotel_discount_amount is null

========
Output: 1.181.744
========

-- Hotel und Flug wurden gebucht. Flug und Hotelrabatt wurden aktiviert. Die Rabatts wurden aber nicht angerechnet. Jahresvergleich. 

select 
CASE
		WHEN EXTRACT(YEAR FROM session_start) = 2021 THEN '2021'
		WHEN EXTRACT(YEAR FROM session_start) = 2022 THEN '2022'
		WHEN EXTRACT(YEAR FROM session_start) = 2023 THEN '2023'
		ELSE 'Unknown'
		END AS year_name,
		count(*)
from sessions
where cancellation = true					
and flight_booked = true					
and hotel_booked = true	
and flight_discount = true 
and hotel_discount = true
and flight_discount_amount is null 			
and hotel_discount_amount is null
group by year_name;

========
Output: 


year_name|count|
---------+-----+
2021     | 3132|
2022     |32700|
2023     |54838|

========


Gibt es einen Fluganbieter oder einen Hotelanbieter, die Leute mit falschen Angeboten locken?


















