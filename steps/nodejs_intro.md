## Einführung in Node.js

Node.js ist eine **Open-Source-Laufzeitumgebung**, die es Ihnen ermöglicht, **JavaScript-Code außerhalb eines Webbrowsers** auszuführen. Ursprünglich wurde JavaScript für die Ausführung im Browser entwickelt, um interaktive Webseiten zu erstellen. Mit Node.js hat sich dies geändert: Sie können JavaScript nun auch auf Servern, für Kommandozeilen-Tools und sogar für Desktop-Anwendungen verwenden.

### Was ist Node.js genau?

Stellen Sie sich Node.js als eine Brücke vor, die JavaScript von der Browser-Umgebung in die Welt des Servers und darüber hinaus führt. Es basiert auf der **V8 JavaScript-Engine von Google Chrome**, derselben leistungsstarken Engine, die auch in Ihrem Chrome-Browser JavaScript-Code blitzschnell ausführt. Dadurch ist Node.js extrem performant.

### Warum ist Node.js so beliebt?

Die Popularität von Node.js beruht auf mehreren Schlüsselfaktoren:

1. **"JavaScript Everywhere" (JavaScript überall):** Entwickler können dieselbe Sprache (JavaScript) sowohl für das Frontend (Browser) als auch für das Backend (Server) verwenden. Das vereinfacht den Entwicklungsprozess erheblich, da kein Sprachwechsel nötig ist und Code wiederverwendet werden kann.
2. **Ereignisgesteuerte, nicht-blockierende E/A (I/O):** Dies ist das Herzstück von Node.js. Im Gegensatz zu traditionellen Servern, die für jede eingehende Anfrage einen neuen Thread starten und blockieren, bis die Aufgabe erledigt ist, arbeitet Node.js mit einem **Single-Threaded Event Loop**. Das bedeutet, dass es Anfragen asynchron verarbeitet. Wenn eine Anfrage eine zeitaufwendige Operation (z.B. Datenbankzugriff oder Dateisystem-E/A) erfordert, wird diese Operation im Hintergrund ausgeführt, während Node.js weiterhin andere Anfragen entgegennimmt und bearbeitet. Sobald die zeitaufwendige Operation abgeschlossen ist, wird ein "Ereignis" ausgelöst, und die entsprechende Callback-Funktion wird ausgeführt. Dies führt zu einer extrem hohen Skalierbarkeit und Effizienz, besonders bei I/O-lastigen Anwendungen.
3. **Schnell und Skalierbar:** Dank der V8-Engine und des nicht-blockierenden I/O-Modells kann Node.js eine große Anzahl gleichzeitiger Verbindungen mit geringem Overhead verwalten.
4. **Umfangreiches Ökosystem (npm):** Node.js verfügt über den **Node Package Manager (npm)**, das größte Software-Repository der Welt. npm bietet Zugang zu Tausenden von wiederverwendbaren Modulen und Bibliotheken, die die Entwicklung beschleunigen und vereinfachen.

### Anwendungsfälle von Node.js

Node.js wird in einer Vielzahl von Bereichen eingesetzt:

- **Webserver und APIs:** Erstellung von hochperformanten HTTP-Servern und RESTful APIs.
- **Echtzeitanwendungen:** Chat-Anwendungen, Online-Spiele, Kollaborationstools (z.B. Google Docs-ähnliche Anwendungen), da es schnelle bidirektionale Kommunikation ermöglicht.
- **Microservices:** Die Leichtigkeit und Effizienz von Node.js machen es ideal für den Aufbau kleiner, unabhängiger Dienste.
- **Daten-Streaming:** Effiziente Verarbeitung großer Datenmengen, z.B. bei Video-Streaming-Diensten (Netflix nutzt Node.js).
- **Kommandozeilen-Tools (CLI-Tools):** Automatisierung von Aufgaben und Erstellung von Skripten.
- **Backend für mobile Apps:** Serverseitige Logik für iOS- und Android-Anwendungen.
- **IoT (Internet of Things):** Steuerung und Datenverarbeitung für IoT-Geräte.

## Node.js Tutorial: Ihre erste Blog-API erstellen (Schritt für Schritt)

In diesem Tutorial erstellen wir eine **einfache Blog-API** mit Node.js. Diese API ermöglicht es Ihnen, Blogbeiträge abzurufen und neue Beiträge hinzuzufügen. Wir bauen die API schrittweise auf, testen jeden Teil und erklären die Konzepte, bevor wir zum Thema Asynchronität übergehen.

### Schritt 1: Node.js installieren

Zuerst müssen Sie Node.js auf Ihrem System installieren.

1. **Besuchen Sie die offizielle Node.js-Website:** Gehen Sie zu https://nodejs.org/de/download/.
2. **Laden Sie den Installer herunter:** Wählen Sie die **"LTS" (Long Term Support)** Version, da diese stabiler und für die meisten Anwendungsfälle empfohlen wird. Es gibt Installer für Windows, macOS und Linux.
3. **Führen Sie den Installer aus:** Folgen Sie den Anweisungen des Installers. **npm (Node Package Manager)** wird automatisch mitinstalliert.
4. **Überprüfen Sie die Installation:** Öffnen Sie Ihr Terminal (oder die Eingabeaufforderung unter Windows) und geben Sie die folgenden Befehle ein:
    
    ```bash
    node -v
    npm -v
    
    ```
    
    Sie sollten die installierten Versionen von Node.js und npm sehen.
    

### Schritt 2: Projekt initialisieren und ES-Module aktivieren

Erstellen Sie einen neuen Ordner für Ihr Projekt und initialisieren Sie ein Node.js-Projekt darin. Wir werden auch **ES-Module** aktivieren, um modernere `import`-Statements verwenden zu können.

1. **Erstellen Sie einen Projektordner:** Öffnen Sie Ihr Terminal und geben Sie ein:
    
    ```bash
    mkdir meine-blog-api
    cd meine-blog-api
    
    ```
    
    - **Erklärung:** `mkdir` erstellt ein neues Verzeichnis (Ordner), und `cd` wechselt in dieses Verzeichnis.
2. **Initialisieren Sie das Projekt:** Geben Sie im Terminal ein:
    
    ```bash
    npm init -y
    
    ```
    
    - **Erklärung:** Dieser Befehl erstellt eine `package.json`Datei im aktuellen Ordner. Diese Datei ist wie ein Manifest für Ihr Projekt: Sie enthält Metadaten (Name, Version, Beschreibung) und listet alle Abhängigkeiten (andere Bibliotheken, die Ihr Projekt benötigt) auf. Die Option `y` (für "yes") beantwortet alle Fragen automatisch mit Standardwerten, was den Prozess beschleunigt.
3. **ES-Module aktivieren:** Öffnen Sie die neu erstellte Datei `package.json` mit einem Texteditor Ihrer Wahl (z.B. VS Code, Sublime Text, Notepad++).
    
    Suchen Sie die Zeile `"main": "index.js",` und fügen Sie **direkt darunter** die Zeile `"type": "module",` hinzu.
    
    Ihre `package.json` sollte dann etwa so aussehen:
    
    ```json
    {
      "name": "meine-blog-api",
      "version": "1.0.0",
      "description": "",
      "main": "index.js",
      "type": "module",  <-- Fügen Sie diese Zeile hinzu
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "start": "node app.js" <-- Fügen Sie auch diese Zeile hinzu
      },
      "keywords": [],
      "author": "",
      "license": "ISC"
    }
    
    ```
    
    - **Erklärung:**
        - `"type": "module"`: Diese Einstellung teilt Node.js mit, dass Sie in Ihren JavaScript-Dateien die moderne **ES-Modul-Syntax** (`import` und `export`) verwenden möchten, anstelle der älteren CommonJS-Syntax (`require()`). Dies ist der aktuelle Standard in der JavaScript-Welt.
        - `"start": "node app.js"`: Diese Zeile im `scripts`Abschnitt definiert einen benutzerdefinierten Befehl. Anstatt jedes Mal `node app.js` einzugeben, können Sie später einfach `npm start`verwenden, um Ihre Anwendung zu starten.

### Schritt 3: Konfigurationsdatei erstellen

Es ist eine gute Praxis, Konfigurationseinstellungen (wie den Port, auf dem der Server lauscht) in einer separaten Datei zu speichern. Das macht es einfacher, Einstellungen zu ändern, ohne den Hauptcode anzupassen.

1. **Erstellen Sie die Datei `config.json`:** Erstellen Sie im Projektordner eine neue Datei namens `config.json`.
2. **Fügen Sie den folgenden Inhalt ein:**
    
    ```json
    {
      "port": 3000,
      "hostname": "127.0.0.1"
    }
    
    ```
    
    - **Erklärung:** Dies ist eine einfache **JSON-Datei** (JavaScript Object Notation). Sie enthält zwei Schlüssel-Wert-Paare:
        - `"port": 3000`: Der Port, auf dem unser Server lauschen wird. Port 3000 ist ein gängiger Standard für Entwicklungszwecke.
        - `"hostname": "127.0.0.1"`: Dies ist die **Loopback-Adresse**, auch bekannt als `localhost`. Sie verweist immer auf Ihren eigenen Computer.

### Teil 1: Synchroner Server (Grundlagen)

