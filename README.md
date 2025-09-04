# batocera-service-sunshine-flatpak

A drop‑in **Batocera service** that auto‑installs/updates Sunshine (Flatpak) and runs it headless with working audio. Put this script in `/userdata/system/services/` and enable it from Batocera’s Services menu.

Repo script path:

```
https://github.com/natemac/batocera-service-sunshine-flatpak/blob/main/sunshine
```

> Tested on Batocera 42 (x86\_64).

---

## What this service does

* Ensures **Flatpak** and **Flathub** are available
* **Installs Sunshine** if missing; **updates it** on every start
* Exports `PULSE_SERVER=unix:/run/pulse/native`
* **Auto‑detects active Pulse sink** (prefers one in `RUNNING`, else default) and exports `PULSE_SINK`
* Runs Sunshine in background and logs to `/userdata/system/logs/sunshine.log`

---

## Installation Methods

### Option 1: SSH Install

1. **SSH into Batocera** (user: `root`).
2. **Download the service** to the services folder and make it executable:

   ```bash
   mkdir -p /userdata/system/services
   curl -fsSL https://raw.githubusercontent.com/natemac/batocera-service-sunshine-flatpak/main/sunshine -o /userdata/system/services/sunshine
   chmod +x /userdata/system/services/sunshine
   ```
3. **Start it once** to trigger install/update and first‑run setup:

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
2. Run the same commands as above to download and chmod the service script.
3. Enable it in **Services** so it runs at boot.

---

### Option 3: SMB File Transfer

1. From another computer on the same network, open the Batocera SMB share (usually `\\BATOCERA\` on Windows or `smb://batocera/` on Linux/Mac).
2. Navigate to:

   ```
   \\BATOCERA\\share\\system\\services\\
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

   (Accept the self‑signed cert the first time.)
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

### From the Services system

Run:

```bash
batocera-services stop sunshine
batocera-services disable sunshine
```

Then remove the service file:

```bash
rm -f /userdata/system/services/sunshine
```

Finally, uninstall Sunshine itself:

```bash
flatpak uninstall -y dev.lizardbyte.app.Sunshine
```

> Note: Running `/userdata/system/services/sunshine uninstall` directly can fail with **Permission denied** if the script doesn’t have the `+x` bit. Use the above commands instead to fully remove Sunshine and the service script.

---

## Troubleshooting

* **No audio on the client**: ensure the service is exporting `PULSE_SERVER` and that a sink is `RUNNING`. Try toggling Moonlight’s audio codec (Opus/AAC) if needed.
* **Can’t reach web UI**: confirm service is running and browse to `https://<batocera-ip>:47990/`. Accept the certificate warning.
* **Updates take long** on first run: Flatpak will download the Sunshine runtime and app; subsequent starts are fast.

---

## License

MIT (or your preferred license).
