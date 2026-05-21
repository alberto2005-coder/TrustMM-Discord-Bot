# 🚀 TrustMM - Bot de Discord de Escrow / Intermediario Automático de Litecoin (LTC)

> **IDIOMA / LANGUAGE**: [Español (ES)](#español) | [English (EN)](#english)

---

## Español

**TrustMM** es un bot de Discord seguro y totalmente automatizado para actuar como intermediario o **Escrow / Middleman** en transacciones peer-to-peer (P2P) de **Litecoin (LTC)**. Elimina la necesidad de intermediarios manuales en las transacciones de criptomonedas mediante el uso de monederos temporales ("hot-wallets") y el monitoreo de transacciones entrantes directamente en la blockchain en tiempo real.

---

## 📋 Características

- **Creación Automatizada de Tickets**: Creación instantánea de canales de texto privados dedicados exclusivamente a cada transacción entre Comprador y Vendedor.
- **Selección de Roles Dinámica**: Interfaz intuitiva con botones interactivos para definir quién actúa como Comprador y quién como Vendedor.
- **Modales Interactivos Seguros**: Transición de entradas por mensajes de texto (`wait_for`) a modales emergentes interactivos integrados en Discord para configurar detalles del trato y capturar direcciones de monedero de manera limpia y sin interferencias en el chat.
- **Soporte de Divisas Personalizables**: Permite al vendedor seleccionar su divisa fiat preferida (como USD, EUR, GBP, ARS, etc.) al configurar la transacción, obteniendo la tasa de conversión a LTC en tiempo real mediante la API de Coinbase.
- **Validación Avanzada de Direcciones LTC**: Validación automatizada mediante expresiones regulares (Regex) de direcciones Litecoin en mainnet (formatos Legacy `L...`, P2SH `M...` y Bech32 `ltc1...`) para evitar transacciones a direcciones inválidas o incorrectas.
- **Sanitización Robusta de Montos**: Capacidad de procesar y limpiar cantidades monetarias ingresadas por el usuario eliminando automáticamente espacios, símbolos de moneda ($, €, £, etc.) y estandarizando separadores decimales.
- **Monitoreo Automático de la Blockchain**: Consulta periódica a través de la API de Tatum para detectar transacciones pendientes y confirmadas en la blockchain de Litecoin de forma automatizada.
- **Gestión Segura de Fondos**: Permite liberar los fondos al vendedor (deduciendo comisiones de red estándar) o reembolsarlos al comprador mediante mutuo acuerdo.
- **Transcripciones HTML del Chat**: Generación automática de auditorías en HTML utilizando `chat-exporter` que son enviadas a los mensajes directos (DMs) de ambas partes al completarse o cancelarse la transacción.
- **Comandos Administrativos**: Panel de control y comandos Slash para administradores para gestionar perfiles, forzar liberaciones manuales de LTC y revisar estadísticas generales del servidor.

---

## 🔧 Requisitos Previos

- **Python 3.8 o superior**
- **Token de Bot de Discord** (con todos los Gateway Intents habilitados en el Discord Developer Portal).
- **API Key de Tatum** (para verificación de balances y envío/emisión de transacciones en la red Litecoin).

---

## ⚙️ Instrucciones de Configuración

### 1. Clonar el Repositorio y Navegar a la Carpeta

```bash
git clone <url-de-tu-repositorio>
cd <carpeta-del-repositorio>
```

### 2. Instalar las Dependencias

Instala las librerías de Python requeridas ejecutando:

```bash
pip install -r requirements.txt
```

### 3. Configurar el archivo `config.ini`

Crea o edita el archivo `config.ini` en la raíz del proyecto con tus credenciales y configuraciones:

```ini
[EmbedSettings]
embed_color = 0xFFFFFF
footer_text = TrustMM | Secure P2P Escrow Bot
icon_url = 
thumbnail_url = 
loading_url = https://www.icegif.com/wp-content/uploads/2023/07/icegif-1263.gif
ltc_image = https://w7.pngwing.com/pngs/786/58/png-transparent-coin-crypto-lite-litecion-ltc-crypto-currency-and-coin-icon.png
tick_image = https://e7.pngegg.com/pngimages/270/706/png-clipart-check-mark-computer-icons-green-tick-mark-angle-text-thumbnail.png

[Token]
TOKEN = TU_DISCORD_BOT_TOKEN_AQUÍ

[APIKeys]
TATUM_APIKEY = TU_TATUM_API_KEY_AQUÍ

[ChannelSettings]
mm_log_channel_id = TU_ID_DE_CANAL_DE_LOGS
category_id = TU_ID_DE_CATEGORÍA_DE_DISCORD

[RoleSettings]
admin_role_id = TU_ID_DE_ROL_DE_ADMINISTRADOR
client_role_id = TU_ID_DE_ROL_DE_CLIENTE

[DisplaySettings]
show_buyer_seller = True
```

### 4. Iniciar el Bot

Ejecuta el bot usando Python en tu terminal:

```bash
python main.py
```

---

## 🛠️ Comandos Slash (`/`)

- **/automm-panel**: Envía el panel embed interactivo de creación de transacciones (Solo Administradores).
- **/transcript**: Genera y envía en el chat una transcripción completa interactiva en HTML del canal actual.
- **/release**: Libera manualmente fondos en LTC de un Deal ID específico a una dirección externa (Solo Administradores).
- **/delete**: Elimina el canal de transacción actual de forma segura después de 5 segundos (Solo Administradores).
- **/profile**: Muestra el perfil y volumen de transacciones de un miembro (Volumen de USD comprado y vendido).
- **/statistics**: Muestra la cantidad total de transacciones completadas y el volumen total en USD en el servidor.
- **/about**: Muestra especificaciones técnicas del bot, tiempo en línea (uptime), uso de CPU/RAM e información de autoría.

---

## 📜 Licencia

Este proyecto está bajo la **LICENCIA DE SOFTWARE LIBRE CON ATRIBUCIÓN OBLIGATORIA (LICENCIA ALBERTO ORTIZ)**.

### Resumen de la Licencia

1. **Uso Gratuito**: Permitido para fines personales y no comerciales.
2. **Bifurcaciones (Forks) y Modificaciones**: Permitidos en plataformas públicas (como GitHub).
3. **Atribución Obligatoria**:
   - Se deben conservar todos los avisos de copyright y el nombre de **Alberto Ortiz** en el código fuente y las respuestas del bot.
   - Cualquier modificación o bifurcación debe declarar explícitamente al inicio de su documentación y en la interfaz:
     > *"Este Software es una bifurcación/modificación basada en el proyecto original 'TrustMM' desarrollado por Alberto Ortiz ([https://github.com/alberto2005-coder](https://github.com/alberto2005-coder))."*
4. **Prohibición Comercial**: Queda estrictamente prohibida la explotación comercial o venta de este Software o sus derivados sin consentimiento previo por escrito.

Para más detalles, consulta el archivo [LICENSE](LICENSE).

---
---

## English

**TrustMM** is a secure, fully automated peer-to-peer (P2P) **Litecoin (LTC) Escrow / Middleman** Discord bot. It eliminates the need for manual intermediaries in cryptocurrency transactions by using temporary hot-wallets and monitoring incoming transactions directly on the blockchain in real-time.

---

## 📋 Features

- **Automated Ticket/Channel Creation**: Secure, dedicated channels created instantly for Comprador/Vendedor (Buyer/Seller) deals.
- **Dynamic Role Selection**: Simple interface buttons for identifying each participant's role (Buyer vs. Seller).
- **Secure Interactive Modals**: Replaces chat-based text listeners (`wait_for` loops) with clean Discord pop-up modals for setting deal parameters and collecting payout wallet addresses.
- **Custom Fiat Currency Support**: Allows the user to specify their preferred fiat currency (e.g., USD, EUR, GBP, ARS) at transaction setup, querying the Coinbase API for real-time LTC conversion rates.
- **Advanced LTC Address Validation**: Automated Regex validation for mainnet Litecoin addresses supporting Legacy (`L...`), P2SH (`M...`), and Bech32 (`ltc1...`) formats to prevent transfer mistakes.
- **Robust Amount Sanitization**: Strips currency symbols ($, €, £, etc.), spaces, and formats decimal separators automatically from user input to ensure robust and error-free amount parsing.
- **Automatic Blockchain Monitoring**: Periodically tracks temporary wallets via Tatum API to detect pending and confirmed deposits.
- **Secure Funds Management**: Funds can be released to the seller (deducting network fees) or refunded back to the buyer through mutual agreement.
- **HTML Chat Transcripts**: Uses `chat-exporter` to auto-generate beautiful HTML audit logs of completed/cancelled transactions and DMs them to both parties.
- **Admin Control Commands**: Complete control panel for administrators to manage profiles, generate reports, force manual releases, and view server deal statistics.

---

## 🔧 Prerequisites

- **Python 3.8+**
- **Discord Bot Token** (with all gateway intents enabled on the Discord Developer Portal)
- **Tatum API Key** (for balance checks and transaction broadcasting)

---

## ⚙️ Setup Instructions

### 1. Clone & Navigate to Folder

```bash
git clone <your-repository-url>
cd <repository-folder>
```

### 2. Install Dependencies

Install all required Python packages using pip:

```bash
pip install -r requirements.txt
```

### 3. Configure `config.ini`

Create/update `config.ini` in the root folder with your Discord Bot Token and API Credentials:

```ini
[EmbedSettings]
embed_color = 0xFFFFFF
footer_text = TrustMM | Secure P2P Escrow Bot
icon_url = 
thumbnail_url = 
loading_url = https://www.icegif.com/wp-content/uploads/2023/07/icegif-1263.gif
ltc_image = https://w7.pngwing.com/pngs/786/58/png-transparent-coin-crypto-lite-litecion-ltc-crypto-currency-and-coin-icon.png
tick_image = https://e7.pngegg.com/pngimages/270/706/png-clipart-check-mark-computer-icons-green-tick-mark-angle-text-thumbnail.png

[Token]
TOKEN = YOUR_DISCORD_BOT_TOKEN_HERE

[APIKeys]
TATUM_APIKEY = YOUR_TATUM_API_KEY_HERE

[ChannelSettings]
mm_log_channel_id = YOUR_LOG_CHANNEL_ID
category_id = YOUR_CATEGORY_ID

[RoleSettings]
admin_role_id = YOUR_ADMIN_ROLE_ID
client_role_id = YOUR_CLIENT_ROLE_ID

[DisplaySettings]
show_buyer_seller = True
```

### 4. Run the Bot

Start the bot using Python:

```bash
python main.py
```

---

## 🛠️ Slash Commands

- **/automm-panel**: Displays the interactive setup embed panel (Admin only).
- **/transcript**: Generates and sends a complete HTML chat transcript of the current channel.
- **/release**: Manually release LTC for a specific deal ID to a custom address (Admin only).
- **/delete**: Safe-deletes the current transaction channel after 5 seconds (Admin only).
- **/profile**: Displays P2P statistics for a specific server member (Spent/Earned USD volume).
- **/statistics**: Shows total deals completed and overall USD volume in the server.
- **/about**: Displays technical specifications, uptime, CPU/RAM usage, and bot information.

---

## 📜 License

This project is licensed under the **OBLIGATORY ATTRIBUTION FREE SOFTWARE LICENSE (ALBERTO ORTIZ LICENSE)**.

### License Summary

1. **Free Use**: Permitted for personal and non-commercial purposes.
2. **Forks & Modifications**: Permitted on public development platforms (such as GitHub).
3. **Obligatory Attribution**:
   - All copyright notices and the name of **Alberto Ortiz** must remain intact in the source code and bot replies.
   - Any modification or fork must explicitly declare at the beginning of its documentation and interface:
     > *"This Software is a fork/modification based on the original 'TrustMM' project developed by Alberto Ortiz ([https://github.com/alberto2005-coder](https://github.com/alberto2005-coder))."*
4. **Commercial Prohibition**: Commercial sale, distribution, or economic exploitation of this Software or its derivatives is strictly prohibited without prior written consent.

For more details, see the [LICENSE](LICENSE) file.