In diesem ersten Teil konzentrieren wir uns auf die grundlegende Struktur eines Node.js-Servers und die synchrone Verarbeitung. Wir bauen Schritt für Schritt auf, um zu sehen, wie ein Server Anfragen empfängt und Antworten sendet.

### Schritt 4: Der erste "Hallo Welt" Server (`app.js`)

Wir erstellen jetzt die Hauptdatei für unsere API, `app.js`, und lassen sie einen einfachen "Hallo Welt" Text zurückgeben.

1. **Erstellen Sie die Datei `app.js`:** Erstellen Sie im Projektordner eine neue Datei namens `app.js`.
2. **Fügen Sie die erste Zeile hinzu:**
    
    ```jsx
    // app.js - Version 1: Hallo Welt Server
    
    // Importieren Sie das eingebaute 'http'-Modul
    import http from 'http';
    
    ```
    
    - **Erklärung:** Die erste Zeile ist ein **Kommentar**, der den Zweck der Datei angibt. Die zweite Zeile verwendet die **ES-Modul-Syntax** (`import`) um das **`http`**Modul zu importieren. Dieses Modul ist ein **eingebautes Node.js-Modul**, das die notwendigen Funktionen bereitstellt, um einen HTTP-Server zu erstellen und mit HTTP-Anfragen und -Antworten zu arbeiten.
3. **Definieren Sie Hostname und Port:**
    
    ```jsx
    // app.js - Version 1: Hallo Welt Server
    
    import http from 'http';
    
    // Definieren Sie den Hostnamen und den Port, auf dem der Server lauschen soll
    const hostname = '127.0.0.1'; // 'localhost'
    const port = 3000;
    
    ```
    
    - **Erklärung:** Hier deklarieren wir zwei Konstanten, `hostname` und `port`, die festlegen, wo unser Server erreichbar sein wird. Vorerst sind diese Werte **fest codiert** (direkt im Code geschrieben). Später werden wir sie aus der `config.json` laden.
4. **Erstellen Sie den Server:**
    
    ```jsx
    // app.js - Version 1: Hallo Welt Server
    
    import http from 'http';
    
    const hostname = '127.0.0.1';
    const port = 3000;
    
    // Erstellen Sie den HTTP-Server
    // Die Funktion, die hier übergeben wird, ist ein 'Callback'.
    // Sie wird bei JEDER eingehenden Anfrage an unseren Server ausgeführt.
    // 'req' (request): Ein Objekt, das alle Informationen über die eingehende Anfrage enthält (z.B. die angefragte URL, die HTTP-Methode).
    // 'res' (response): Ein Objekt, mit dem wir die Antwort an den Client (z.B. den Webbrowser) senden.
    const server = http.createServer((req, res) => {
        // Loggen Sie die eingehende Anfrage im Terminal
        console.log(`Anfrage erhalten: ${req.method} ${req.url}`);
    
        // Setzen Sie den HTTP-Statuscode (200 = OK) und den Content-Type Header
        // 'Content-Type: text/plain' teilt dem Browser mit, dass die Antwort einfacher Text ist.
        res.writeHead(200, { 'Content-Type': 'text/plain' });
    
        // Senden Sie den Inhalt der Antwort und beenden Sie die Verbindung
        // res.end() sendet den eigentlichen Inhalt der Antwort.
        res.end('Hallo Welt von Ihrem Node.js Server!\n');
    });
    
    ```
    
    - **Erklärung:**
        - `http.createServer()`: Dies ist die zentrale Funktion, um einen HTTP-Server zu erstellen. Sie nimmt eine **Callback-Funktion** als Argument entgegen.
        - `(req, res) => { ... }`: Dies ist die Callback-Funktion, die bei jeder neuen HTTP-Anfrage an unseren Server aufgerufen wird.
        - `console.log(...)`: Gibt eine Nachricht im Terminal aus, wenn eine Anfrage empfangen wird. Dies ist nützlich zum Debuggen.
        - `res.writeHead(200, { 'Content-Type': 'text/plain' })`: Dies ist der erste Schritt, um eine Antwort zu senden.
            - `200`: Der **HTTP-Statuscode**. `200 OK` bedeutet, dass die Anfrage erfolgreich verarbeitet wurde.
            - `{ 'Content-Type': 'text/plain' }`: Dies sind die **HTTP-Header**. Der `Content-Type`Header ist wichtig, da er dem Client (z.B. Ihrem Browser) mitteilt, welche Art von Daten er erwarten soll. Hier ist es einfacher Text.
        - `res.end('Hallo Welt von Ihrem Node.js Server!\n')`: Dies ist der letzte Schritt. Die `end()`Methode sendet den eigentlichen Inhalt der Antwort an den Client und schließt die Verbindung.
5. **Starten Sie den Server:**
    
    ```jsx
    // app.js - Version 1: Hallo Welt Server
    
    import http from 'http';
    
    const hostname = '127.0.0.1';
    const port = 3000;
    
    const server = http.createServer((req, res) => {
        console.log(`Anfrage erhalten: ${req.method} ${req.url}`);
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('Hallo Welt von Ihrem Node.js Server!\n');
    });
    
    // Starten Sie den Server und lassen Sie ihn auf eingehende Anfragen lauschen
    // Die Funktion im dritten Argument wird ausgeführt, sobald der Server erfolgreich gestartet ist.
    server.listen(port, hostname, () => {
        console.log(`Server läuft unter http://${hostname}:${port}/`);
        console.log('Öffnen Sie http://localhost:3000/ in Ihrem Browser, um ihn zu testen.');
    });
    
    ```
    
    - **Erklärung:**
        - `server.listen(port, hostname, () => { ... })`: Diese Methode startet den Server. Er beginnt, auf dem angegebenen `port` und `hostname` auf eingehende HTTP-Anfragen zu warten.
        - Die Callback-Funktion (`() => { ... }`) wird ausgeführt, sobald der Server erfolgreich gestartet ist und bereit ist, Anfragen zu empfangen. Hier geben wir eine Bestätigungsnachricht im Terminal aus.

**Code Check 1: Ihr `app.js` sollte jetzt so aussehen**

```jsx
// app.js - Version 1: Hallo Welt Server

import http from 'http';

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
    console.log(`Anfrage erhalten: ${req.method} ${req.url}`);
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Hallo Welt von Ihrem Node.js Server!\n');
});

server.listen(port, hostname, () => {
    console.log(`Server läuft unter http://${hostname}:${port}/`);
    console.log('Öffnen Sie http://localhost:3000/ in Ihrem Browser, um ihn zu testen.');
});

```

1. **Testen Sie den Server:**
    - Öffnen Sie Ihr Terminal im Projektordner (`meine-blog-api`).
    - Starten Sie den Server mit: `npm start`
    - Öffnen Sie Ihren Webbrowser und navigieren Sie zu `http://localhost:3000/`. Sie sollten den Text "Hallo Welt von Ihrem Node.js Server!" sehen.
    - Beobachten Sie das Terminal: Sie sollten die geloggte Anfrage sehen, z.B. `Anfrage erhalten: GET /`.
    - Um den Server zu stoppen, drücken Sie `Strg + C` (oder `Ctrl + C`) im Terminal.

### Schritt 5: Konfiguration aus Datei laden

Jetzt machen wir unseren Server flexibler, indem wir den Port und Hostnamen aus der `config.json`-Datei lesen.

