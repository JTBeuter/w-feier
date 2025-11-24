# 🎮 Nobody's Perfect - Web-App

Eine browserbasierte Web-App für das Spiel "Nobody's Perfect" mit Echtzeit-Datenbank und automatischer Punktevergabe.

## Features

✅ **Team-Management** - Teams können sich mit Namen registrieren  
✅ **Fragen & Antworten** - Host kann Fragen stellen, Teams geben Antworten  
✅ **Voting System** - Teams können auf alle Antworten außer ihrer eigenen abstimmen  
✅ **Automatische Punkte** - Punkte nach Nobody's Perfect-System:
- Richtige Antwort: +2 Punkte
- Pro Stimme auf die eigene Antwort: +1 Punkt

✅ **Echtzeit-Aktualisierung** - Firebase WebSockets, kein Reload nötig  
✅ **Responsive Design** - Funktioniert auf Desktop, Tablet und Handy  
✅ **Persistente Speicherung** - Alle Daten in Firebase Realtime Database

## Technologie-Stack

- **Frontend**: HTML5, CSS3, JavaScript (ES6+)
- **Backend**: Firebase Realtime Database
- **Authentifizierung**: Firebase Anonymous Authentication
- **Hosting**: GitHub Pages / lokaler Server

## Installation & Setup

### 1. Firebase Projekt vorbereiten

