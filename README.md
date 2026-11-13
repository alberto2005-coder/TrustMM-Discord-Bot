# TrustMM — escrow automático de Litecoin en Discord

[![Python](https://img.shields.io/badge/Python-3.8%2B-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![discord.py](https://img.shields.io/badge/discord.py-2.x-5865F2?logo=discord&logoColor=white)](https://discordpy.readthedocs.io/)
[![Litecoin](https://img.shields.io/badge/Litecoin-LTC-A6A9AA?logo=litecoin&logoColor=white)](https://litecoin.org/)
[![GPL-3.0](https://img.shields.io/badge/Licencia-GPL--3.0-blue)](LICENSE)

[Español](#español) · [English](#english)

---

## Español

### Índice

- [Descripción](#descripción)
- [Cómo funciona un trato](#cómo-funciona-un-trato)
- [Características](#características)
- [Requisitos](#requisitos)
- [Instalación](#instalación)
- [Configuración](#configuración)
- [Comandos](#comandos)
- [Estructura del proyecto](#estructura-del-proyecto)
- [Seguridad y responsabilidad](#seguridad-y-responsabilidad)
- [Licencia](#licencia)

### Descripción

**TrustMM** es un bot de Discord que actúa como **intermediario (escrow / middleman)** en transacciones P2P de **Litecoin (LTC)**. El bot abre un canal privado por operación, genera una dirección de depósito temporal, vigila la blockchain y libera o devuelve los fondos cuando ambas partes confirman el trato. Con ello sustituye al intermediario humano por un flujo automatizado y auditable.

### Cómo funciona un trato

1. Un administrador publica el panel con `/automm-panel`.
2. Comprador y vendedor abren un ticket y eligen su rol con botones.
3. Cada uno completa un **modal** con los datos del trato y su dirección de cobro.
4. El vendedor fija el precio en su divisa; el bot consulta la tasa LTC/fiat en **Coinbase**.
5. El bot crea una **dirección LTC temporal** (Tatum) y muestra el importe y un QR de depósito.
6. El comprador envía los LTC; el bot consulta el saldo de la dirección periódicamente.
7. Con el depósito confirmado, el bot libera los fondos al vendedor (descontando la comisión de red) o los reembolsa al comprador si ambas partes acuerdan cancelar.
8. Al cerrar, genera una **transcripción HTML** y la envía por MD a las dos partes.

### Características

- **Tickets automáticos**: canal de texto privado por transacción.
- **Selección de rol** con botones (comprador / vendedor).
- **Modales de Discord** en lugar de esperar mensajes de texto.
- **Divisas fiat configurables** (USD, EUR, GBP, ARS…) con tasa en tiempo real.
- **Validación de direcciones LTC** mainnet: Legacy (`L...`), P2SH (`M...`) y Bech32 (`ltc1...`).
- **Saneamiento de importes**: quita símbolos de moneda y normaliza separadores decimales.
- **Seguimiento de la blockchain** vía API de Tatum, con depósitos pendientes y confirmados.
- **Liberación y reembolso** con deducción de la comisión de red.
- **Transcripciones HTML** enviadas por MD al completar o cancelar.
- **Comandos de administración** para perfiles, liberaciones manuales y estadísticas.

### Requisitos

- **Python 3.8** o superior.
- **Token de bot** de Discord con los *Gateway Intents* necesarios activados en el Developer Portal.
- **Clave de API de Tatum** para consultar saldos y emitir transacciones en Litecoin.
- Una categoría de Discord y un canal de logs.

### Instalación

```bash
git clone https://github.com/alberto2005-coder/TrustMM-Discord-Bot.git
cd TrustMM-Discord-Bot

python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS / Linux
source .venv/bin/activate

pip install -r requirements.txt
```

Dependencias: `discord.py`, `qrcode`, `psutil`, `chat-exporter` y `configparser`.

### Configuración

Copia `config.ini.example` a `config.ini` y rellena cada sección:

```ini
[Token]
TOKEN = TU_DISCORD_BOT_TOKEN

[APIKeys]
TATUM_APIKEY = TU_TATUM_API_KEY

[ChannelSettings]
mm_log_channel_id = ID_DEL_CANAL_DE_LOGS
category_id = ID_DE_LA_CATEGORIA

[RoleSettings]
admin_role_id = ID_DEL_ROL_ADMIN
client_role_id = ID_DEL_ROL_CLIENTE
```

`[EmbedSettings]` controla colores, textos e imágenes de los embeds; `[DisplaySettings]` decide si se muestran los roles de comprador y vendedor.

Después:

1. Activa los intents en el portal (incluido **Message Content** si tu versión de discord.py lo requiere).
2. Invita el bot con los scopes `bot` y `applications.commands`.
3. Arranca:

```bash
python main.py
```

4. Publica el panel con `/automm-panel` en el canal de admisión.

> [!IMPORTANT]
> `config.ini` contiene el token del bot y la clave de Tatum. No lo subas al repositorio; si se filtra, cualquiera puede mover fondos o controlar el bot. Rota ambas credenciales de inmediato.

### Comandos

| Comando | Quién | Qué hace |
|---|---|---|
| `/automm-panel` | Admin | Publica el panel de creación de tratos |
| `/transcript` | Todos | Genera y envía una transcripción HTML del canal actual |
| `/release` | Admin | Libera manualmente LTC de un *deal ID* a una dirección |
| `/delete` | Admin | Borra el canal de la transacción tras 5 segundos |
| `/profile` | Todos | Perfil y volumen (USD comprado/vendido) de un miembro |
| `/statistics` | Todos | Tratos completados y volumen total en USD del servidor |
| `/about` | Todos | Especificaciones, uptime y uso de CPU/RAM |

### Estructura del proyecto

```text
TrustMM-Discord-Bot/
├── main.py             # Bot, panel, modales, comandos y flujo del trato
├── utils.py            # Tatum, Coinbase, validación LTC y persistencia JSON
├── requirements.txt
├── config.ini.example  # Plantilla de configuración
├── .gitignore
├── LICENSE
└── README.md
```

### Seguridad y responsabilidad

- **Custodia de fondos**: durante un trato, los LTC están en una dirección controlada por el bot. Un fallo del código, del servidor o de las claves puede provocar **pérdida total**. Prueba primero con cantidades mínimas o en una red de prueba.
- **Claves y token**: guarda `config.ini` fuera del repositorio y con permisos restringidos. La clave de Tatum permite emitir transacciones.
- **Direcciones**: valida siempre la dirección de cobro con el vendedor por un canal aparte; el bot comprueba el formato, no la identidad de su dueño.
- **Legalidad**: operar como intermediario puede tener implicaciones fiscales, de blanqueo de capitales o de licencias en tu país. Consulta la normativa aplicable antes de usarlo con dinero real.
- **Sin garantías**: el proyecto se ofrece tal cual, sin soporte ni responsabilidad del autor por pérdidas derivadas de su uso.

### Licencia

**GNU General Public License v3.0 (GPL-3.0)** — consulta [LICENSE](LICENSE). Las copias o versiones modificadas deben distribuirse bajo la misma licencia y conservar la atribución a **Alberto Ortiz**.

---

## English

### Table of contents

- [Overview](#overview)
- [How a deal works](#how-a-deal-works)
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Configuration](#configuration)
- [Commands](#commands)
- [Project structure](#project-structure)
- [Security and responsibility](#security-and-responsibility)
- [License](#license)

### Overview

**TrustMM** is a Discord bot that acts as an **escrow / middleman** for peer-to-peer **Litecoin (LTC)** deals. It opens a private channel per trade, generates a temporary deposit address, watches the blockchain and releases or refunds the funds once both parties confirm. It replaces the human middleman with an automated, auditable flow.

### How a deal works

1. An admin publishes the panel with `/automm-panel`.
2. Buyer and seller open a ticket and pick their role with buttons.
3. Each fills in a **modal** with the deal details and their payout address.
4. The seller sets the price in their fiat currency; the bot fetches the LTC/fiat rate from **Coinbase**.
5. The bot creates a **temporary LTC address** (Tatum) and shows the amount plus a deposit QR code.
6. The buyer sends the LTC; the bot polls the address balance.
7. Once the deposit is confirmed, the bot releases the funds to the seller (minus the network fee) or refunds the buyer if both parties agree to cancel.
8. On closing, it generates an **HTML transcript** and DMs it to both parties.

### Features

- **Automatic tickets**: one private text channel per transaction.
- **Role selection** via buttons (buyer / seller).
- **Discord modals** instead of waiting for text messages.
- **Configurable fiat currencies** (USD, EUR, GBP, ARS…) with live rates.
- **LTC address validation** on mainnet: Legacy (`L...`), P2SH (`M...`) and Bech32 (`ltc1...`).
- **Amount sanitisation**: strips currency symbols and normalises decimal separators.
- **Blockchain tracking** through the Tatum API, covering pending and confirmed deposits.
- **Release and refund** with the network fee deducted.
- **HTML transcripts** DM'd on completion or cancellation.
- **Admin commands** for profiles, manual releases and statistics.

### Requirements

- **Python 3.8** or later.
- A **Discord bot token** with the required *Gateway Intents* enabled in the Developer Portal.
- A **Tatum API key** to check balances and broadcast Litecoin transactions.
- One Discord category and one log channel.

### Installation

```bash
git clone https://github.com/alberto2005-coder/TrustMM-Discord-Bot.git
cd TrustMM-Discord-Bot

python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS / Linux
source .venv/bin/activate

pip install -r requirements.txt
```

Dependencies: `discord.py`, `qrcode`, `psutil`, `chat-exporter` and `configparser`.

### Configuration

Copy `config.ini.example` to `config.ini` and fill in each section:

```ini
[Token]
TOKEN = YOUR_DISCORD_BOT_TOKEN

[APIKeys]
TATUM_APIKEY = YOUR_TATUM_API_KEY

[ChannelSettings]
mm_log_channel_id = YOUR_LOG_CHANNEL_ID
category_id = YOUR_CATEGORY_ID

[RoleSettings]
admin_role_id = YOUR_ADMIN_ROLE_ID
client_role_id = YOUR_CLIENT_ROLE_ID
```

`[EmbedSettings]` controls embed colours, text and images; `[DisplaySettings]` decides whether buyer and seller roles are shown.

Then:

1. Enable the intents in the portal (including **Message Content** if your discord.py version needs it).
2. Invite the bot with the `bot` and `applications.commands` scopes.
3. Start it:

```bash
python main.py
```

4. Publish the panel with `/automm-panel` in your intake channel.

> [!IMPORTANT]
> `config.ini` holds the bot token and the Tatum key. Never commit it; if it leaks, anyone can move funds or control the bot. Rotate both credentials immediately.

### Commands

| Command | Who | What it does |
|---|---|---|
| `/automm-panel` | Admin | Publishes the deal creation panel |
| `/transcript` | Everyone | Generates and sends an HTML transcript of the current channel |
| `/release` | Admin | Manually releases LTC for a deal ID to an address |
| `/delete` | Admin | Deletes the transaction channel after 5 seconds |
| `/profile` | Everyone | A member's profile and USD bought/sold volume |
| `/statistics` | Everyone | Completed deals and total USD volume for the server |
| `/about` | Everyone | Specs, uptime and CPU/RAM usage |

### Project structure

```text
TrustMM-Discord-Bot/
├── main.py             # Bot, panel, modals, commands and deal flow
├── utils.py            # Tatum, Coinbase, LTC validation and JSON storage
├── requirements.txt
├── config.ini.example  # Configuration template
├── .gitignore
├── LICENSE
└── README.md
```

### Security and responsibility

- **Fund custody**: during a deal, the LTC sits in an address controlled by the bot. A bug in the code, the host or the keys can cause **total loss**. Start with minimal amounts or a test network.
- **Keys and token**: keep `config.ini` outside the repository with restricted permissions. The Tatum key can broadcast transactions.
- **Addresses**: double-check the payout address with the seller over a separate channel; the bot validates the format, not the owner.
- **Legality**: running an escrow service may carry tax, anti-money-laundering or licensing obligations in your country. Check the applicable rules before using real funds.
- **No warranty**: the project is provided as-is, with no support and no liability for losses arising from its use.

### License

**GNU General Public License v3.0 (GPL-3.0)** — see [LICENSE](LICENSE). Copies or modified versions must be distributed under the same license and keep the attribution to **Alberto Ortiz**.
