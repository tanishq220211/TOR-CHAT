# tor_chat

A peer-to-peer, end-to-end encrypted terminal chat application routed entirely through the Tor anonymity network. No central servers, no registration, and no tracking.

---

## Architecture & Overview

* **True P2P:** Connections are established directly between two peers using Tor Onion Services. One user acts as a hidden listener (host), and the other connects directly via a Tor SOCKS proxy.
* **Dual Layer Security:** In addition to Tor's native onion routing encryption, an optional second layer of symmetric encryption (Fernet, via a 32-byte key derived using PBKDF2HMAC with 390,000 iterations) can be added by providing a shared passphrase.
* **Flexible UI:** Features a full Textual Terminal User Interface (TUI) with a split-pane view, dedicated system status log, and auto-scrolling chat history. Automatically falls back to a lightweight, pure CLI loop if dependencies aren't present.

---

## Requirements & Installation

This project requires a working installation of Python 3 and the Tor daemon on your machine.

### 1. Install System Dependencies (Linux - Ubuntu/Zorin)
First, ensure your package manager is up to date and install the native Tor service: 

sudo apt update
sudo apt install tor -y
2. Install Python Packages
Install the required application framework, proxy controller, and cryptographic primitives:

Bash
pip install stem pysocks cryptography textual
Configuration (Pre-Flight Check)
For the script to programmatically build onion tunnels, your Tor process must have its Control Port enabled. Choose one of the two methods below before launching the app:

Method A: The Quick Command Loop (Temporary)
Stop the system background daemon and launch Tor manually in a dedicated terminal window:

Bash
sudo systemctl stop tor
tor --SocksPort 9050 --ControlPort 9051 --CookieAuthentication 1
Keep this terminal window running in the background while chatting.

Method B: Configure System Service (Permanent)
If you prefer Tor to run silently in the background automatically, open the main configuration file:

Bash
sudo nano /etc/tor/torrc
Scroll to the bottom and paste the following lines:

Plaintext
ControlPort 9051
CookieAuthentication 1
Save and close (Ctrl+O, Enter, Ctrl+X), then restart the service:

Bash
sudo systemctl restart tor
Usage Guide
Run the main file using the positional subcommand required by the interface engine.

Hosting a Chat Room
To create a fresh .onion address and wait for your contact to join:

Bash
python3 torchatmadebythesmartestpersonalive.py host
Advanced Host Options:
Add a shared passphrase layer:

Bash
python3 torchatmadebythesmartestpersonalive.py host --key "your secret passphrase"
Make your onion address permanent across restarts:

Bash
python3 torchatmadebythesmartestpersonalive.py host --persist ~/.tor_chat_identity
Fallback to the bare input() text interface:

Bash
python3 torchatmadebythesmartestpersonalive.py host --plain
Connecting to a Peer
Once your contact provides you with their unique onion address link, open your terminal and dial out:

Bash
python3 torchatmadebythesmartestpersonalive.py connect <paste_onion_address_here.onion>
Advanced Connect Options:
Connect using the matching passphrase:

Bash
python3 torchatmadebythesmartestpersonalive.py connect <onion_address> --key "your secret passphrase"