1. **Importieren Sie zusätzliche Module:** Fügen Sie diese Zeilen **direkt unter** `import http from 'http';` hinzu:
    
    ```jsx
    // app.js - Version 2: Konfiguration aus Datei laden
    
    import http from 'http';
    import { readFileSync } from 'fs'; // Für synchrones Lesen von Dateien
    import path from 'path';           // Für die Arbeit mit Dateipfaden
    import { fileURLToPath } from 'url'; // Hilft bei der Umwandlung von Modul-URLs in Dateipfade
    
    ```
    
    - **Erklärung:**
        - `fs` (File System): Ein weiteres eingebautes Node.js-Modul, das Funktionen zum Interagieren mit dem Dateisystem bereitstellt (Dateien lesen, schreiben, etc.). Wir importieren hier speziell `readFileSync`, die **synchrone** Version zum Lesen einer Datei.
        - `path`: Ein eingebautes Modul, das Hilfsfunktionen für die Arbeit mit Dateipfaden bereitstellt. Es hilft, Pfade plattformübergreifend korrekt zu behandeln (z.B. `/` unter Linux/macOS vs. `\` unter Windows).
        - `url`: Das `fileURLToPath`Modul ist notwendig, weil in **ES-Modulen** die globalen Variablen `__filename` und `__dirname` (die den aktuellen Dateipfad und Verzeichnisnamen enthalten) nicht mehr direkt verfügbar sind. Wir müssen sie manuell ableiten.
2. **Definieren Sie `__filename` und `__dirname`:** Fügen Sie diese Zeilen **direkt unter** den `import`Statements hinzu:
    
    ```jsx
    // app.js - Version 2: Konfiguration aus Datei laden
    
    import http from 'http';
    import { readFileSync } from 'fs';
    import path from 'path';
    import { fileURLToPath } from 'url';
    
    // In ES-Modulen sind __filename und __dirname nicht global verfügbar.
    // Wir definieren sie manuell für die Pfadbehandlung.
    const __filename = fileURLToPath(import.meta.url);
    const __dirname = path.dirname(__filename);
    
    ```
    
    - **Erklärung:**
        - `import.meta.url`: In ES-Modulen enthält dies die URL der aktuellen Moduldatei (z.B. `file:///path/to/your/app.js`).
        - `fileURLToPath(import.meta.url)`: Wandelt diese URL in einen lokalen Dateipfad um.
        - `path.dirname(__filename)`: Extrahiert den Verzeichnisnamen aus dem vollständigen Dateipfad. Jetzt haben wir `__dirname`, das den Pfad zum aktuellen Ordner (`meine-blog-api`) enthält.
3. **Laden Sie die Konfiguration:** Ersetzen Sie die Zeilen `const hostname = '127.0.0.1';` und `const port = 3000;` durch diese neuen Zeilen:
    
    ```jsx
    // app.js - Version 2: Konfiguration aus Datei laden
    
    import http from 'http';
    import { readFileSync } from 'fs';
    import path from 'path';
    import { fileURLToPath } from 'url';
    
    const __filename = fileURLToPath(import.meta.url);
    const __dirname = path.dirname(__filename);
    
    // Laden Sie die Konfiguration synchron aus der config.json Datei
    // path.join(__dirname, 'config.json') erstellt den korrekten Pfad zur Datei.
    // readFileSync liest den Inhalt der Datei. 'utf8' gibt an, dass es Text ist.
    // JSON.parse wandelt den JSON-String in ein JavaScript-Objekt um.
    const configPath = path.join(__dirname, 'config.json');
    const config = JSON.parse(readFileSync(configPath, 'utf8'));
    
    // Extrahieren Sie Port und Hostname aus dem Konfigurationsobjekt
    const { port, hostname } = config;
    
    ```
    
    - **Erklärung:**
        - `path.join(__dirname, 'config.json')`: Erstellt einen vollständigen und korrekten Pfad zur `config.json`Datei, basierend auf dem aktuellen Verzeichnis.
        - `readFileSync(configPath, 'utf8')`: Liest den Inhalt der Datei `config.json` synchron. Das bedeutet, die Ausführung des Codes wird an dieser Stelle **angehalten**, bis die gesamte Datei gelesen wurde. Für Konfigurationsdateien, die nur einmal beim Start der Anwendung gelesen werden, ist dies akzeptabel.
        - `JSON.parse(...)`: Der Inhalt der `config.json` ist ein JSON-String. `JSON.parse()` wandelt diesen String in ein JavaScript-Objekt um, auf das wir dann zugreifen können.
        - `const { port, hostname } = config;`: Dies ist **Objekt-Destrukturierung**. Es ist eine Kurzschreibweise, um die Eigenschaften `port` und `hostname` aus dem `config`Objekt zu extrahieren und sie in separate Konstanten gleichen Namens zu speichern.
4. **Aktualisieren Sie `server.listen` (keine Codeänderung, nur Erklärung):** Die Zeile `server.listen(port, hostname, () => { ... });` verwendet nun die dynamisch geladenen `port`und `hostname`Variablen.

**Code Check 2: Ihr `app.js` sollte jetzt so aussehen**

```jsx
// app.js - Version 2: Konfiguration aus Datei laden

import http from 'http';
import { readFileSync } from 'fs';
import path from 'path';
import { fileURLToPath } from 'url';

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

const configPath = path.join(__dirname, 'config.json');
const config = JSON.parse(readFileSync(configPath, 'utf8'));

const { port, hostname } = config;

const server = http.createServer((req, res) => {
    console.log(`Anfrage erhalten: ${req.method} ${req.url}`);
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Hallo Welt von Ihrem Node.js Server!\n');
});

server.listen(port, hostname, () => {
    console.log(`Server läuft unter http://${hostname}:${port}/`);
    console.log('Öffnen Sie http://localhost:3000/ in Ihrem Browser, um ihn zu testen.');
});

```

1. **Testen Sie den Server:**
    - Stoppen Sie den Server (`Strg + C`), falls er noch läuft.
    - Starten Sie den Server mit: `npm start`
    - Öffnen Sie `http://localhost:3000/`. Es sollte immer noch "Hallo Welt" angezeigt werden.
    - **Ändern Sie den Port in Ihrer `config.json`** (z.B. von `3000` auf `4000`). Speichern Sie die Datei.
    - Stoppen Sie den Server (`Strg + C`) und starten Sie ihn neu.
    - Versuchen Sie `http://localhost:3000/` (sollte jetzt nicht mehr funktionieren) und `http://localhost:4000/` (sollte jetzt funktionieren).
    - **Erklärung:** Sie haben erfolgreich gelernt, wie man Konfigurationen aus einer externen Datei lädt, was Ihre Anwendungen flexibler macht.

### Schritt 6: Blogbeiträge als "In-Memory-Datenbank" und GET-Endpunkt für alle Beiträge

Jetzt fügen wir unsere Blogbeiträge hinzu und erstellen den ersten API-Endpunkt, um sie abzurufen.

1. **Fügen Sie die In-Memory-Datenbank hinzu:** Fügen Sie diese Zeilen **direkt unter** der Deklaration von `port`und `hostname` hinzu:
    
    ```jsx
    // app.js - Version 3: Blogbeiträge und GET /posts
    
    // ... (vorheriger Code) ...
    
    const { port, hostname } = config;
    
    // Eine einfache In-Memory-Datenbank für Blogbeiträge
    // In einer echten Anwendung würden Sie hier eine persistente Datenbank wie MongoDB oder PostgreSQL verwenden.
    // Für dieses Tutorial speichern wir die Daten einfach im Arbeitsspeicher.
    let posts = [
        { id: 1, title: 'Mein erster Blogbeitrag', content: 'Das ist der Inhalt meines ersten Beitrags. Willkommen in der Welt von Node.js!', author: 'Alice', date: '2024-07-29' },
        { id: 2, title: 'Node.js Grundlagen verstehen', content: 'Heute lernen wir die Event Loop und asynchrone Programmierung kennen.', author: 'Bob', date: '2024-07-28' }
    ];
    let nextId = 3; // Eine Variable, um neuen Beiträgen eindeutige IDs zu geben
    
    ```
    
    - **Erklärung:**
        - `let posts = [...]`: Wir deklarieren ein Array namens `posts`. Dieses Array wird unsere "Datenbank" sein und JavaScript-Objekte enthalten, die unsere Blogbeiträge repräsentieren. Da es sich um eine `let`Variable handelt, können wir später neue Beiträge hinzufügen.
        - `let nextId = 3;`: Diese Variable hilft uns, jedem neuen Beitrag eine eindeutige ID zuzuweisen.
2. **Fügen Sie CORS-Header und OPTIONS-Handling hinzu:** Fügen Sie diese Zeilen **direkt am Anfang** der Callback-Funktion von `http.createServer` hinzu (also direkt nach `console.log(...)`):
    
    ```jsx
    // app.js - Version 3: Blogbeiträge und GET /posts
    
    // ... (vorheriger Code) ...
    
    const server = http.createServer((req, res) => {
        console.log(`Anfrage erhalten: ${req.method} ${req.url}`);
    
        // CORS-Header setzen: Erlaubt Anfragen von anderen Domänen (wichtig für Frontend-Anwendungen)
        // '*' bedeutet, dass Anfragen von JEDER Domäne erlaubt sind.
        res.setHeader('Access-Control-Allow-Origin', '*');
        res.setHeader('Access-Control-Allow-Methods', 'GET, POST, OPTIONS'); // Erlaubte HTTP-Methoden
        res.setHeader('Access-Control-Allow-Headers', 'Content-Type'); // Erlaubte Header in Anfragen
    
        // Behandeln von OPTIONS-Anfragen (Preflight-Anfragen für CORS)
        // Browser senden OPTIONS-Anfragen, um zu prüfen, ob eine Cross-Origin-Anfrage erlaubt ist.
        if (req.method === 'OPTIONS') {
            res.writeHead(204); // 204 No Content: Erfolgreiche Antwort, aber ohne Inhalt
            res.end();
            return; // Beenden Sie die Funktion hier, da die OPTIONS-Anfrage behandelt wurde
        }
    
        // ... (Rest der Server-Logik) ...
    });
    
    ```
    
    - **Erklärung:**
        - **CORS (Cross-Origin Resource Sharing):** Wenn eine Webseite (z.B. auf `meine-website.com`) versucht, Daten von einer anderen Domäne (z.B. unserer API auf `localhost:3000`) abzurufen, blockieren Browser dies standardmäßig aus Sicherheitsgründen. Die `Access-Control-Allow-Origin`Header teilen dem Browser mit, dass er Anfragen von anderen Domänen zulassen soll. `'*'`erlaubt alle Domänen.
        - **OPTIONS-Anfragen:** Bevor ein Browser eine komplexe HTTP-Anfrage (wie z.B. eine `POST`Anfrage mit einem bestimmten `Content-Type`) an eine andere Domäne sendet, sendet er oft eine sogenannte "Preflight"-`OPTIONS`Anfrage. Diese Anfrage soll prüfen, ob die eigentliche Anfrage vom Server akzeptiert wird. Wir müssen diese `OPTIONS`Anfragen explizit mit einem `204 No Content`Statuscode beantworten.
