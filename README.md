# Plan Net Journey - Organisation

> **DRAFT** — Dieses Setup wird aktiv weiterentwickelt. [Feedback geben](../../issues/new?template=feedback.yml)

Dieses Repository verwaltet organisationsweite Prozesse fuer [plan-net-journey](https://github.com/plan-net-journey).

**Warum das alles?** Lies die [Erklaerung](ERKLAERUNG.html) — warum GitHub, warum Claude Code, warum dieses Setup.

## Onboarding — Neues Teammitglied

### Variante 1: Mit Claude Code (empfohlen)

1. [Claude Code installieren](https://claude.ai/download) (Desktop App oder CLI)
2. Dieses Repo klonen: `git clone https://github.com/plan-net-journey/.github.git`
3. Claude Code im Repo-Ordner oeffnen und sagen: *"Lies die ONBOARDING.md und fuehre mich durch das Setup"*
4. Claude fuehrt dich automatisch durch alle Schritte — auf Deutsch oder Englisch

### Variante 2: Manuell

Folge den Schritten in der [ONBOARDING.md](ONBOARDING.md) selbst.

## Ticket-Workflows

| Was | Link | Wer |
|-----|------|-----|
| **Zugang zur Org beantragen** | [Access Request](../../issues/new?template=access-request.yml) | Jeder mit GitHub Account |
| **Neues Repo beantragen** | [Repo Request](../../issues/new?template=repo-request.yml) | Org-Mitglieder |
| **Schreibzugang zu Repo** | [Repo Access](../../issues/new?template=repo-access-request.yml) | Org-Mitglieder |
| **Feedback geben** | [Feedback](../../issues/new?template=feedback.yml) | Alle |

### Ablauf

1. Issue erstellen (ueber die Links oben)
2. Admin wird automatisch benachrichtigt
3. Admin antwortet mit `/approve` (auch per E-Mail-Reply moeglich)
4. GitHub Action fuehrt die Aktion automatisch aus
5. Issue wird automatisch geschlossen

### Regeln

- **Alle Repos sind private** — es gibt keine Ausnahmen
- Members koennen keine Repos direkt erstellen — alles ueber Tickets
- Neue Accounts: `@house-of-communication.com` E-Mail empfohlen
- Bestehende GitHub Accounts koennen verwendet werden
- SSH Keys sind Pflicht fuer git-Operationen

## Skills

- **[Journey-Skills Marketplace](https://github.com/plan-net-journey/Journey-Skills/blob/main/MARKETPLACE.md)** — 52 kundenuebergreifende Skills
- **Kunden-Repos** — Projektspezifische Skills leben im jeweiligen Repo

## Admin Setup

Admins muessen einmalig einen Personal Access Token (classic) mit `admin:org` und `repo` Scope als Repository Secret `ORG_ADMIN_TOKEN` hinterlegen.

1. [Token erstellen](https://github.com/settings/tokens/new?scopes=admin:org,repo&description=PNJ-Org-Admin)
2. In diesem Repo: Settings → Secrets and variables → Actions → New repository secret
3. Name: `ORG_ADMIN_TOKEN`, Value: der erstellte Token
