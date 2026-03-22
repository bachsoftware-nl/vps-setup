# VPS Setup Script

> *"Een nieuwe server is als een lege koelkast: veelbelovend, maar totaal nutteloos."*

Dit script zet een kale Ubuntu VPS om in een werkende machine waar je ook daadwerkelijk iets mee kunt. Geen gedoe, geen handmatig `apt install` tikken terwijl je halfslaperig bent. Gewoon één commando en klaar.

## Installatie

```bash
sudo bash <(curl -fsSL https://bachsoftware.nl/vibecoding)
```

Ja, het heet `/vibecoding`. Nee, we schamen ons er niet voor.

## Wat krijg je

| Tool | Waarom |
|------|--------|
| **fish shell** | Omdat bash uit 1989 komt en het laat merken |
| **starship** | Een mooie prompt zodat je tenminste trots kunt zijn op iets |
| **tmux** | Sessies die overleven als je verbinding wegvalt (en dat gaat gebeuren) |
| **fzf** | Fuzzy finder, want typen is voor mensen zonder deadlines |
| **Node.js 18** | Omdat Claude Code er nou eenmaal op draait |
| **Claude Code** | De enige AI-assistent die ook echt commits schrijft |
| **Docker Engine** | Want containers zijn gewoon betere servers in een doos |
| **Traefik v3** | Reverse proxy met automatisch HTTPS voor elk project |

## Traefik — de centrale verkeersregelaar

Tijdens de installatie vraagt het script of je Traefik wilt instellen. Als je dat doet:

1. Geef je domein op (bijv. `jouwdomein.nl`)
2. Stel twee DNS-records in bij jouw domeinbeheerder:

| Type | Naam | Waarde |
|------|------|--------|
| A | `jouwdomein.nl` | `<server IP>` |
| A (wildcard) | `*.jouwdomein.nl` | `<server IP>` |

Het wildcard record zorgt dat elk nieuw project direct bereikbaar is via `projectnaam.jouwdomein.nl` — zonder extra DNS-wijzigingen.

Traefik regelt automatisch HTTPS via Let's Encrypt voor elk project dat je aanmaakt.

## Handige commando's na installatie

| Commando | Wat het doet |
|----------|-------------|
| `cc <naam>` | Start Claude in een tmux sessie |
| `ccb <prompt>` | Start Claude met een initiële taak, bedenkt zelf een sessienaam |
| `ai` | Overzicht van alle draaiende Claude instanties |
| `tl` | Alle tmux sessies, met icoontjes en alles |
| `tc <naam>` | Nieuwe tmux sessie |
| `tk <naam>` | Kill een tmux sessie (gebruik met gevoel) |
| `t <naam>` | Spring naar een sessie |
| `new-project <naam> [poort]` | Nieuw project scaffolden met Traefik docker-compose |
| `traefik-status` | Traefik status en actieve containers op proxy netwerk |
| `server-update` | `apt update && apt upgrade`, maar dan met minder karakter |
| `kill-zombies` | Jaagt zombie-processen naar het hiernamaals |
| **Alt+K** | Interactieve command-kiezer via fzf |

## Nieuw project starten

```bash
# Scaffold een nieuw project (maakt ~/projects/mijnapp/ aan)
new-project mijnapp

# Of met een specifieke containerpoort
new-project mijnapp 8080

# Pas de docker-compose.yml aan, dan starten:
cd ~/projects/mijnapp
docker compose up -d
```

Het project is direct bereikbaar op `https://mijnapp.jouwdomein.nl` — Traefik pikt het automatisch op en haalt een Let's Encrypt certificaat.

## Wat het script doet

1. Installeert alle tools hierboven
2. Maakt een `deploy` user aan als die er nog niet is (of past de bestaande aan)
3. Installeert Docker Engine en voegt de deploy user toe aan de docker groep
4. Zet fish als standaard shell
5. Schrijft alle functies naar `~/.config/fish/functions/`
6. Voegt een welkomstscherm toe met server stats en een dagelijkse quote
7. Updatet de MOTD zodat je bij het inloggen meteen ziet welke Claude sessies er draaien
8. Loopt de Traefik wizard (optioneel, maar aanbevolen)

## Na installatie

```bash
# Log in als deploy
su - deploy

# Authenticeer bij Anthropic (eerste keer)
claude

# Start je eerste Claude sessie
cc mijn-project

# Of maak direct een nieuw project aan
new-project mijnapp
```

## Variabelen

Wil je een andere gebruikersnaam dan `deploy`? Dat kan:

```bash
sudo DEPLOY_USER=bart bash <(curl -fsSL https://bachsoftware.nl/vibecoding)
```

## Aanbevolen VPS

Voor een tientje per maand heb je een prima VPS waar dit allemaal op past. Wij draaien op een **Strato VPS** — goedkoop, stabiel, datacenters in Duitsland.

👉 **[Strato VPS — vanaf ~€10/maand](https://www.strato.nl/server/vps/)**

Kies de kleinste Linux VPS met Ubuntu 22.04 of 24.04. Dat is genoeg voor Traefik + meerdere projecten + Claude Code.

## Vereisten

- Ubuntu (getest op 22.04 en 24.04)
- Root toegang
- Een domein met beheer over de DNS
- Een API key van Anthropic
- Koffie (niet vereist, maar sterk aanbevolen)

---

*Gemaakt door [BachSoftware](https://bachsoftware.nl) — wij zetten servers op zodat jij dat niet hoeft.*
