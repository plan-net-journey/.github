# Plan Net Journey - Organisation

Dieses Repository verwaltet organisationsweite Prozesse fuer [plan-net-journey](https://github.com/plan-net-journey).

## Onboarding — Neues Teammitglied

### Variante 1: Mit Claude Code (empfohlen)

1. [Claude Code installieren](https://claude.ai/download) (Desktop App oder CLI)
2. Claude Code oeffnen und einfach sagen: *"Hilf mir beim PNJ GitHub Onboarding"* und die [ONBOARDING.md](ONBOARDING.md) als Datei mitgeben — oder diesen Link verwenden: https://claude.ai/claude-code/onboard/iCikgg3eQUXj
3. Claude fuehrt dich automatisch durch alle Schritte

### Variante 2: Manuell

Folge den Schritten in der [ONBOARDING.md](ONBOARDING.md) selbst.

## Ticket-Workflows

| Was | Link | Wer |
|-----|------|-----|
| **Zugang zur Org beantragen** | [Access Request](../../issues/new?template=access-request.yml) | Jeder mit GitHub Account |
| **Neues Repo beantragen** | [Repo Request](../../issues/new?template=repo-request.yml) | Org-Mitglieder |
| **Schreibzugang zu Repo** | [Repo Access](../../issues/new?template=repo-access-request.yml) | Org-Mitglieder |

### Ablauf

1. Issue erstellen (ueber die Links oben)
2. Admin wird automatisch benachrichtigt
3. Admin antwortet mit `/approve` (auch per E-Mail-Reply moeglich)
4. GitHub Action fuehrt die Aktion automatisch aus
5. Issue wird automatisch geschlossen

### Regeln

- **Alle Repos sind private** — es gibt keine Ausnahmen
- Members koennen keine Repos direkt erstellen — alles ueber Tickets
- GitHub Account muss mit `@house-of-communication.com` E-Mail registriert sein
- SSH Keys sind Pflicht fuer git-Operationen

## Admin Setup

Admins muessen einmalig einen Personal Access Token (classic) mit `admin:org` und `repo` Scope als Repository Secret `ORG_ADMIN_TOKEN` hinterlegen.

1. [Token erstellen](https://github.com/settings/tokens/new?scopes=admin:org,repo&description=PNJ-Org-Admin)
2. In diesem Repo: Settings → Secrets and variables → Actions → New repository secret
3. Name: `ORG_ADMIN_TOKEN`, Value: der erstellte Token