3. **Fügen Sie den GET /posts-Endpunkt hinzu:** Ersetzen Sie die Zeilen `res.writeHead(200, { 'Content-Type': 'text/plain' }); res.end('Hallo Welt von Ihrem Node.js Server!\n');` durch den folgenden Codeblock:
    
    ```jsx
    // app.js - Version 3: Blogbeiträge und GET /posts
    
    // ... (vorheriger Code) ...
    
    const server = http.createServer((req, res) => {
        console.log(`Anfrage erhalten: ${req.method} ${req.url}`);
    
        res.setHeader('Access-Control-Allow-Origin', '*');
        res.setHeader('Access-Control-Allow-Methods', 'GET, POST, OPTIONS');
        res.setHeader('Access-Control-Allow-Headers', 'Content-Type');
    
        if (req.method === 'OPTIONS') {
            res.writeHead(204);
            res.end();
            return;
        }
    
        // Routing-Logik: Prüfen Sie die angefragte URL und HTTP-Methode
        if (req.url === '/posts' && req.method === 'GET') {
            // Wenn die URL '/posts' ist UND die Methode GET ist
            res.writeHead(200, { 'Content-Type': 'application/json' }); // Setzen Sie den Content-Type auf JSON
            res.end(JSON.stringify(posts)); // Wandeln Sie das 'posts'-Array in einen JSON-String um und senden Sie es
        } else {
            // Wenn keine der obigen Bedingungen zutrifft, senden Sie einen 404 Not Found Fehler
            res.writeHead(404, { 'Content-Type': 'application/json' });
            res.end(JSON.stringify({ message: 'Endpunkt nicht gefunden' }));
        }
    });
    
    // ... (Rest des Codes) ...
    
    ```
    
    - **Erklärung:**
        - `if (req.url === '/posts' && req.method === 'GET')`: Dies ist unsere erste **Routing-Regel**. Wir prüfen, ob die angefragte URL genau `/posts` ist und ob die HTTP-Methode `GET` ist.
        - `res.writeHead(200, { 'Content-Type': 'application/json' })`: Für API-Endpunkte, die Daten zurückgeben, ist es üblich, den `Content-Type` auf `application/json` zu setzen.
        - `JSON.stringify(posts)`: JavaScript-Objekte (wie unser `posts`Array) können nicht direkt als HTTP-Antwort gesendet werden. Sie müssen in einen String umgewandelt werden. `JSON.stringify()` ist die Standardmethode, um JavaScript-Objekte in einen **JSON-String** zu konvertieren.
        - `else { ... }`: Wenn die Anfrage nicht `/posts` (GET) ist, senden wir einen **404 Not Found**Fehler. Der Statuscode `404` bedeutet, dass die angeforderte Ressource auf dem Server nicht gefunden wurde.
4. **Aktualisieren Sie die Server-Startnachricht:** Ändern Sie die `console.log` Nachricht in `server.listen` zu:
    
    ```jsx
    // app.js - Version 3: Blogbeiträge und GET /posts
    
    // ... (vorheriger Code) ...
    
    server.listen(port, hostname, () => {
        console.log(`Server läuft unter http://${hostname}:${port}/`);
        console.log(`Testen Sie: GET http://${hostname}:${port}/posts`); // Neue Testanweisung
    });
    
    ```
    

**Code Check 3: Ihr `app.js` sollte jetzt so aussehen**

```jsx
// app.js - Version 3: Blogbeiträge und GET /posts

import http from 'http';
import { readFileSync } from 'fs';
import path from 'path';
import { fileURLToPath } from 'url';

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

const configPath = path.join(__dirname, 'config.json');
const config = JSON.parse(readFileSync(configPath, 'utf8'));

const { port, hostname } = config;

let posts = [
    { id: 1, title: 'Mein erster Blogbeitrag', content: 'Das ist der Inhalt meines ersten Beitrags. Willkommen in der Welt von Node.js!', author: 'Alice', date: '2024-07-29' },
    { id: 2, title: 'Node.js Grundlagen verstehen', content: 'Heute lernen wir die Event Loop und asynchrone Programmierung kennen.', author: 'Bob', date: '2024-07-28' }
];
let nextId = 3;

const server = http.createServer((req, res) => {
    console.log(`Anfrage erhalten: ${req.method} ${req.url}`);

    res.setHeader('Access-Control-Allow-Origin', '*');
    res.setHeader('Access-Control-Allow-Methods', 'GET, POST, OPTIONS');
    res.setHeader('Access-Control-Allow-Headers', 'Content-Type');

    if (req.method === 'OPTIONS') {
        res.writeHead(204);
        res.end();
        return;
    }

    if (req.url === '/posts' && req.method === 'GET') {
        res.writeHead(200, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify(posts));
    } else {
        res.writeHead(404, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify({ message: 'Endpunkt nicht gefunden' }));
    }
});

server.listen(port, hostname, () => {
    console.log(`Server läuft unter http://${hostname}:${port}/`);
    console.log(`Testen Sie: GET http://${hostname}:${port}/posts`);
});

```

1. **Testen Sie den Server:**
    - Stoppen Sie den Server (`Strg + C`), falls er noch läuft.
    - Starten Sie den Server mit: `npm start`
    - Öffnen Sie `http://localhost:3000/posts` in Ihrem Webbrowser. Sie sollten jetzt eine JSON-Antwort mit Ihren Blogbeiträgen sehen, etwa so: `[{"id":1,"title":"Mein erster Blogbeitrag",...},{"id":2,"title":"Node.js Grundlagen verstehen",...}]`.
    - Versuchen Sie, `http://localhost:3000/` zu öffnen. Sie sollten die 404-Fehlermeldung sehen.
    - **Erklärung:** Sie haben erfolgreich einen API-Endpunkt erstellt, der eine Liste von Daten zurückgibt.

### Schritt 7: GET-Endpunkt für einzelnen Blogbeitrag

Als Nächstes fügen wir die Möglichkeit hinzu, einen einzelnen Blogbeitrag über seine ID abzurufen (z.B. `/posts/1`).

1. **Fügen Sie den GET /posts/:id-Endpunkt hinzu:** Fügen Sie diesen `else if`Block **direkt nach** dem `if (req.url === '/posts' && req.method === 'GET')` Block hinzu:
    
    ```jsx
    // app.js - Version 4: GET /posts/:id
    
    // ... (vorheriger Code) ...
    
    if (req.url === '/posts' && req.method === 'GET') {
        res.writeHead(200, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify(posts));
    } else if (req.url.match(/^\/posts\/(\d+)$/) && req.method === 'GET') {
        // Wenn die URL dem Muster '/posts/ZAHL' entspricht und die Methode GET ist
        // req.url.match(/^\/posts\/(\d+)$/) ist ein regulärer Ausdruck:
        // ^/posts/ : Beginnt mit "/posts/"
        // (\d+) : Fängt eine oder mehrere Ziffern ein (die ID)
        // $ : Endet hier
        const idString = req.url.split('/')[2]; // Teilt die URL bei '/' und nimmt das dritte Element (die ID als String)
        const id = parseInt(idString); // Wandelt den ID-String in eine Ganzzahl um
    
        // Suchen des Beitrags im 'posts'-Array nach der ID
        // .find() ist eine Array-Methode, die das erste Element zurückgibt, das die Bedingung erfüllt.
        const post = posts.find(p => p.id === id);
    
        if (post) {
            // Wenn ein Beitrag gefunden wurde
            res.writeHead(200, { 'Content-Type': 'application/json' });
            res.end(JSON.stringify(post)); // Senden Sie den gefundenen Beitrag als JSON
        } else {
            // Wenn kein Beitrag mit der gegebenen ID gefunden wurde
            res.writeHead(404, { 'Content-Type': 'application/json' }); // 404 Not Found Statuscode
            res.end(JSON.stringify({ message: 'Blogbeitrag nicht gefunden' }));
        }
    } else {
        res.writeHead(404, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify({ message: 'Endpunkt nicht gefunden' }));
    }
    
    // ... (Rest des Codes) ...
    
    ```
    
    - **Erklärung:**
        - `req.url.match(/^\/posts\/(\d+)$/)`: Hier verwenden wir einen **regulären Ausdruck** (`RegEx`), um zu prüfen, ob die URL dem Muster `/posts/GEFOLGT_VON_ZAHLEN` entspricht. Die Klammern um `(\d+)` sind eine **Fanggruppe**, die die tatsächlichen Zahlen (die ID) extrahiert.
        - `req.url.split('/')[2]`: Dies ist eine einfachere Methode, um die ID zu extrahieren. `split('/')`teilt die URL in ein Array von Strings auf (z.B. `['', 'posts', '1']` für `/posts/1`). Das Element an Index `2` ist dann die ID.
        - `parseInt(idString)`: Wandelt den extrahierten String in eine ganze Zahl um, da IDs normalerweise Zahlen sind.
        - `posts.find(p => p.id === id)`: Die Array-Methode `find()` durchsucht das `posts`Array und gibt das erste Objekt zurück, bei dem die Bedingung (`p.id === id`) wahr ist. Wenn kein passendes Objekt gefunden wird, gibt `find()` `undefined` zurück.
        - **Fehlerbehandlung:** Wenn `post` `undefined` ist (d.h., kein Beitrag gefunden wurde), senden wir einen **HTTP-Statuscode 404 (Not Found)** zurück, der anzeigt, dass die angeforderte Ressource nicht existiert.
