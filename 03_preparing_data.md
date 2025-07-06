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





