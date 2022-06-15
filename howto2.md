# Biketirol Applikation (Teil 2) - Höhenprofil aus dem GPX-Track mit leaflet-elevation erstellen

### Leaflet Plugin elevation

* Das verlinkte Plugin auf der Leaflet Plugin-Webseite funktioniert nicht mehr richtig, ein [Fork von Raruto](https://github.com/Raruto/leaflet-elevation) ist besser

* Laden von der GitHub Page (unpkg.com nicht cdnjs.com)

    ```javascript
    <!-- leaflet-elevation -->
    <link rel="stylesheet" href="https://unpkg.com/@raruto/leaflet-elevation/dist/leaflet-elevation.css" />
    <script src="https://unpkg.com/@raruto/leaflet-elevation/dist/leaflet-elevation.js"></script>
    ```

* Einbau in `main.js` (options für elevation aktuell noch leer)

    ```javascript
    let controlElevation = L.control.elevation({}).addTo(map);
    gpxTrack.on("addline", function(evt) {
        // Höhenprofil aktualisieren
        controlElevation.addData(evt.line);
    });
    ```

### Elevation styling

* Javascript Code in `main.js`

    ```javascript
    let controlElevation = L.control.elevation({
        time: false,
        detached: true,
        theme: 'bike-tirol',
        elevationDiv: "#profile",
        height: 300,
    }).addTo(map);
    ```

    * Keine Anzeige der Zeit (`time: false`)
    * eigenen DIV für profile verwenden (`detached: true`)
    * styling mit CSS (eigenes theme `bike-tirol`) (`theme: 'bike-tirol'`)
    * ID des eigenen DIV (`elevationDiv: "#profile"`)
    * DIV Höhe im Plugin einstellen (`height: 300`)

* und die CSS Änderungen in `main.css`

    ```css
    #profile {
        border: 1px solid gray;
        margin-top: 0.3em;
        padding: 0.5em;
    }
    .bike-tirol {
        /* --ele-area: seagreen; */
        --ele-area: #2e8b57a1;
        --ele-bg: white;
    }
    ```
