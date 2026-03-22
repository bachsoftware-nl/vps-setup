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
| `server-update` | `apt update && apt upgrade`, maar dan met minder karakter |
| `kill-zombies` | Jaagt zombie-processen naar het hiernamaals |
| **Alt+K** | Interactieve command-kiezer via fzf |

## Wat het script doet

1. Installeert alle tools hierboven
2. Maakt een `deploy` user aan als die er nog niet is (of past de bestaande aan)
3. Zet fish als standaard shell
4. Schrijft alle functies naar `~/.config/fish/functions/`
5. Voegt een welkomstscherm toe met server stats en een dagelijkse quote
6. Updatet de MOTD zodat je bij het inloggen meteen ziet welke Claude sessies er draaien

## Na installatie

```bash
# Log in als deploy
su - deploy

# Authenticeer bij Anthropic (eerste keer)
claude

# Start je eerste project
cc mijn-project
```

## Variabelen

Wil je een andere gebruikersnaam dan `deploy`? Dat kan:

```bash
sudo DEPLOY_USER=bart bash <(curl -fsSL https://bachsoftware.nl/vibecoding)
```

## Vereisten

- Ubuntu (getest op 22.04 en 24.04)
- Root toegang
- Een API key van Anthropic
- Koffie (niet vereist, maar sterk aanbevolen)

---

*Gemaakt door [BachSoftware](https://bachsoftware.nl) — wij zetten servers op zodat jij dat niet hoeft.*
