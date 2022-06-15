
# Biketirol Applikation (Teil 1) - Visualisieren eines GPX-Tracks mit leaflet-gpx

## Repo erstellen

1. erstellt ein neues Repo `biketirol` in eurem Account auf github.com
2. cloned das neue Repo lokal und packt dort das von uns vorbereitete Template [biketirol.zip](https://webmapping.github.io/templates/biketirol.zip) aus
3. löst alle TODO Anweisungen in `index.html` entsprechen auf
4. speichert den GPX Track eurer Etappe im Format **.gpx** lokal im Unterverzeichnis `data/`
5. committed und pushed alle Schritte
6. macht das Repo `biketirol` online verfügbar

## Laden von GPX in Leaflet

### GPX Plugin

* von CDNjs einbinden <https://cdnjs.com/libraries/leaflet-gpx>
* copy/paste script tag in `index.html`

### Plugin Aufruf

* GPX track auf Etappen-Nummer umbenannen
* Plugin Aufruf (Achtung auf das **new**)

```javascript
let gpxTrack = new L.GPX("./data/07.gpx", {
    async: true,
}).addTo(overlays.gpx);
```
Anzeige in der Karte funktioniert

### Marker stylen

* JavaScript console zeigt Fehlermeldungen für nicht vorhandene Marker. Lösung

    ```javascript
    let gpxTrack = new L.GPX("../data/07.gpx", {
        async: true,
        marker_options: {
            startIconUrl: 'icons/start.png',
            endIconUrl: 'icons/finish.png',
            shadowUrl: null,
            iconSize: [32, 37],
            iconAnchor: [16, 37],
            popupAnchor: [0, -37]
        },
    }).addTo(overlays.gpx);
    ```

* Track-Linie mit `polyline_options` strichliert, schwarz zeichnen

    ```javascript
    polyline_options: {
        color: "black",
        dashArray: [2, 5]
    }
    ```

* auf den gezeichneten Track blicken

    ```javascript
    // nach dem Laden ...
    gpxTrack.on("loaded", function(evt) {
        // Ausschnitt auf den GPX-Track setzen
        map.fitBounds(evt.target.getBounds());
    });
    ```

* GPX-Track Eigenschaften als Popup anzeigen

    ```javascript
    // GPX-Track Eigenschaften als Popup anzeigen
    evt.target.bindPopup(`
        <h3>${evt.target.get_name()}</h3>
        <ul>
            <li>Streckenlänge: ${Math.round(evt.target.get_distance()/1000)} km</li>
            <li>tiefster Punkt: ${evt.target.get_elevation_min()} m</li>
            <li>höchster Punkt: ${evt.target.get_elevation_max()} m</li>
            <li>Höhenmeter bergauf: ${Math.round(evt.target.get_elevation_gain())} m</li>
            <li>Höhenmeter bergab: ${Math.round(evt.target.get_elevation_loss())} m</li>
        </ul>
    `);
    ```

Achtung die Werte für bergauf/bergab sind weit fern der Realität
