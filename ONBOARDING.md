# Plan Net Journey (PNJ) — GitHub Onboarding

Du hilfst einem neuen Teammitglied, sich fuer die Plan Net Journey GitHub Organisation einzurichten. Fuehre den User Schritt fuer Schritt durch alle Phasen. Pruefe bei jedem Schritt automatisch den aktuellen Zustand, ueberspringe bereits erledigte Schritte, und bestatige den Erfolg bevor du weitergehst.

**WICHTIG:** Viele User haben noch nie mit einem Terminal, Git oder GitHub gearbeitet. Erklaere bei jedem Schritt kurz WARUM er noetig ist, nicht nur WAS zu tun ist. Verwende einfache Sprache. Wenn ein Befehl interaktiv laufen muss (Passphrase, Browser-Login), sage dem User genau was passieren wird bevor er es ausfuehrt.

## Kontext

Plan Net Journey nutzt GitHub um Skills, Agents und Projekt-Wissen im Team zu teilen. Statt Prompts per Mail zu schicken, werden sie als Dateien gespeichert und versioniert — so profitieren alle von Verbesserungen.

**Claude Code** ist das Werkzeug dafuer. Es gibt zwei Varianten — beide funktionieren gleich:
- **Claude Code Desktop App** (empfohlen fuer Einsteiger): Eine App die man herunterlaedt und oeffnet, aehnlich wie andere Desktop-Apps. Man tippt Fragen und Anweisungen ein, Claude arbeitet direkt mit den Dateien auf dem Rechner.
- **Claude Code CLI**: Fuer Leute die gerne im Terminal arbeiten.

**Nicht verwechseln:** Die normale "Claude" Chat-App (claude.ai) ist etwas anderes — sie kann keine Dateien auf dem Rechner bearbeiten und keine Skills ausfuehren.