2. **Aktualisieren Sie die Server-Startnachricht:** Fügen Sie eine neue Testanweisung in `server.listen` hinzu:
    
    ```jsx
    // app.js - Version 4: GET /posts/:id
    
    // ... (vorheriger Code) ...
    
    server.listen(port, hostname, () => {
        console.log(`Server läuft unter http://${hostname}:${port}/`);
        console.log(`Testen Sie: GET http://${hostname}:${port}/posts`);
        console.log(`Testen Sie: GET http://${hostname}:${port}/posts/1`); // Neue Testanweisung
        console.log(`Testen Sie: GET http://${hostname}:${port}/posts/99 (für 404 Fehler)`); // Neue Testanweisung
    });
    
    ```
    

**Code Check 4: Ihr `app.js` sollte jetzt so aussehen**

```jsx
// app.js - Version 4: GET /posts/:id

import http from 'http';
import { readFileSync } from 'fs';
import path from 'path';
import { fileURLToPath } from 'url';

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

const configPath = path.join(__dirname, 'config.json');
const config = JSON.parse(readFileSync(configPath, 'utf8'));

const { port, hostname } = config;

let posts = [
    { id: 1, title: 'Mein erster Blogbeitrag', content: 'Das ist der Inhalt meines ersten Beitrags. Willkommen in der Welt von Node.js!', author: 'Alice', date: '2024-07-29' },
    { id: 2, title: 'Node.js Grundlagen verstehen', content: 'Heute lernen wir die Event Loop und asynchrone Programmierung kennen.', author: 'Bob', date: '2024-07-28' }
];
let nextId = 3;

const server = http.createServer((req, res) => {
    console.log(`Anfrage erhalten: ${req.method} ${req.url}`);

    res.setHeader('Access-Control-Allow-Origin', '*');
    res.setHeader('Access-Control-Allow-Methods', 'GET, POST, OPTIONS');
    res.setHeader('Access-Control-Allow-Headers', 'Content-Type');

    if (req.method === 'OPTIONS') {
        res.writeHead(204);
        res.end();
        return;
    }

    if (req.url === '/posts' && req.method === 'GET') {
        res.writeHead(200, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify(posts));
    } else if (req.url.match(/^\/posts\/(\d+)$/) && req.method === 'GET') {
        const id = parseInt(req.url.split('/')[2]);
        const post = posts.find(p => p.id === id);

        if (post) {
            res.writeHead(200, { 'Content-Type': 'application/json' });
            res.end(JSON.stringify(post));
        } else {
            res.writeHead(404, { 'Content-Type': 'application/json' });
            res.end(JSON.stringify({ message: 'Blogbeitrag nicht gefunden' }));
        }
    } else {
        res.writeHead(404, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify({ message: 'Endpunkt nicht gefunden' }));
    }
});

server.listen(port, hostname, () => {
    console.log(`Server läuft unter http://${hostname}:${port}/`);
    console.log(`Testen Sie: GET http://${hostname}:${port}/posts`);
    console.log(`Testen Sie: GET http://${hostname}:${port}/posts/1`);
    console.log(`Testen Sie: GET http://${hostname}:${port}/posts/99 (für 404 Fehler)`);
});

```

1. **Testen Sie den Server:**
    - Stoppen Sie den Server (`Strg + C`), falls er noch läuft.
    - Starten Sie den Server mit: `npm start`
    - Öffnen Sie `http://localhost:3000/posts/1` in Ihrem Browser. Sie sollten den ersten Blogbeitrag als JSON sehen.
    - Öffnen Sie `http://localhost:3000/posts/2`. Sie sollten den zweiten Blogbeitrag sehen.
    - Öffnen Sie `http://localhost:3000/posts/99`. Sie sollten eine 404-Fehlermeldung erhalten: `{"message":"Blogbeitrag nicht gefunden"}`.
    - **Erklärung:** Sie haben erfolgreich einen API-Endpunkt erstellt, der einzelne Ressourcen basierend auf ihrer ID abrufen kann.

### Schritt 8: Einfacher POST-Endpunkt (ohne Body-Parsing)

Jetzt fügen wir einen Endpunkt hinzu, um neue Blogbeiträge zu erstellen. Zunächst senden wir nur eine statische Bestätigung, da das Parsen des Anfrage-Bodys eine asynchrone Operation ist, die wir später behandeln werden.

1. **Fügen Sie den POST /posts-Endpunkt hinzu:** Fügen Sie diesen `else if`Block **direkt nach** dem `else if (req.url.match(/^\/posts\/(\d+)$/) && req.method === 'GET')` Block hinzu:
    
    ```jsx
    // app.js - Version 5: Einfacher POST /posts (ohne Body-Parsing)
    
    // ... (vorheriger Code) ...
    
    } else if (req.url === '/posts' && req.method === 'POST') {
        // Wenn die URL '/posts' ist UND die Methode POST ist
        // Hier simulieren wir vorerst nur den Empfang eines neuen Beitrags.
        // Das Sammeln und Parsen des tatsächlichen Request-Bodys ist eine asynchrone Operation,
        // die wir im nächsten Teil des Tutorials behandeln werden.
        res.writeHead(201, { 'Content-Type': 'application/json' }); // Statuscode 201 Created
        res.end(JSON.stringify({ message: 'Neuer Blogbeitrag empfangen (Body wird noch nicht verarbeitet)' }));
    } else {
        res.writeHead(404, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify({ message: 'Endpunkt nicht gefunden' }));
    }
    
    // ... (Rest des Codes) ...
    
    ```
    
    - **Erklärung:**
        - `else if (req.url === '/posts' && req.method === 'POST')`: Dies ist die Routing-Regel für das Erstellen neuer Ressourcen. `POST` ist die Standard-HTTP-Methode zum Senden von Daten an den Server, um eine neue Ressource zu erstellen.
        - `res.writeHead(201, { 'Content-Type': 'application/json' })`: Der **HTTP-Statuscode 201 (Created)** ist die korrekte Antwort, wenn eine Ressource erfolgreich erstellt wurde.
        - Wir senden eine einfache JSON-Nachricht zurück, um zu bestätigen, dass die Anfrage empfangen wurde. Der wichtige Hinweis hier ist, dass wir den **Body der Anfrage noch nicht verarbeiten**. Das liegt daran, dass der Request-Body in Node.js als **Stream** empfangen wird, und die Verarbeitung von Streams ist eine asynchrone Operation.
2. **Aktualisieren Sie die Server-Startnachricht:** Fügen Sie eine neue Testanweisung in `server.listen` hinzu:
    
    ```jsx
    // app.js - Version 5: Einfacher POST /posts (ohne Body-Parsing)
    
    // ... (vorheriger Code) ...
    
    server.listen(port, hostname, () => {
        console.log(`Server läuft unter http://${hostname}:${port}/`);
        console.log(`Testen Sie: GET http://${hostname}:${port}/posts`);
        console.log(`Testen Sie: GET http://${hostname}:${port}/posts/1`);
        console.log(`Testen Sie: GET http://${hostname}:${port}/posts/99 (für 404 Fehler)`);
        console.log(`Testen Sie: POST http://${hostname}:${port}/posts (mit curl oder Postman)`); // Neue Testanweisung
    });
    
    ```
    

**Code Check 5: Ihr `app.js` sollte jetzt so aussehen**

```jsx
// app.js - Version 5: Einfacher POST /posts (ohne Body-Parsing)

import http from 'http';
import { readFileSync } from 'fs';
import path from 'path';
import { fileURLToPath } from 'url';

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

const configPath = path.join(__dirname, 'config.json');
const config = JSON.parse(readFileSync(configPath, 'utf8'));

const { port, hostname } = config;

let posts = [
    { id: 1, title: 'Mein erster Blogbeitrag', content: 'Das ist der Inhalt meines ersten Beitrags. Willkommen in der Welt von Node.js!', author: 'Alice', date: '2024-07-29' },
    { id: 2, title: 'Node.js Grundlagen verstehen', content: 'Heute lernen wir die Event Loop und asynchrone Programmierung kennen.', author: 'Bob', date: '2024-07-28' }
];
let nextId = 3;

const server = http.createServer((req, res) => {
    console.log(`Anfrage erhalten: ${req.method} ${req.url}`);

    res.setHeader('Access-Control-Allow-Origin', '*');
    res.setHeader('Access-Control-Allow-Methods', 'GET, POST, OPTIONS');
    res.setHeader('Access-Control-Allow-Headers', 'Content-Type');

    if (req.method === 'OPTIONS') {
        res.writeHead(204);
        res.end();
        return;
    }

    if (req.url === '/posts' && req.method === 'GET') {
        res.writeHead(200, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify(posts));
    } else if (req.url.match(/^\/posts\/(\d+)$/) && req.method === 'GET') {
        const id = parseInt(req.url.split('/')[2]);
        const post = posts.find(p => p.id === id);

        if (post) {
            res.writeHead(200, { 'Content-Type': 'application/json' });
            res.end(JSON.stringify(post));
        } else {
            res.writeHead(404, { 'Content-Type': 'application/json' });
            res.end(JSON.stringify({ message: 'Blogbeitrag nicht gefunden' }));
        }
    } else if (req.url === '/posts' && req.method === 'POST') {
        res.writeHead(201, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify({ message: 'Neuer Blogbeitrag empfangen (Body wird noch nicht verarbeitet)' }));
    } else {
        res.writeHead(404, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify({ message: 'Endpunkt nicht gefunden' }));
    }
});

