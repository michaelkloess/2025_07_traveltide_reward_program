-----------------------------------------------
Table: users

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

-----------------------------------------------
Table: sessions

Non-values: trip_id: 3072218 -> Not every session has a Trip_ID. Conclusion: not every session has a booking
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



-----------------------------------------------
Table: flights


1901038 entries
13 columns

Es fehlen 88734 return_time angeben. Ggf. wurden keine Rückflüge gebucht, bzw. angetreten.
Es gibt keine Duplikate




Nachverfolgen: Woran liegt das? 
---
Es gibt 90670 Einträge, in denen gebucht wurde, Discount aktiviert wurde, und dann auch wieder stoniert wurde. 
2021	3132
2022	32700
2023	54838
---

88.734 Leute haben keinen Rückflug gebucht




-----------------------------------------------
Table: hotels

1918617 entries
7 columns
Es gibt keine Null-Werte
Es gibt keine Dupliakte

---
Data Cleansing: 
-- Es gibt negative Hotelnächte
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






















