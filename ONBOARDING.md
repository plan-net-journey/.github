# Plan Net Journey (PNJ) — GitHub Onboarding

Du hilfst einem neuen Teammitglied, sich fuer die Plan Net Journey GitHub Organisation einzurichten. Fuehre den User Schritt fuer Schritt durch alle Phasen. Pruefe bei jedem Schritt automatisch den aktuellen Zustand, ueberspringe bereits erledigte Schritte, und bestatige den Erfolg bevor du weitergehst.

## Kontext

Plan Net Journey nutzt GitHub fuer die kollaborative Entwicklung von Skills, Agents und Projekt-Assets. Alle Projektdaten — CLAUDE.md, MEMORY.md, Skills, Agent-Konfigurationen — werden lokal gespeichert und mit git versioniert. Grosse Dateien (PPTX, Videos) werden ueber symbolische Links auf Box/OneDrive oder git LFS verwaltet.

Das primaere Arbeitstool ist **Claude Code** (Terminal CLI oder Desktop App).

Die GitHub Organisation: **plan-net-journey** (https://github.com/plan-net-journey)

---

## Phase 1: Tools installieren und verifizieren

Fuehre ALLE folgenden Checks aus und installiere fehlende Tools. Nach jeder Installation verifiziere mit dem gleichen Check-Befehl.

### 1.1 Homebrew (macOS Paketmanager)

```bash
which brew && brew --version 2>&1 | head -1 || echo "NICHT INSTALLIERT"
```

Falls nicht installiert — der User muss das interaktiv ausfuehren:
```
! /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
Danach muss der User ggf. den PATH aktualisieren (brew zeigt das nach Installation an).

### 1.2 Git

```bash
git --version 2>&1 || echo "NICHT INSTALLIERT"
```

Falls nicht installiert:
```bash
xcode-select --install
```
Oder falls brew verfuegbar: `brew install git`

### 1.3 GitHub CLI (gh)

```bash
gh --version 2>&1 | head -1 || echo "NICHT INSTALLIERT"
```

Falls nicht installiert:
```bash
brew install gh
```

### 1.4 Claude Code

```bash
which claude && claude --version 2>/dev/null || echo "NICHT INSTALLIERT"
```

Falls nicht installiert — der User muss das interaktiv ausfuehren:
```
! brew install --cask claude-code
```

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

Frag den User ob bereits ein GitHub Account existiert.

**Falls NEIN (neuer Account):**
1. Sag dem User, dass ein GitHub Account benoetigt wird
2. **Empfehlung:** Die E-Mail sollte `vorname.nachname@house-of-communication.com` sein
3. Sag dem User: Geh zu https://github.com/signup und erstelle einen Account
4. Warte bis der User fertig ist und frag nach dem GitHub Username
5. Verifiziere: `curl -s https://api.github.com/users/USERNAME --max-time 10 | grep login`

**Falls JA (bestehender Account):**
1. Frag nach dem GitHub Username
2. Verifiziere dass der Account existiert: `curl -s https://api.github.com/users/USERNAME --max-time 10 | grep login`
3. Der User kann seinen bestehenden Account verwenden, unabhaengig von der E-Mail
4. Optional: Empfehle die @house-of-communication.com Adresse als zusaetzliche E-Mail hinzuzufuegen (GitHub Settings → Emails)

Merke dir den Username fuer spaetere Schritte.

---

## Phase 3: SSH Key einrichten

Pruefe ob bereits ein SSH Key existiert:
```bash
ls -la ~/.ssh/id_ed25519.pub 2>/dev/null && cat ~/.ssh/id_ed25519.pub | head -1 && echo "SSH Key vorhanden" || echo "Kein SSH Key gefunden"
```

**Falls kein Key vorhanden:**

Der User muss den Befehl interaktiv ausfuehren (Passphrase-Abfrage). Verwende die E-Mail des Users (die mit der der GitHub Account registriert ist):
```
! ssh-keygen -t ed25519 -C "user@example.com" -f ~/.ssh/id_ed25519
```

Warte bis der User bestaetigt, dass der Key erstellt wurde, dann verifiziere:
```bash
ls -la ~/.ssh/id_ed25519.pub && echo "Key erfolgreich erstellt"
```

**SSH Agent konfigurieren:**
```bash
eval "$(ssh-agent -s)" && ssh-add ~/.ssh/id_ed25519 2>&1
```

**macOS Keychain Integration** — pruefe und konfiguriere `~/.ssh/config`:

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

## Phase 5: GitHub CLI authentifizieren

Pruefe ob gh bereits authentifiziert ist:
```bash
gh auth status 2>&1
```

**Falls nicht authentifiziert:**

Der User muss `gh auth login` interaktiv ausfuehren:
```
! gh auth login
```

Fuehre ihn durch die Auswahl:
- **GitHub.com** waehlen
- **SSH** als Protokoll waehlen
- Den vorhandenen SSH Key (`~/.ssh/id_ed25519.pub`) auswaehlen
- **Login with a web browser** waehlen

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

## Phase 6: SSH-Verbindung testen

```bash
ssh -T git@github.com 2>&1
```

Erwartete Antwort: `Hi USERNAME! You've successfully authenticated, but GitHub does not provide shell access.`

Falls `Permission denied (publickey)`:
1. Pruefe ob der SSH Agent laeuft: `ssh-add -l`
2. Falls leer: `ssh-add ~/.ssh/id_ed25519`
3. Pruefe SSH config: `cat ~/.ssh/config`
4. Pruefe ob Key auf GitHub ist: `gh ssh-key list`
5. Teste verbose: `ssh -vT git@github.com 2>&1 | grep -E "(Offering|Accepted|Authentication)"`

Wiederhole den Test bis er erfolgreich ist. NICHT weitergehen bevor SSH funktioniert.

---

## Phase 7: Zugang zur Organisation beantragen

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
- Das Issue wurde erstellt und ein Admin wird automatisch benachrichtigt
- Nach Freigabe (Admin antwortet mit `/approve`) erhaelt der User eine Einladung per E-Mail
- Die Einladung muss in GitHub angenommen werden
- Das kann einige Minuten bis Stunden dauern

Falls das `.github` Repo nicht erreichbar ist:
- Kontaktiere **@basteiz** auf GitHub oder schreib an b.zimmermann@house-of-communication.com

---

## Phase 8: Setup verifizieren (nach Org-Zugang)

Falls der User bereits Org-Mitglied ist oder gerade eingeladen wurde und angenommen hat:

```bash
echo "=== Org-Repos ===" && \
gh api orgs/plan-net-journey/repos --jq '.[].name' 2>/dev/null && \
echo "=================="
```

Test-Clone via SSH:
```bash
mkdir -p ~/clients && cd ~/clients && \
gh repo clone plan-net-journey/Journey-Skills -- --depth 1 2>&1
```

Verifiziere den Clone:
```bash
ls -la ~/clients/Journey-Skills/ && echo "Clone erfolgreich!"
```

Raeume auf:
```bash
rm -rf ~/clients/Journey-Skills
```

Falls der User noch nicht Mitglied ist, ueberspringe diesen Schritt und erwaehne, dass er ihn nach Erhalt der Einladung selbst durchfuehren kann.

---

## Phase 9: gh CLI in Claude Code verifizieren

Teste dass gh-Befehle innerhalb von Claude Code funktionieren:
```bash
gh auth status && echo "--- gh funktioniert in Claude Code ---"
```

Teste git-Operationen:
```bash
cd /tmp && git init test-pnj-verify && cd test-pnj-verify && \
echo "test" > test.txt && git add . && git commit -m "test" && \
cd / && rm -rf /tmp/test-pnj-verify && echo "--- git funktioniert in Claude Code ---"
```

---

## Phase 10: Zusammenfassung

Zeige eine Zusammenfassung aller Ergebnisse:

```
=== PNJ Onboarding - Zusammenfassung ===

Tools:
  Homebrew:   [installiert / nicht installiert]
  Git:        [version]
  GitHub CLI: [version]
  Claude Code:[version]

GitHub:
  Username:   [username]
  E-Mail:     [email]
  SSH Key:    [eingerichtet / fehlt]
  SSH Test:   [erfolgreich / fehlgeschlagen]
  Org-Zugang: [Mitglied / beantragt / ausstehend]

Git Config:
  user.name:  [name]
  user.email: [email]
  Protokoll:  [SSH / HTTPS]

=========================================
```

**Naechste Schritte:**

1. **Journey Skills Marketplace klonen:**
   ```bash
   gh repo clone plan-net-journey/Journey-Skills
   ```
   Du hast automatisch Lesezugang. Der Marketplace enthaelt 50+ kundenuebergreifende Skills.
   Siehe `MARKETPLACE.md` fuer den vollstaendigen Katalog.

2. **Projektstruktur verstehen:**
   - `CLAUDE.md` — Projektkontext und Anweisungen fuer Claude
   - `MEMORY.md` — Index der gespeicherten Erinnerungen
   - `.claude/skills/` — Projektspezifische Skills (leben im Kunden-Repo)
   - `.claude/agents/` — Agent-Definitionen
   - Kundenuebergreifende Skills → Journey-Skills Marketplace

3. **Grosse Dateien (Videos, PPTX, etc.)** gehoeren nicht ins Git Repo. Sie bleiben in eurem Cloud-Storage (Box, OneDrive, Google Drive) und werden im Projekt per Symlink referenziert. Das wird eingerichtet wenn ein konkretes Kunden-Repo genutzt wird — die CLAUDE.md im Projekt erklaert wie.

**Alles weitere laeuft ueber Tickets:**

| Was | Ticket |
|-----|--------|
| Neues Repo fuer einen Kunden | [Repo Request](https://github.com/plan-net-journey/.github/issues/new?template=repo-request.yml) |
| Schreibzugang zu einem Repo | [Repo Access](https://github.com/plan-net-journey/.github/issues/new?template=repo-access-request.yml) |
| Schreibzugang zum Marketplace | [Repo Access](https://github.com/plan-net-journey/.github/issues/new?template=repo-access-request.yml) (Repo: Journey-Skills) |
| Etwas unklar oder fehlt | [Feedback](https://github.com/plan-net-journey/.github/issues/new?template=feedback.yml) |

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
- Weise am Ende auf das Feedback-Ticket hin: https://github.com/plan-net-journey/.github/issues/new?template=feedback.yml