server.listen(port, hostname, () => {
    console.log(`Server läuft unter http://${hostname}:${port}/`);
    console.log(`Testen Sie: GET http://${hostname}:${port}/posts`);
    console.log(`Testen Sie: GET http://${hostname}:${port}/posts/1`);
    console.log(`Testen Sie: GET http://${hostname}:${port}/posts/99 (für 404 Fehler)`);
    console.log(`Testen Sie: POST http://${hostname}:${port}/posts (mit curl oder Postman)`);
});

```

1. **Testen Sie den Server:**
    - Stoppen Sie den Server (`Strg + C`), falls er noch läuft.
    - Starten Sie den Server mit: `npm start`
    - Sie können POST-Anfragen nicht direkt im Browser testen. Oder verwenden Sie `curl` im Terminal:
        
        ```bash
        curl -X POST http://localhost:3000/posts
        
        ```
        
        Sie sollten die JSON-Nachricht `{ "message": "Neuer Blogbeitrag empfangen (Body wird noch nicht verarbeitet)" }` sehen.
        
    - **Erklärung:** Sie haben erfolgreich einen Endpunkt erstellt, der auf POST-Anfragen reagiert. Der nächste Schritt ist, den Inhalt dieser Anfragen zu verarbeiten.

### Teil 2: Asynchrone Konzepte und Refactoring

Node.js glänzt durch seine **asynchrone, nicht-blockierende Natur**. In diesem Teil werden wir verstehen, warum das so wichtig ist und wie wir unseren Code anpassen, um diese Vorteile zu nutzen.

### Einführung in Asynchronität

Stellen Sie sich vor, Sie bestellen in einem Restaurant.

- **Synchron (blockierend):** Sie bestellen Ihr Essen. Der Koch beginnt sofort mit der Zubereitung. Sie müssen am Schalter stehen bleiben und warten, bis Ihr Essen fertig ist, bevor Sie sich setzen oder etwas anderes tun können. Während Sie warten, kann niemand anderes bestellen.
- **Asynchron (nicht-blockierend):** Sie bestellen Ihr Essen. Der Koch nimmt die Bestellung entgegen und sagt Ihnen, dass er Sie ruft, wenn es fertig ist. Sie können sich setzen, etwas trinken oder mit Freunden plaudern, während der Koch Ihr Essen zubereitet. In der Zwischenzeit können andere Kunden bestellen. Wenn Ihr Essen fertig ist, ruft der Koch Ihren Namen.

Genau so funktioniert Node.js. Wenn eine Operation (wie das Lesen einer Datei, eine Datenbankabfrage oder eine Netzwerkanfrage) Zeit braucht, blockiert Node.js nicht und wartet. Stattdessen gibt es die Aufgabe an das Betriebssystem weiter und fährt fort, andere Anfragen zu bearbeiten. Wenn die Aufgabe abgeschlossen ist, wird ein "Ereignis" ausgelöst, und die entsprechende Funktion (ein sogenannter **Callback**) wird ausgeführt.

**Warum ist das wichtig?** In Webanwendungen gibt es viele **I/O-Operationen** (Input/Output), die Zeit kosten:

- Lesen/Schreiben von Dateien auf der Festplatte.
- Abfragen von Datenbanken.
- Kommunikation mit externen APIs.

Wenn diese Operationen synchron wären, würde Ihr Server bei jeder solchen Operation blockieren und könnte keine anderen Anfragen bearbeiten. Das würde die Leistung drastisch reduzieren. Node.js ist darauf ausgelegt, Tausende von gleichzeitigen Verbindungen effizient zu verarbeiten, und das gelingt ihm durch sein asynchrones, nicht-blockierendes I/O-Modell.

**Promises und `async/await`:** Früher wurden asynchrone Operationen hauptsächlich mit **Callbacks** gehandhabt, was oft zu verschachteltem Code führte (bekannt als "Callback-Hölle"). Moderne JavaScript- und Node.js-Entwicklung verwendet **Promises** und die Syntax **`async/await`**, um asynchronen Code viel lesbarer und wartbarer zu machen.

- Ein **Promise** ist ein Objekt, das einen zukünftigen Wert repräsentiert, der entweder erfolgreich sein oder fehlschlagen kann.
- **`async`** macht eine Funktion asynchron, sodass sie Promises zurückgeben kann.
- **`await`** kann nur innerhalb einer `async`Funktion verwendet werden und **pausiert die Ausführung der *aktuellen `async`Funktion***, bis ein Promise erfüllt (erfolgreich abgeschlossen) oder abgelehnt (fehlgeschlagen) ist. Dabei blockiert es aber nicht den gesamten Node.js-Prozess! Der Event Loop kann in der Zwischenzeit andere Aufgaben bearbeiten.

### Schritt 9: Refactoring: Asynchrones Laden der Konfiguration und simulierte Verzögerung

Wir werden nun `readFileSync` durch die asynchrone Version `readFile` ersetzen und eine künstliche Verzögerung in unsere GET-Endpunkte einbauen, um die asynchrone Natur zu demonstrieren.

1. **Ändern Sie den `fs` Import:** Ändern Sie die Zeile `import { readFileSync } from 'fs';` zu:
    
    ```jsx
    // app.js - Version 6: Asynchrones Laden und simulierte Verzögerung
    
    import http from 'http';
    import { readFile } from 'fs/promises'; // Neu: readFile aus 'fs/promises' für asynchrones Lesen
    import path from 'path';
    import { fileURLToPath } from 'url';
    
    // ... (Rest des Codes) ...
    
    ```
    
    - **Erklärung:** Wir importieren jetzt `readFile` aus `fs/promises`. Das `fs/promises`Modul bietet Promise-basierte Versionen der Dateisystemfunktionen, die die Arbeit mit `async/await` erleichtern.
2. **Laden Sie die Konfiguration asynchron:** Ändern Sie die Zeile `const config = JSON.parse(readFileSync(configPath, 'utf8'));` zu:
    
    ```jsx
    // app.js - Version 6: Asynchrones Laden und simulierte Verzögerung
    
    // ... (vorheriger Code) ...
    
    // Laden Sie die Konfiguration asynchron aus der config.json Datei
    // Da wir 'await' auf Top-Level verwenden, muss das Modul als 'type: module' in package.json definiert sein.
    const config = JSON.parse(await readFile(configPath, 'utf8'));
    
    ```
    
    - **Erklärung:** `await readFile(configPath, 'utf8')` liest die Datei jetzt asynchron. Die Ausführung des Moduls wird hier pausiert, bis die Datei gelesen ist, aber der Node.js-Prozess kann in der Zwischenzeit andere Dinge tun (obwohl es beim Start noch keine anderen Dinge gibt).
3. **Fügen Sie eine `delay`Hilfsfunktion hinzu:** Fügen Sie diese Zeile **direkt unter** der Deklaration von `nextId`hinzu:
    
    ```jsx
    // app.js - Version 6: Asynchrones Laden und simulierte Verzögerung
    
    // ... (vorheriger Code) ...
    
    let nextId = 3;
    
    // Eine Hilfsfunktion, die eine Verzögerung simuliert und ein Promise zurückgibt
    const delay = ms => new Promise(resolve => setTimeout(resolve, ms));
    
    ```
    
    - **Erklärung:** Diese kleine Funktion erzeugt ein **Promise**, das nach einer bestimmten Anzahl von Millisekunden (`ms`) aufgelöst wird. Sie ist perfekt, um eine zeitaufwendige Operation zu simulieren, ohne den Prozess zu blockieren.
4. **Machen Sie die Server-Callback-Funktion `async`:** Ändern Sie die Zeile `const server = http.createServer((req, res) => {` zu:
    
    ```jsx
    // app.js - Version 6: Asynchrones Laden und simulierte Verzögerung
    
    // ... (vorheriger Code) ...
    
    // Machen Sie die Callback-Funktion des Servers 'async', um 'await' darin verwenden zu können.
    const server = http.createServer(async (req, res) => {
    
    ```
    
    - **Erklärung:** Indem wir `async` vor die Funktion setzen, teilen wir JavaScript mit, dass diese Funktion asynchrone Operationen enthalten wird und selbst ein Promise zurückgibt. Dies ist notwendig, um das Schlüsselwort `await` innerhalb dieser Funktion verwenden zu können.
5. **Fügen Sie simulierte Verzögerungen in den GET-Endpunkten hinzu:** Fügen Sie `await delay(...)` in den entsprechenden Blöcken hinzu:
    
    ```jsx
    // app.js - Version 6: Asynchrones Laden und simulierte Verzögerung
    
    // ... (vorheriger Code) ...
    
    if (req.url === '/posts' && req.method === 'GET') {
        res.writeHead(200, { 'Content-Type': 'application/json' });
        // Simulieren einer Datenbankabfrage mit einer kleinen Verzögerung (500ms)
        // 'await delay(500)' pausiert die Ausführung DIESER SPEZIFISCHEN Anfrage
        // für 500ms, aber der Server kann in dieser Zeit andere Anfragen bearbeiten!
        await delay(500);
        res.end(JSON.stringify(posts));
    } else if (req.url.match(/^\/posts\/(\d+)$/) && req.method === 'GET') {
        const id = parseInt(req.url.split('/')[2]);
        const post = posts.find(p => p.id === id);
    
        if (post) {
            res.writeHead(200, { 'Content-Type': 'application/json' });
            await delay(300); // Auch hier eine Verzögerung
            res.end(JSON.stringify(post));
        } else {
            res.writeHead(404, { 'Content-Type': 'application/json' });
            res.end(JSON.stringify({ message: 'Blogbeitrag nicht gefunden' }));
        }
    }
    // ... (Rest des Codes) ...
    
    ```
    
    - **Erklärung:** Das Hinzufügen von `await delay(...)` simuliert eine zeitaufwendige Operation, wie sie bei einer echten Datenbankabfrage auftreten würde. Der wichtige Punkt ist: Obwohl die Ausführung für *diese spezielle Anfrage* pausiert, **blockiert der Node.js Event Loop nicht**. Er kann in der Zwischenzeit andere eingehende Anfragen bearbeiten. Dies ist das Kernprinzip der **nicht-blockierenden I/O** in Node.js.

**Code Check 6: Ihr `app.js` sollte jetzt so aussehen**

```jsx
// app.js - Version 6: Asynchrones Laden und simulierte Verzögerung

import http from 'http';
import { readFile } from 'fs/promises'; // Neu: readFile aus 'fs/promises'
import path from 'path';
import { fileURLToPath } from 'url';

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

const configPath = path.join(__dirname, 'config.json');
// Konfiguration asynchron laden
const config = JSON.parse(await readFile(configPath, 'utf8'));
const { port, hostname } = config;

let posts = [
    { id: 1, title: 'Mein erster Blogbeitrag', content: 'Das ist der Inhalt meines ersten Beitrags. Willkommen in der Welt von Node.js!', author: 'Alice', date: '2024-07-29' },
    { id: 2, title: 'Node.js Grundlagen verstehen', content: 'Heute lernen wir die Event Loop und asynchrone Programmierung kennen.', author: 'Bob', date: '2024-07-28' }
];
let nextId = 3;

// Hilfsfunktion, die eine Verzögerung simuliert und ein Promise zurückgibt
const delay = ms => new Promise(resolve => setTimeout(resolve, ms));

// Die Callback-Funktion des Servers ist jetzt 'async'
const server = http.createServer(async (req, res) => {
    console.log(`Anfrage erhalten: ${req.method} ${req.url}`);

    res.setHeader('Access-Control-Allow-Origin', '*');
    res.setHeader('Access-Control-Allow-Methods', 'GET, POST, OPTIONS');
    res.setHeader('Access-Control-Allow-Headers', 'Content-Type');

    if (req.method === 'OPTIONS') {
        res.writeHead(204);
        res.end();
        return;
    }

    if (req.url === '/posts' && req.method === 'GET') {
        res.writeHead(200, { 'Content-Type': 'application/json' });
        await delay(500); // Simulierte Verzögerung
        res.end(JSON.stringify(posts));
    } else if (req.url.match(/^\/posts\/(\d+)$/) && req.method === 'GET') {
        const id = parseInt(req.url.split('/')[2]);
        const post = posts.find(p => p.id === id);

        if (post) {
            res.writeHead(200, { 'Content-Type': 'application/json' });
            await delay(300); // Simulierte Verzögerung
            res.end(JSON.stringify(post));
        } else {
            res.writeHead(404, { 'Content-Type': 'application/json' });
            res.end(JSON.stringify({ message: 'Blogbeitrag nicht gefunden' }));
        }
    } else if (req.url === '/posts' && req.method === 'POST') {
        res.writeHead(201, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify({ message: 'Neuer Blogbeitrag empfangen (Body wird noch nicht verarbeitet)' }));
    } else {
        res.writeHead(404, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify({ message: 'Endpunkt nicht gefunden' }));
    }
});

server.listen(port, hostname, () => {
    console.log(`Server läuft unter http://${hostname}:${port}/`);
    console.log('API-Endpunkte zum Testen:');
    console.log(`GET http://${hostname}:${port}/posts`);
    console.log(`GET http://${hostname}:${port}/posts/1`);
    console.log(`POST http://${hostname}:${port}/posts (mit JSON-Body)`);
});

```

1. **Testen Sie den Server:**
    - Stoppen Sie den Server (`Strg + C`), falls er noch läuft.
    - Starten Sie den Server mit: `npm start`
    - Öffnen Sie `http://localhost:3000/posts` in Ihrem Webbrowser. Sie werden eine leichte Verzögerung bemerken (ca. 0,5 Sekunden), bevor die Daten erscheinen.
    - **Wichtig:** Versuchen Sie, mehrere Tabs mit `http://localhost:3000/posts` gleichzeitig zu öffnen oder schnell hintereinander auf "Aktualisieren" zu klicken. Sie werden sehen, dass der Server nicht blockiert und alle Anfragen bearbeitet, auch wenn jede einzelne Anfrage eine Verzögerung hat. Dies demonstriert die **nicht-blockierende Natur** von Node.js und der Event Loop.
    - **Erklärung:** Sie haben erfolgreich asynchrone Operationen in Ihren Server integriert und die Vorteile der Nicht-Blockierung demonstriert.

### Schritt 10: Refactoring: Asynchrones Parsen des POST-Request-Bodys

Der Body einer POST-Anfrage wird in Node.js als **Stream** empfangen. Das bedeutet, die Daten kommen nicht auf einmal an, sondern in kleinen Stücken (`chunks`). Wir müssen diese Stücke sammeln, bis der gesamte Body empfangen wurde. Dies ist eine klassische asynchrone Operation.

1. **Fügen Sie die `getRequestBody`Hilfsfunktion hinzu:** Fügen Sie diese Funktion **direkt unter** der `delay`Funktion hinzu:
    
    ```jsx
    // app.js - Version 7: Asynchrones Parsen des POST-Bodys
    
    // ... (vorheriger Code) ...
    
    const delay = ms => new Promise(resolve => setTimeout(resolve, ms));
    
    // Hilfsfunktion zum asynchronen Sammeln des Request-Bodys
    // Diese Funktion gibt ein Promise zurück, das mit dem vollständigen Body-String aufgelöst wird.
    function getRequestBody(req) {
        return new Promise((resolve, reject) => {
            let body = ''; // Variable, um die Datenstücke zu sammeln
    
            // 'data'-Ereignis: Wird ausgelöst, wenn ein Datenstück (chunk) empfangen wird
            req.on('data', chunk => {
                body += chunk.toString(); // Konvertiert den Buffer zu String und fügt ihn zum Body hinzu
            });
    
            // 'end'-Ereignis: Wird ausgelöst, wenn der gesamte Body empfangen wurde
            req.on('end', () => {
                resolve(body); // Lösen Sie das Promise mit dem gesammelten Body auf
            });
    
            // 'error'-Ereignis: Fehlerbehandlung, falls ein Problem beim Lesen des Streams auftritt
            req.on('error', err => {
                reject(err); // Lehnt das Promise bei einem Fehler ab
            });
        });
    }
    
    const server = http.createServer(async (req, res) => {
    // ... (Rest des Codes) ...
    
    ```
    
    - **Erklärung:**
        - `getRequestBody(req)`: Diese Funktion nimmt das `req` (Request)-Objekt entgegen, das ein **Readable Stream** ist.
        - `new Promise((resolve, reject) => { ... })`: Wir erstellen ein neues Promise, das entweder `resolve` (erfolgreich) oder `reject` (Fehler) aufruft.
        - `req.on('data', chunk => { ... })`: Dies ist ein **Ereignis-Listener**. Immer wenn ein neues Datenstück des Request-Bodys empfangen wird, wird diese Callback-Funktion ausgeführt. `chunk` ist ein `Buffer`Objekt, das wir mit `toString()` in einen String umwandeln.
        - `req.on('end', () => { ... })`: Dieser Listener wird aufgerufen, wenn der gesamte Request-Body empfangen wurde. Dann rufen wir `resolve(body)` auf, um das Promise mit dem vollständigen Body-String zu erfüllen.
        - `req.on('error', err => { ... })`: Ein wichtiger Listener für die Fehlerbehandlung, falls beim Lesen des Streams ein Problem auftritt.
2. **Verarbeiten Sie den POST-Endpunkt asynchron:** Ersetzen Sie den `else if (req.url === '/posts' && req.method === 'POST')` Block durch den folgenden Code:
    
    ```jsx
    // app.js - Version 7: Asynchrones Parsen des POST-Bodys
    
    // ... (vorheriger Code) ...
    
    } else if (req.url === '/posts' && req.method === 'POST') {
        try {
            // Warten Sie asynchron, bis der gesamte Request-Body empfangen wurde
            const body = await getRequestBody(req);
            // Wandeln Sie den JSON-String im Body in ein JavaScript-Objekt um
            const newPost = JSON.parse(body);
    
            // Grundlegende Validierung: Prüfen, ob notwendige Felder vorhanden sind
            if (!newPost.title || !newPost.content || !newPost.author) {
                res.writeHead(400, { 'Content-Type': 'application/json' }); // 400 Bad Request
                res.end(JSON.stringify({ message: 'Fehlende Felder: title, content und author sind erforderlich.' }));
                return; // Beenden Sie die Funktion hier
            }
    
            // Weisen Sie dem neuen Beitrag eine ID und das aktuelle Datum zu
            newPost.id = nextId++;
            newPost.date = new Date().toISOString().split('T')[0]; // Aktuelles Datum im Format YYYY-MM-DD
    
            posts.push(newPost); // Fügen Sie den neuen Beitrag zur In-Memory-Liste hinzu
    
            res.writeHead(201, { 'Content-Type': 'application/json' }); // 201 Created Statuscode
            await delay(200); // Simulierte Verzögerung für die Datenbankoperation
            res.end(JSON.stringify(newPost)); // Senden Sie den neu erstellten Beitrag als JSON zurück
        } catch (error) {
            // Fehlerbehandlung: Wenn JSON ungültig ist oder andere Fehler auftreten
            console.error('Fehler beim Parsen der Anfrage oder ungültiges JSON:', error);
            res.writeHead(400, { 'Content-Type': 'application/json' }); // 400 Bad Request
            res.end(JSON.stringify({ message: 'Ungültige Anfrage: JSON-Format erwartet oder Daten fehlen.' }));
        }
    } else {
        res.writeHead(404, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify({ message: 'Endpunkt nicht gefunden' }));
    }
    
    ```
    
    - **Erklärung:**
        - `try...catch`: Dies ist ein wichtiger Block für die **Fehlerbehandlung**. Wenn innerhalb des `try`Blocks ein Fehler auftritt (z.B. wenn `JSON.parse` auf ungültige Daten trifft), wird die Ausführung sofort zum `catch`Block springen.
        - `const body = await getRequestBody(req);`: Hier rufen wir unsere Hilfsfunktion auf und **warten**mit `await`, bis der gesamte Request-Body asynchron gesammelt wurde.
        - `const newPost = JSON.parse(body);`: Sobald der Body als String verfügbar ist, parsen wir ihn in ein JavaScript-Objekt.
        - **Validierung:** Wir führen eine einfache Validierung durch, um sicherzustellen, dass die erforderlichen Felder (`title`, `content`, `author`) im neuen Beitrag vorhanden sind. Wenn nicht, senden wir einen `400 Bad Request`Statuscode.
        - `newPost.id = nextId++;`: Wir weisen dem neuen Beitrag eine eindeutige ID zu und erhöhen `nextId` für den nächsten Beitrag.
        - `newPost.date = new Date().toISOString().split('T')[0];`: Fügt das aktuelle Datum hinzu.
        - `posts.push(newPost);`: Fügt den neuen Beitrag zu unserem `posts`Array hinzu.
        - `res.writeHead(201, { ... })` und `res.end(JSON.stringify(newPost))`: Senden die Erfolgsantwort mit dem Statuscode `201 Created` und dem neu erstellten Beitrag.

**Code Check 7: Ihr `app.js` sollte jetzt so aussehen (Endversion)**

```jsx
// app.js - Version 7: Asynchrones Parsen des POST-Bodys

import http from 'http';
import { readFile } from 'fs/promises';
import path from 'path';
import { fileURLToPath } from 'url';

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

const configPath = path.join(__dirname, 'config.json');
const config = JSON.parse(await readFile(configPath, 'utf8'));
const { port, hostname } = config;

let posts = [
    { id: 1, title: 'Mein erster Blogbeitrag', content: 'Das ist der Inhalt meines ersten Beitrags. Willkommen in der Welt von Node.js!', author: 'Alice', date: '2024-07-29' },
    { id: 2, title: 'Node.js Grundlagen verstehen', content: 'Heute lernen wir die Event Loop und asynchrone Programmierung kennen.', author: 'Bob', date: '2024-07-28' }
];
let nextId = 3;

const delay = ms => new Promise(resolve => setTimeout(resolve, ms));

// Hilfsfunktion zum asynchronen Sammeln des Request-Bodys
function getRequestBody(req) {
    return new Promise((resolve, reject) => {
        let body = '';
        req.on('data', chunk => {
            body += chunk.toString();
        });
        req.on('end', () => {
            resolve(body);
        });
        req.on('error', err => {
            reject(err);
        });
    });
}

const server = http.createServer(async (req, res) => {
    console.log(`Anfrage erhalten: ${req.method} ${req.url}`);

    res.setHeader('Access-Control-Allow-Origin', '*');
    res.setHeader('Access-Control-Allow-Methods', 'GET, POST, OPTIONS');
    res.setHeader('Access-Control-Allow-Headers', 'Content-Type');

    if (req.method === 'OPTIONS') {
        res.writeHead(204);
        res.end();
        return;
    }

    if (req.url === '/posts' && req.method === 'GET') {
        res.writeHead(200, { 'Content-Type': 'application/json' });
        await delay(500);
        res.end(JSON.stringify(posts));
    } else if (req.url.match(/^\/posts\/(\d+)$/) && req.method === 'GET') {
        const id = parseInt(req.url.split('/')[2]);
        const post = posts.find(p => p.id === id);

        if (post) {
            res.writeHead(200, { 'Content-Type': 'application/json' });
            await delay(300);
            res.end(JSON.stringify(post));
        } else {
            res.writeHead(404, { 'Content-Type': 'application/json' });
            res.end(JSON.stringify({ message: 'Blogbeitrag nicht gefunden' }));
        }
    } else if (req.url === '/posts' && req.method === 'POST') {
        try {
            const body = await getRequestBody(req);
            const newPost = JSON.parse(body);

            if (!newPost.title || !newPost.content || !newPost.author) {
                res.writeHead(400, { 'Content-Type': 'application/json' });
                res.end(JSON.stringify({ message: 'Fehlende Felder: title, content und author sind erforderlich.' }));
                return;
            }

            newPost.id = nextId++;
            newPost.date = new Date().toISOString().split('T')[0];
            posts.push(newPost);

            res.writeHead(201, { 'Content-Type': 'application/json' });
            await delay(200);
            res.end(JSON.stringify(newPost));
        } catch (error) {
            console.error('Fehler beim Parsen der Anfrage oder ungültiges JSON:', error);
            res.writeHead(400, { 'Content-Type': 'application/json' });
            res.end(JSON.stringify({ message: 'Ungültige Anfrage: JSON-Format erwartet oder Daten fehlen.' }));
        }
    } else {
        res.writeHead(404, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify({ message: 'Endpunkt nicht gefunden' }));
    }
});

server.listen(port, hostname, () => {
    console.log(`Server läuft unter http://${hostname}:${port}/`);
    console.log('API-Endpunkte zum Testen:');
    console.log(`GET http://${hostname}:${port}/posts`);
    console.log(`GET http://${hostname}:${port}/posts/1`);
    console.log(`POST http://${hostname}:${port}/posts (mit JSON-Body)`);
});

```

1. **Testen Sie den Server:**
    - Stoppen Sie den Server (`Strg + C`), falls er noch läuft.
    - Starten Sie den Server mit: `npm start`
    - Verwenden Sie `curl` (oder ein Tool wie Postman/Insomnia), um eine POST-Anfrage mit einem JSON-Body zu senden:
        
        ```bash
        curl -X POST -H "Content-Type: application/json" -d '{ "title": "Mein neuer Beitrag", "content": "Dieser Beitrag wurde über die API hinzugefügt.", "author": "Charlie" }' http://localhost:3000/posts
        
        ```
        
        Sie sollten den neu hinzugefügten Beitrag als JSON-Antwort erhalten.
        
    - Rufen Sie dann `http://localhost:3000/posts` im Browser auf, um zu überprüfen, ob der neue Beitrag in der Liste ist.
    - Versuchen Sie eine ungültige POST-Anfrage (z.B. kein JSON oder fehlende Felder):
        
        ```bash
        curl -X POST -H "Content-Type: application/json" -d 'Dies ist kein JSON' http://localhost:3000/posts
        
        ```
        
        oder
        
        ```bash
        curl -X POST -H "Content-Type: application/json" -d '{ "title": "Nur Titel" }' http://localhost:3000/posts
        
        ```
        
        Sie sollten eine `400 Bad Request`-Antwort erhalten.
        
    - **Erklärung:** Sie haben erfolgreich einen voll funktionsfähigen API-Endpunkt für das Erstellen von Blogbeiträgen implementiert, der asynchrone Datenströme verarbeitet und grundlegende Fehlerbehandlung bietet.

### Herzlichen Glückwunsch!

Sie haben nun einen grundlegenden Node.js-Server erstellt, der GET- und POST-Anfragen verarbeiten kann, Konfigurationen lädt und die asynchrone Natur von Node.js demonstriert. Dies ist eine solide Grundlage für die Entwicklung komplexerer Webanwendungen.

### Was kommt als Nächstes?

- **Express.js:** Für komplexere APIs ist das `http`Modul zu "low-level". Ein Framework wie **Express.js** vereinfacht das Routing, die Middleware-Verwaltung und die Handhabung von Anfragen und Antworten erheblich. Es ist der nächste logische Schritt.
- **Datenbank-Integration:** Um Ihre Blogbeiträge dauerhaft zu speichern, müssten Sie eine echte Datenbank (z.B. MongoDB, PostgreSQL) integrieren.
- **Fehlerbehandlung:** Verbessern Sie die Fehlerbehandlung und das Logging.
- **Authentifizierung & Autorisierung:** Fügen Sie Sicherheitsmechanismen hinzu, um den Zugriff auf Ihre API zu kontrollieren.