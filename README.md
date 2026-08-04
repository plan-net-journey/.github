# Plan Net Journey - Organisation

Dieses Repository verwaltet organisationsweite Prozesse fuer [plan-net-journey](https://github.com/plan-net-journey).

## Zugang beantragen

Neuer Mitarbeiter? [Erstelle ein Access Request Issue](../../issues/new?template=access-request.yml), um Zugang zur Organisation zu beantragen.

**Voraussetzung:** Ein GitHub Account mit deiner `@house-of-communication.com` E-Mail Adresse.

## Neues Repository beantragen

Org-Mitglied? [Erstelle ein Repo Request Issue](../../issues/new?template=repo-request.yml), um ein neues Repository fuer einen Kunden oder ein Projekt anzulegen.

## Ablauf

1. Issue erstellen
2. Admin prueft und setzt das Label `approved`
3. GitHub Action fuehrt die Aktion automatisch aus (Einladung / Repo-Erstellung)
4. Issue wird automatisch geschlossen

## Admin Setup

Admins muessen einmalig einen Personal Access Token (classic) mit `admin:org` und `repo` Scope als Repository Secret `ORG_ADMIN_TOKEN` hinterlegen.

Anleitung:
1. [Token erstellen](https://github.com/settings/tokens/new?scopes=admin:org,repo&description=PNJ-Org-Admin)
2. In diesem Repo: Settings → Secrets and variables → Actions → New repository secret
3. Name: `ORG_ADMIN_TOKEN`, Value: der erstellte Token
