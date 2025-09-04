# batocera-service-sunshine-flatpak (v2)

A drop-in **Batocera service** that auto-installs/updates Sunshine (Flatpak, system scope) and runs it headless with working audio. Put this script in `/userdata/system/services/` and enable it from Batocera’s Services menu.

Repo script path:

```
https://github.com/natemac/batocera-service-sunshine-flatpak/blob/main/sunshine
```

> Tested on Batocera 42 (x86\_64).

---

## What’s new in v2

* Forces installation into **system scope** to avoid “multiple installations” prompt.
* Removes any user-scope Sunshine install automatically to prevent conflicts.
* Still auto-installs/updates Sunshine each time service starts.
* Auto-detects Pulse sink (`RUNNING` > default) and sets `PULSE_SINK`.
* Uses `flatpak --system run` consistently.

---

## Installation Methods

### Option 1: SSH Install

1. **SSH into Batocera** (user: `root`).
2. **Download the v2 service** to the services folder and make it executable:

   ```bash
   mkdir -p /userdata/system/services
   curl -fsSL https://raw.githubusercontent.com/natemac/batocera-service-sunshine-flatpak/main/sunshine -o /userdata/system/services/sunshine
   chmod +x /userdata/system/services/sunshine
   ```
3. **Start it once** to trigger install/update and first-run setup:

   ```bash
   batocera-services restart sunshine
   batocera-services status sunshine
   tail -n 100 /userdata/system/logs/sunshine.log
   ```
4. **Enable autostart** in Batocera:

   * EmulationStation → **Main Menu → System Settings → Services → Sunshine → Enabled**

---

### Option 2: XTerm on Batocera

1. Press **F1** to open the file manager, then **Applications → XTerm**.
2. Run the same commands as above to download and chmod the v2 service script.
3. Enable it in **Services** so it runs at boot.

---

### Option 3: SMB File Transfer

1. From another computer on the same network, open the Batocera SMB share (usually `\\BATOCERA\` on Windows or `smb://batocera/` on Linux/Mac).
2. Navigate to:

   ```
   \\BATOCERA\share\system\services\
   ```
3. Copy the `sunshine` service file into this folder.
4. SSH into Batocera or use XTerm to make the script executable:

   ```bash
   chmod +x /userdata/system/services/sunshine
   ```
5. Restart the service:

   ```bash
   batocera-services restart sunshine
   ```
6. Enable autostart in Batocera’s Services menu.

---

## Pair Moonlight

1. Open Sunshine’s web UI on another device:

   ```
   https://<batocera-ip>:47990/
   ```

   (Accept the self-signed cert the first time.)
2. Set a username/password if prompted.
3. In **Audio/Video**, leave **Audio Sink** empty (default) so the service’s `PULSE_SINK` takes effect.
4. On your TV/phone, open **Moonlight**, select the host, enter the PIN in Sunshine’s UI.

---

## Useful commands

```bash
batocera-services status sunshine
batocera-services restart sunshine
cat /userdata/system/logs/sunshine.log | tail -n 200
```

---

## Uninstall

1. Stop the service:

   ```bash
   batocera-services stop sunshine
   ```
2. Remove the service file:

   ```bash
   rm -f /userdata/system/services/sunshine
   ```
3. Uninstall Sunshine (system + user scope just in case):

   ```bash
   flatpak --system uninstall -y dev.lizardbyte.app.Sunshine
   flatpak --user uninstall -y dev.lizardbyte.app.Sunshine || true
   ```

---

## Troubleshooting

* **Still seeing old errors about multiple installations**: ensure you’ve removed any user-scope install with `flatpak --user uninstall -y dev.lizardbyte.app.Sunshine`.
* **No audio on the client**: confirm the service log shows a valid `PULSE_SINK`. Restart the service after changing HDMI ports.
* **Can’t reach web UI**: confirm service is running and browse to `https://<batocera-ip>:47990/`.

---

## License

MIT (or your preferred license).
