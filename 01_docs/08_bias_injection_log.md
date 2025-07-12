Kundensegmentierung auf Grundlage von: 
hotel_namen bei Buchung. 
    Beispiel: 
    Spontaneous Adventurers: Wyndham – günstige Stadthotels, flexibel
    Business Travelers: NH Hotels – Business-Lage, MICE-kompatibel
    Luxury Travelers: Aman – Luxus, Exklusivität, Natur
    Family Travelers: Marriott (Residence Inn, Autograph Collection mit Familienausrichtung)
    Budget Travelers: Choice Hotels (Econo Lodge, Rodeway Inn etc.)
    Young Travelers: Moxy (Marriott) – junges Design, Events, Instagram-friendly
    
airline bei Buchung? 


check_in_time & check_out_time korrigiert, wenn sie vertausch war und darauf neue nächte berechnet: 

- wenn check_in_time > check_out_time = nichts tauschen - nächte auf Werten berechnen
- wenn check_in_time < check_out_time = Werte vertauschen - nächte auf Werten berechnen