1. Gehe zu [Firebase Console](https://console.firebase.google.com)
2. Erstelle ein neues Projekt oder nutze ein bestehendes
3. Aktiviere **Realtime Database** (im `europe-west1` Region)
4. Gehe zu **Authentication** → Sign-in method → Aktiviere **Anonymous**
5. Database Rules anpassen für Echtzeit-Daten:

```json
{
  "rules": {
    "game": {
      ".read": true,
      ".write": true
    }
  }
}
```

### 2. Firebase Config eintragen

Die Firebase-Konfiguration ist bereits in `index.html` und `host.html` eingetragen:

```javascript
const firebaseConfig = {
    apiKey: "AIzaSyC85YAPrSD3a4uw-bzTOWWAx18KBG4Enes",
    authDomain: "w-feier.firebaseapp.com",
    databaseURL: "https://w-feier-default-rtdb.europe-west1.firebasedatabase.app",
    projectId: "w-feier",
    storageBucket: "w-feier.firebasestorage.app",
    messagingSenderId: "319969590062",
    appId: "1:319969590062:web:7601a072a2429df53758bf",
    measurementId: "G-C7KVSD61MF"
};
```

### 3. Lokal testen

```bash
# Mit Python 3
python -m http.server 8000

# Oder mit Node.js
npx http-server

# Öffne dann http://localhost:8000 im Browser
```

### 4. Auf GitHub Pages deployen

```bash
# Repository klonen oder erstellen
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/USERNAME/w-feier.git
git push -u origin main

# In GitHub Repository Settings:
# - Settings → Pages
# - Source: main branch
# - Save
```

Nach ca. 1-2 Minuten ist die App unter `https://USERNAME.github.io/w-feier/` erreichbar.

## Nutzung

### 👥 Für Spieler

1. Öffne `index.html` (oder die GitHub Pages URL)
2. Gib deinen Teamnamen ein
3. Klicke "Spiel beitreten"
4. Warte auf die erste Frage
5. Gib deine Antwort ein
6. Stimme auf andere Antworten ab
7. Schau das Scoreboard an

### 🎮 Für Host

1. Öffne `host.html` (oder GitHub Pages `/host.html`)
2. Gib eine Frage und die richtige Antwort ein
3. Klicke "Frage stellen"
4. Beobachte die Antworten in Echtzeit
5. Klicke "Runde beenden" um Punkte zu vergeben
6. Starte neue Runde

## Datenbankstruktur

```
game/
├── currentQuestion/          # Aktuelle Frage
│   ├── id: "q1"
│   ├── text: "Frage..."
│   ├── correct: "Richtige Antwort"
│   └── createdAt: "2025-11-21T..."
│
├── questions/                 # Fragen-Historie
│   └── q1/
│       ├── id: "q1"
│       ├── text: "..."
│       └── correct: "..."
│
├── answers/                   # Antworten nach Frage
│   └── q1/
│       └── TeamName/
│           ├── text: "Antwort..."
│           ├── votes: 2
│           └── submittedAt: "..."
│
├── scores/                    # Punktestand
│   ├── Team1: 10
│   ├── Team2: 8
│   └── Team3: 5
│
└── teams/                     # Registrierte Teams
    └── TeamName/
        ├── name: "TeamName"
        └── joinedAt: "..."
```

## Gameplay Beispiel

**Runde 1:**
- Host stellt Frage: "Wie heißt die Hauptstadt von Australien?"
- Teams antworten: Team A: "Sydney", Team B: "Melbourne", Team C: "Canberra"
- Richtige Antwort: "Canberra"
- Teams stimmen ab:
  - Team A: 1 Stimme
  - Team B: 0 Stimmen
  - Team C: 3 Stimmen

**Punktevergabe:**
- Team A: 0 (falsch) + 1 (Stimme) = 1 Punkt
- Team B: 0 (falsch) + 0 (Stimmen) = 0 Punkte
- Team C: 2 (richtig) + 3 (Stimmen) = 5 Punkte

## Buttons & Funktionen

### Spieler-Interface
- **Spiel beitreten** - Mit Teamnamen beitreten
- **Zum Host-Panel** - Wechsel zur Host-Verwaltung
- **Antwort einreichen** - Antwortspeichern
- **Abstimmen** - Auf andere Antworten klicken zum Abstimmen
- **Verlassen** - Spiel beenden und neuen Teamnamen eingeben

### Host-Interface
- **Frage stellen** - Neue Frage + richtige Antwort abspeichern
- **Runde beenden** - Punkte berechnen und vergeben
- **Spiel zurücksetzen** - Alle Daten löschen (⚠️ nicht rückgängig machen)
- **Zurück zum Spiel** - Zur Spieler-Ansicht

## Troubleshooting

### Problem: Daten werden nicht synchronisiert
- Überprüfe Firebase Database Rules (sollte `.read: true` sein)
- Überprüfe Browser-Konsole auf Fehler (F12)
- Stelle sicher, dass Internet-Verbindung aktiv ist

### Problem: Spiel lädt nicht
- Leere Browser Cache (Strg+Shift+L)
- Überprüfe Firebase Config in index.html/host.html
- Prüfe ob Firebase Projekt aktiv ist

### Problem: Teams sehen keine Frage
- Host muss zuerst "Frage stellen" klicken
- Refreshe die Seite im Browser der Spieler

## Customization

### Design anpassen
Alle Farben/Styles sind in `styles.css` definiert. Ändere die CSS-Variablen:

```css
:root {
    --primary-color: #6366f1;    /* Hauptfarbe */
    --secondary-color: #ec4899;   /* Sekundärfarbe */
    --success-color: #10b981;     /* Erfolgsgrün */
    --bg-color: #0f172a;          /* Dunkler Hintergrund */
}
```

### Punkt-System ändern
In `host.html` in der `finishRound()` Funktion:

```javascript
if (answerData.text.toLowerCase() === currentQuestion.correct.toLowerCase()) {
    scores[teamName] += 2;  // Ändere diese Zahl für "Richtig"-Punkte
}
scores[teamName] += (answerData.votes || 0);  // Ändere für Abstimmungs-Punkte
```

## Mobile Optimierung

Die App ist vollständig responsive:
- Mobile: 1 Spalte
- Tablet: 2 Spalten bei größerer Ansicht
- Desktop: Optimal formatiert

Breakpoints in `styles.css`:
- 768px - Tablet
- 480px - Mobile

## Performance

- **Live-Updates**: Firebase WebSockets (sofort)
- **Datenbankzugriffe**: Optimiert mit Transaktionen
- **Bundle Size**: < 1 MB (nur HTML/CSS/JS)
- **Kompatibilität**: Alle modernen Browser (Chrome, Firefox, Safari, Edge)

## Sicherheit

⚠️ **Wichtig**: Diese Demo nutzt anonyme Authentifizierung und öffentliche Datenbank-Rules. 
Für Produktion solltest du:
- Authentifizierung mit User-Accounts implementieren
- Database Rules restriktiver setzen
- Daten-Validierung hinzufügen
- HTTPS verwenden (GitHub Pages nutzt automatisch HTTPS)

## Support & Fragen

Bei Problemen:
1. Überprüfe Firebase Console auf Fehler
2. Schau Browser-Konsole an (F12 → Console)
3. Probiere Seite zu refreshen
4. Setze das Spiel zurück (Host Panel)

## Lizenz

Kostenlos verwendbar. Viel Spaß beim Spielen! 🎮

---

**Erstellt mit ❤️ für Nobody's Perfect**

Letzte Aktualisierung: November 2025
