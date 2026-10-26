# Strassenpräzises Opening-Momentum

Jede Opening-News mit Latitude und Longitude wird dynamisch der **nächstgelegenen modellierten Premiumstrasse** zugeordnet. Die Zuordnung gilt ausschliesslich, wenn der kürzeste Abstand zur Strassengeometrie **höchstens 200 Meter** beträgt. Punkte ausserhalb dieses Radius oder Städte ohne gepflegte Strassengeometrie werden nicht künstlich zugeordnet.

Zusätzlich ist eine Zuordnung für Momentum und Grade nur zulässig, wenn **Strasse und Hausnummer** bestätigt sind. Die strukturierte Pipeline extrahiert eine Hausnummer nur, wenn sie in der Quelle explizit vorkommt; Google Places muss mindestens Brand, vollständige Strasse und eine Hausnummer in der formatierten Adresse bestätigen. Stimmen eine aus der Quelle stammende und eine von Google gelieferte Hausnummer nicht überein, wird der Datensatz verworfen. Liegt in der Quelle keine Nummer vor, darf Google Places die Hausnummer liefern – jedoch ausschliesslich bei eindeutigem Marken- und Strassentreffer.

## Vertrauensprüfung der Geodaten

Ein Opening-Momentum darf nur aus einer räumlich belastbaren Datenbasis entstehen. Die Prüfung erfolgt für jede Kombination aus Stadt und Land und bewertet die Verteilung aller geolokalisierten Punkte.

| Prüfkriterium | Konsequenz |
|---|---|
| Keine Koordinaten | `untrusted` – kein Momentum |
| Genau ein eindeutiger Punkt | `untrusted` – Stadt-Zentroid, kein Momentum |
| Weniger als fünf eindeutige Punkte | `untrusted` – zu geringe räumliche Vielfalt |
| Ein Punkt repräsentiert mindestens 80 % aller Punkte | `untrusted` – Dominanz eines Zentroids |
| Mindestens fünf Punkte, keiner dominiert | `trusted` – Momentum aktivierbar |

Der Trust-Status ist absichtlich konservativ. Er verhindert, dass stadtweit wiederverwendete Koordinaten fälschlich einer einzelnen High Street zugerechnet werden. In der aktuellen Zürich-Stichprobe repräsentierte der dominierende Standort **98,3 %** der geolokalisierten Meldungen; daher bleibt das Momentum korrekt deaktiviert. Der historische Bestand enthält gegenwärtig noch keine hausnummerngenau validierten Nachrichten; deshalb liefert er bewusst **keinen** Momentum-Bonus, bis die laufende Pipeline echte Adressbestätigungen erzeugt.

## Momentum und Grade

Für vertrauenswürdige Daten zählt der Service die innerhalb des gewählten Zeitfensters (Standard: 12 Monate) zugeordneten Eröffnungen. Der daraus berechnete Bonus ist auf **maximal +4** begrenzt. Bei `untrusted` ist der Bonus immer **0**. Die Hilfsfunktion `applyOpeningMomentumToGrade(baseGrade, openings, trust)` addiert den Bonus nur dann zum vorhandenen Basis-Grade, wenn die Prüfregeln erfüllt sind.

Der öffentliche Datenendpunkt `analytics.streetMomentum` liefert Strasse, Zuordnungsanzahl, Trust-Status, eligible Openings und Momentum-Bonus. Sobald die laufende Importpipeline zusätzliche strassenpräzise Google-Places-Koordinaten liefert, aktiviert sich der Bonus ohne Lockerung der Regeln automatisch.
