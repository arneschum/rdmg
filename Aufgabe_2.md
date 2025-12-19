![alt text](./imgs/cover_a2.png)

# Aufgabe 2: Standortanalyse

- Kreieren Sie ein Konto bei NASA‘s Earth Observing System Data and Information System (EOSDIS)
- https://search.earthdata.nasa.gov/search
- Geben Sie in der Suchmaske „Aster“ ein und clicken sie auf „ASTER Global Digital Elevation Model V003“
  - ! Achtung: Diesen Eintrag gibt es 2 mal, achten Sie auf das GDEM im GeoTIFF-Format
  - Zoomen Sie auf den Harz und spannen dort ein Rectangle auf
  - Downloaden Sie:
    - ASTGTMV003_N51E010.zip
- Sie können die Aster-Daten auch über den Earth Explorer herunterladen.
  - Gehen Sie auf den Tab “Data Sets” – Digital Elevation – Aster Global DEM 
  - Definieren Sie das Rechteck über Search Criteria – Coordinates – Use Map und ändern Sie das Rechteck entsprechend unserer Gegend

**1. Was ist Aster? (Kurze Erklärung). Wie wird insbesondere das Höhenmodell gewonnen?**

- Laden Sie das Höhenmodell „ASTGTM2_N51E010_dem“in QGIS 

**2. Welche Projektion hat das Raster? Welche Länge der Halbachsen liegen diesem Ellipsoid zugrunde und wie groß ist die Abplattung?**

- Stellen Sie das Kartenkoordinatensystem auf UTM32 um 
- Erstellen und speichern Sie ein neues Polygon- Shapefile in UTM 32
- Schalten Sie das Shapefile in Editiermodus und erweitern Sie das Menü um die Leiste: „Erweiterte Digitalisierungswerkzeugleiste“ (Advanced Digitizing)
  - Clicken Sie erst auf Add Feature (Editierleiste) und danach auf „Erweiterte Digitalisierungswerkzeuge einschalten“  
  ![alt text](imgs/advanced_digitizing.png)

- Geben Sie in den erweiterten Einstellungen die Koordinaten: 575000 (X) und 5760000 (Y) an und sperren Sie beide Koordinaten
- Bestätigen Sie die Koordinate im Kartenfenster (Click überall möglich) und die Koordinate wird gesetzt
- Geben Sie die zweite Koordinate mit 634000 und 5760000 an und sperren Sie wiederum die Koordinaten
- Bestätigen Sie die zweite Koordinate wieder mit Click ins Kartenfenster
- Wiederholen sie den Vorgang für Koordinate 3 (634000, 5700000), Koordinate 4 (575000, 5700000) und wiederum der Ursprungskoordinate (575000, 5760000)
- Das Polygon ist nun geschlossen. Mit einem Rechtsclick vergeben Sie noch die ID 1 und das Polygon ist fertig.
- Speichern Sie das Polygon und beenden Sie den Editiermodus
- Wir haben ein Polygon erstellt, das wir zum Clippen benutzen wollen

- Transformieren Sie mit dem Tool „Warp“ (dt. „Transformieren (Reprojizieren“)  das Höhenmodell in UTM 32 (Datum: ETRS-89)
**3. Welche ESPG-Codes müssen wir für Source SRS und Target SRS angeben?**
    - Lassen Sie die anderen Parameter bitte voreingestellt!
    - Output: N51E010_utm32_dem.tif
- Clippen Sie nun das transformierte Raster mit unserem Shapefile. Geben Sie dafür unser Shapefile als „Mask Layer“ an!
    - Output: ausschnitt_harz_utm32.tif

- Erstellen Sie nun die Höhenlinien (Contour) aus dem Höhenmodell
    - Geben Sie hierfür ein Outputshapefile an, 100m als Intervallgrenze und Attribute name: ELEV
- Schauen sie im Original-Höhenmodell unter Eigenschaften in den Statistikwerten nach, was der höchste Punkt ist.

**4a. Stimmt dieser Wert? Wie kriegen wir für unser Höhenmodell die Einstellungen so angepasst, dass auch unsere Extremwerte richtig in der Statistik angezeigt werden?**
- Finden Sie anhand der Höhenlinien den ungefähren Punkt der höchsten Erhebung auf dem Raster
**4b. 	Bei welcher Koordinate liegt dieser ca., wie heißt er und wie ist die Höhe des Berges im Vergleich durch eine Internetrecherche?**

**5. Schauen Sie sich in den Metadaten des geclippten 	Höhenmodells die Pixelauflösung an. Was stellen Sie fest?**
- Dies ist für eine Vielzahl an Tools in QGIS nicht geeignet. Wir müssen einen Weg finden eine exakte räumliche Auflösung für x und y zu finden. Dies ist nicht über die Maske „Transformieren“ möglich
- In der Warp-Maske sehen Sie unten einen Kommandozeilen-Befehl. Diesen müssen wir in der Kommandozeile (cmd.exe) anwenden, um das gewünschte Resultat zu erzielen.
- Dies sind mächtige Kommandozeilenwerkzeuge, die z.B. im batch processing Verwendung finden (um Massendaten zu automatisieren)

**6a. 	Finden Sie das Verzeichnis, wo sich die „gdalwarp.exe“ versteckt. Wo ist das? 
6b.	Suchen Sie sich aus diesem Ordner ein weiteres GDAL-Tool aus und erklären Sie dessen Bedeutung!**
- Öffnen Sie die Kommandozeile „cmd“ und ändern Sie den Pfad auf das Verzeichnis mit den GDAL-Tools

```bash
cd “Pfad zum Verzeichnis“
gdalwarp
```

- Sie sollten nun die Hilfe für das Werkzeug erhalten
Kreieren Sie eine *.bat-Datei und öffnen Sie diese. In die erste Zeile kommt der cd-Befehl
!! Arbeiten Sie auf einem anderen Laufwerk (d.h .sie schreiben die Batch z.B. auf einem USB-Stick müssen 3 Zeilen angeben werden, hier steht vor dem cd-Befehl noch die Änderungen des Laufwerks durch z.B. „C:“ 
Nun (zweite bzw. dritte Zeile) müssen wir zunächst gdalwarp angeben:
gdalwarp –flag Wert1 –flag2 Wert2 ….
Hier müssen Sie Parameter wie Quell-CRS, Ziel-CRS, Clip-Ausdehnung, die x,y Auflösung (wir wollen eine Auflösung von 25m für x und y!), das Source-File (!! Ursprungshöhenmodell) und das Ziel-File angeben!!
! Achten Sie darauf, keine Umlaute in Pfad und Datei zu verwenden. Bei Leerzeichen Pfad und Datei in „“ setzen !
Tipp: Schreiben Sie in die letzte Zeile „PAUSE“. Dies verhindert, dass die Kommandozeile sofort wieder verschwindet. So kann man die Fehler bzw. die Fertigstellung erkennen.
6c. Wie heißt der vollständige gdalwarp-Befehl, das uns für die gleichen Koordinaten wie das Polygon das umprojizierte Raster in x, y 25m Auflösung generiert? (Aus dem Original-Höhenmodell!)