Die GitHub Organisation: **plan-net-journey** (https://github.com/plan-net-journey)

---

## Phase 1: Tools installieren und verifizieren

Der User liest diese Anleitung wahrscheinlich INNERHALB von Claude Code. Das heisst: Claude Code ist bereits installiert. Pruefe trotzdem alle Tools und installiere fehlende. Der User muss dafuer NICHTS im Terminal tun — du fuehrst die Befehle aus. Nur bei interaktiven Schritten (Passphrase, Browser-Login) muss der User selbst handeln — sage ihm dann genau was passieren wird.

Falls der User fragt "was ist ein Terminal" oder "wo finde ich das": Erklaere dass Claude Code bereits ein Terminal eingebaut hat. Er muss nichts separat oeffnen. Alles was er tun muss: hier in diesem Chat-Fenster mit dir sprechen.

### 1.1 Homebrew pruefen

Homebrew ist ein Programm das andere Programme installiert — wie ein App Store fuers Terminal. Viele Tools die wir brauchen werden darueber installiert.

```bash
which brew && brew --version 2>&1 | head -1 || echo "NICHT INSTALLIERT"
```

Falls nicht installiert, sag dem User:
> Ich muss Homebrew installieren — das ist ein Paketmanager fuer macOS, vergleichbar mit einem App Store. Es oeffnet sich ein Fenster das nach deinem Mac-Passwort fragt. Das ist normal und sicher.

Dann:
```
! /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 1.2 Git pruefen

Git ist das Versionierungssystem — es speichert alle Aenderungen an Dateien und macht sie teilbar.

```bash
git --version 2>&1 || echo "NICHT INSTALLIERT"
```

Falls nicht installiert — auf macOS kommt meistens eine Aufforderung die "Command Line Tools" zu installieren. Sag dem User:
> Es oeffnet sich ein Installations-Dialog. Klicke auf "Installieren" und warte bis es fertig ist (kann ein paar Minuten dauern).

```bash
xcode-select --install
```

### 1.3 GitHub CLI pruefen

Die GitHub CLI (`gh`) ist ein Werkzeug um mit GitHub zu sprechen — Repos klonen, Issues erstellen, Zugang verwalten.

```bash
gh --version 2>&1 | head -1 || echo "NICHT INSTALLIERT"
```

Falls nicht installiert:
```bash
brew install gh
```

### 1.4 Alles pruefen

Zeige dem User eine Uebersicht:
```bash
echo "=== Dein Setup ===" && \
echo -n "Homebrew: " && (brew --version 2>&1 | head -1 || echo "FEHLT") && \
echo -n "Git:      " && (git --version 2>&1 || echo "FEHLT") && \
echo -n "GitHub CLI:" && (gh --version 2>&1 | head -1 || echo "FEHLT") && \
echo "==================="
```

Alle drei muessen installiert sein bevor es weitergeht. Falls etwas fehlt und du es nicht installieren kannst, erklaere dem User was er tun muss — in einfacher Sprache, ohne Fachbegriffe.

Falls `brew` nicht verfuegbar oder es Probleme gibt:
```
! npm install -g @anthropic-ai/claude-code
```

Falls npm auch nicht verfuegbar:
```
! brew install node
! npm install -g @anthropic-ai/claude-code
```

**Hinweis:** Claude Code CLI und Claude Code Desktop App teilen sich das gleiche Shell-Environment. Alle hier installierten Tools funktionieren in beiden.

### 1.5 Verifikations-Check

Fuehre diesen Sammel-Check aus und zeige dem User das Ergebnis:

```bash
echo "=== Tool-Verifikation ===" && \
echo -n "Homebrew: " && (brew --version 2>&1 | head -1 || echo "FEHLT") && \
echo -n "Git:      " && (git --version 2>&1 || echo "FEHLT") && \
echo -n "GitHub CLI:" && (gh --version 2>&1 | head -1 || echo "FEHLT") && \
echo -n "Claude:   " && (claude --version 2>/dev/null || echo "FEHLT") && \
echo "========================="
```

Alle vier muessen installiert sein bevor es weitergeht.

---

## Phase 2: GitHub Account

GitHub ist die Plattform auf der unsere Projekte liegen — aehnlich wie OneDrive oder SharePoint fuer Dateien, aber speziell fuer Code und Skills. Man braucht einen Account um darauf zuzugreifen.

Frag den User ob bereits ein GitHub Account existiert.

**Falls NEIN (neuer Account):**
1. Sag dem User, dass ein kostenloser GitHub Account benoetigt wird
2. **Empfehlung:** Am besten die Firmen-E-Mail verwenden: `vorname.nachname@house-of-communication.com`
3. Sag dem User: *"Oeffne https://github.com/signup in deinem Browser und erstelle einen Account. Melde dich wenn du fertig bist."*
4. Warte bis der User fertig ist und frag nach dem GitHub Username (das ist der Name den man bei der Registrierung gewaehlt hat)
5. Verifiziere: `curl -s https://api.github.com/users/USERNAME --max-time 10 | grep login`

**Falls JA (bestehender Account):**
1. Frag nach dem GitHub Username
2. Verifiziere dass der Account existiert: `curl -s https://api.github.com/users/USERNAME --max-time 10 | grep login`
3. Der User kann seinen bestehenden Account verwenden, egal welche E-Mail er dort nutzt
4. Optional: Empfehle die @house-of-communication.com Adresse als zusaetzliche E-Mail hinzuzufuegen (GitHub Settings → Emails)

Merke dir den Username fuer spaetere Schritte.

---

## Phase 3: SSH Key einrichten

Ein SSH Key ist wie ein digitaler Ausweis — er beweist gegenueber GitHub dass du wirklich du bist, ohne dass du jedes Mal ein Passwort eingeben musst. Der Key wird einmal erstellt und funktioniert dann automatisch.

Pruefe ob bereits ein SSH Key existiert:
```bash
ls -la ~/.ssh/id_ed25519.pub 2>/dev/null && cat ~/.ssh/id_ed25519.pub | head -1 && echo "SSH Key vorhanden" || echo "Kein SSH Key gefunden"
```

**Falls kein Key vorhanden:**

Sag dem User:
> Ich erstelle jetzt einen SSH Key fuer dich. Das ist wie ein digitaler Ausweis fuer GitHub. Du wirst nach einem Passwort gefragt — das schuetzt den Key auf deinem Rechner. Merke es dir oder lass es leer (dann wird nicht nach einem Passwort gefragt).

Der User muss den Befehl selbst ausfuehren (wegen der Passwort-Abfrage). Ersetze die E-Mail durch die des Users:
```
! ssh-keygen -t ed25519 -C "user@example.com" -f ~/.ssh/id_ed25519
```

Warte bis der User bestaetigt, dass der Key erstellt wurde, dann verifiziere:
```bash
ls -la ~/.ssh/id_ed25519.pub && echo "Key erfolgreich erstellt"
```

**SSH Agent konfigurieren** (damit der Key automatisch verwendet wird):
```bash
eval "$(ssh-agent -s)" && ssh-add ~/.ssh/id_ed25519 2>&1
```

**macOS Keychain Integration** (damit der Key auch nach einem Neustart noch funktioniert) — pruefe und konfiguriere `~/.ssh/config`:

Lies zuerst die bestehende config:
```bash
cat ~/.ssh/config 2>/dev/null || echo "Keine SSH config vorhanden"
```

Stelle sicher, dass folgender Block vorhanden ist (erstelle oder aktualisiere die Datei):
```
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

Falls die Datei nicht existiert, erstelle sie. Falls ein anderer `Host github.com` Block existiert, passe ihn an. Setze korrekte Berechtigungen:
```bash
chmod 600 ~/.ssh/config
```

---

## Phase 4: Git konfigurieren

Git muss wissen wer du bist — dein Name und deine E-Mail erscheinen bei jeder Aenderung die du speicherst, damit das Team sieht wer was gemacht hat.

Pruefe die aktuelle Konfiguration:
```bash
echo "Name:  $(git config --global user.name)"
echo "Email: $(git config --global user.email)"
```

**Wichtig:** Die globale Git-Config NICHT aendern falls der User bereits eine eingerichtet hat — das wuerde alle anderen Projekte beeinflussen.

**Falls git config komplett leer ist (neuer User):**

Frag den User nach vollem Namen und E-Mail:
```bash
git config --global user.name "Vorname Nachname"
git config --global user.email "vorname.nachname@house-of-communication.com"
```

**Falls bereits konfiguriert (bestehender User):**

Erklaere dem User, dass PNJ-Repos eine eigene Identity bekommen koennen. Nach dem Klonen eines PNJ-Repos kann man lokal setzen:
```bash
cd <repo-verzeichnis>
git config user.name "Vorname Nachname"
git config user.email "vorname.nachname@house-of-communication.com"
```

Das betrifft nur dieses eine Repo, nicht die globale Konfiguration.

**SSH-Protokoll:** Wird NICHT global umgeschrieben. Stattdessen nutzen wir `gh config`:
```bash
gh config set git_protocol ssh
```

Das sorgt dafuer, dass `gh repo clone` automatisch SSH verwendet, ohne andere git-Operationen zu beeinflussen.

---

## Phase 5: Bei GitHub anmelden

Damit Claude Code mit GitHub sprechen kann (Projekte herunterladen, Tickets erstellen, etc.), muss die Verbindung einmal hergestellt werden. Das passiert ueber die GitHub CLI — ein Werkzeug das im Hintergrund laeuft.

Pruefe ob die Verbindung bereits steht:
```bash
gh auth status 2>&1
```

**Falls nicht angemeldet:**

Sag dem User:
> Ich starte jetzt die Anmeldung bei GitHub. Es oeffnet sich dein Browser und du wirst gebeten dich bei GitHub einzuloggen. Danach ist die Verbindung hergestellt.

Der User muss den Befehl selbst ausfuehren:
```
! gh auth login
```

Fuehre ihn durch die Auswahl — sag ihm genau was er anklicken soll:
1. **GitHub.com** waehlen (nicht Enterprise)
2. **SSH** als Protokoll waehlen
3. Den vorhandenen SSH Key (`~/.ssh/id_ed25519.pub`) auswaehlen
4. **Login with a web browser** waehlen — dann oeffnet sich der Browser

**Falls bereits authentifiziert aber mit HTTPS statt SSH:**

```bash
gh config set git_protocol ssh
gh auth refresh -s admin:public_key
```

Der User muss die Refresh-Authentifizierung interaktiv bestaetigen falls noetig:
```
! gh auth refresh -s admin:public_key
```

**SSH Key zu GitHub hochladen:**

Pruefe ob der Key bereits auf GitHub ist:
```bash
LOCAL_KEY=$(cat ~/.ssh/id_ed25519.pub | awk '{print $2}')
echo "Lokaler Key: ${LOCAL_KEY:0:20}..."
gh ssh-key list 2>&1
```

Falls der lokale Key nicht in der Liste auftaucht:
```bash
gh ssh-key add ~/.ssh/id_ed25519.pub --title "$(hostname) - PNJ $(date +%Y-%m-%d)"
```

Verifiziere nach dem Upload:
```bash
gh ssh-key list
```

---

## Phase 6: Verbindung testen

Jetzt pruefen wir ob alles zusammen funktioniert — der SSH Key, die GitHub-Anmeldung, und die Verbindung.

```bash
ssh -T git@github.com 2>&1
```

Erwartete Antwort: `Hi USERNAME! You've successfully authenticated...`

Sag dem User:
> Wenn hier dein GitHub-Name steht, ist alles eingerichtet. Dein Rechner kann jetzt sicher mit GitHub kommunizieren.

Falls `Permission denied (publickey)` — debugge aktiv. Sag dem User nicht einfach "es hat nicht funktioniert", sondern pruefe:
1. Pruefe ob der SSH Agent laeuft: `ssh-add -l`
2. Falls leer: `ssh-add ~/.ssh/id_ed25519`
3. Pruefe SSH config: `cat ~/.ssh/config`
4. Pruefe ob Key auf GitHub ist: `gh ssh-key list`
5. Teste verbose: `ssh -vT git@github.com 2>&1 | grep -E "(Offering|Accepted|Authentication)"`

Falls du unsicher bist, sag dem User: *"Falls du unsicher bist, beschreibe mir was du siehst und ich helfe dir weiter."*

Wiederhole den Test bis er erfolgreich ist. NICHT weitergehen bevor die Verbindung steht.

---

## Phase 7: Zugang zum Team beantragen

Die "Organisation" auf GitHub ist wie ein Teamraum — dort liegen alle Projekte. Ohne Zugang kann man die Projekte nicht sehen.

Pruefe ob der User bereits Mitglied ist:
```bash
gh api orgs/plan-net-journey/memberships/USERNAME 2>/dev/null && echo "Bereits Mitglied!" || echo "Noch kein Mitglied"
```

**Falls bereits Mitglied:** Ueberspringe diesen Schritt.

**Falls kein Mitglied:**

Frag den User nach Team/Rolle und erstelle ein Access Request Issue:

```bash
gh issue create --repo plan-net-journey/.github \
  --title "Access Request: USERNAME" \
  --assignee basteiz \
  --body "### GitHub Username

USERNAME

### E-Mail Adresse

vorname.nachname@house-of-communication.com

### Voller Name

Vorname Nachname

### Team / Rolle

TEAM_ROLLE" \
  --label "access-request"
```

Sag dem User:
> Dein Antrag wurde erstellt! Ein Admin wird automatisch benachrichtigt und prueft ihn. Nach Freigabe bekommst du eine E-Mail von GitHub mit einer Einladung — klicke dort auf "Einladung annehmen". Das kann einige Minuten bis Stunden dauern.
> 
> Nach der Annahme hast du automatisch Zugang zum **Journey-Skills Marketplace** (50+ fertige Skills) und zum **admin-ops** Repo (Dokumentation und Tickets).

Falls das `.github` Repo nicht erreichbar ist:
- Kontaktiere **@basteiz** auf GitHub oder schreib an b.zimmermann@house-of-communication.com

---

## Phase 8: Zugang pruefen (nach Org-Beitritt)

Falls der User bereits Mitglied ist oder die Einladung gerade angenommen hat — teste ob der Zugang funktioniert.

Falls der User noch auf die Einladung wartet, ueberspringe diesen Schritt und sage:
> Sobald du die Einladungs-E-Mail bekommst und annimmst, oeffne Claude Code nochmal und sage "Ich habe die Einladung angenommen, teste meinen Zugang."

Pruefe welche Projekte sichtbar sind:
```bash
echo "=== Deine Projekte ===" && \
gh api orgs/plan-net-journey/repos --jq '.[].name' 2>/dev/null && \
echo "======================"
```

Teste ob ein Projekt heruntergeladen werden kann:
```bash
mkdir -p ~/clients && cd ~/clients && \
gh repo clone plan-net-journey/Journey-Skills -- --depth 1 2>&1
```

Verifiziere und raeume auf:
```bash
ls -la ~/clients/Journey-Skills/ && echo "Herunterladen hat funktioniert!" && rm -rf ~/clients/Journey-Skills
```

---

## Phase 9: Zusammenfassung

Zeige eine Zusammenfassung aller Ergebnisse:

```
=== PNJ Onboarding - Zusammenfassung ===

Tools:        alle installiert
GitHub User:  [username]
SSH Key:      eingerichtet
Verbindung:   [erfolgreich / fehlgeschlagen]
Team-Zugang:  [Mitglied / beantragt]

=========================================
```

**Was du jetzt hast:**

- **Journey-Skills Marketplace** — 50+ fertige Skills die du sofort nutzen kannst. Herunterladen mit: `gh repo clone plan-net-journey/Journey-Skills`
- **admin-ops** — Dokumentation, Admin-Skills und Tickets. Herunterladen mit: `gh repo clone plan-net-journey/admin-ops`

**Wie Projekte funktionieren:**

Jedes Projekt ist ein Ordner mit:
- `CLAUDE.md` — Beschreibt das Projekt fuer Claude (Kontext, Regeln, Struktur)
- `.claude/skills/` — Skills die nur fuer dieses Projekt relevant sind
- Grosse Dateien (Videos, PPTX, etc.) bleiben in eurem Cloud-Storage (Box, OneDrive) und werden per Verweis eingebunden

**Alles was du brauchst, bekommst du ueber Tickets:**

| Was du brauchst | Wie |
|-----------------|-----|
| Ein neues Projekt-Repo | [Ticket erstellen](https://github.com/plan-net-journey/admin-ops/issues/new?template=repo-request.yml) |
| Schreibzugang zu einem Projekt | [Ticket erstellen](https://github.com/plan-net-journey/admin-ops/issues/new?template=repo-access-request.yml) |
| Etwas funktioniert nicht / fehlt | [Feedback geben](https://github.com/plan-net-journey/admin-ops/issues/new?template=feedback.yml) |

---

## Hinweise fuer Claude

- **Sprache:** Antworte in der Sprache in der der User dich anspricht. Deutsch oder Englisch — passe dich an.
- Sei geduldig — viele User haben noch nie mit git/GitHub gearbeitet
- Erklaere WARUM jeder Schritt wichtig ist, nicht nur WAS zu tun ist
- Bei Fehlern: debugge aktiv, nicht nur Fehlermeldung zeigen
- Interaktive Befehle (ssh-keygen, gh auth login, brew install) muessen vom User mit `! befehl` ausgefuehrt werden
- Ueberpruefe jeden Schritt bevor du zum naechsten gehst
- Falls der User schon erfahren ist und Schritte ueberspringen will, lass ihn
- Am Ende die Zusammenfassung aus Phase 10 zeigen, auch wenn Schritte uebersprungen wurden
- Weise am Ende auf das Feedback-Ticket hin: https://github.com/plan-net-journey/admin-ops/issues/new?template=feedback.yml
