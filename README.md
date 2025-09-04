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

## Quick start (SSH)

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

   On first run, it will install Sunshine via Flatpak. Watch the log for progress.
4. **Enable autostart** in Batocera:

   * EmulationStation → **Main Menu → System Settings → Services → Sunshine → Enabled**

---

## Quick start (XTerm on Batocera)

1. Press **F1** to open the file manager, then **Applications → XTerm**.
2. Run the same commands:

   ```bash
   mkdir -p /userdata/system/services
   curl -fsSL https://raw.githubusercontent.com/natemac/batocera-service-sunshine-flatpak/main/sunshine -o /userdata/system/services/sunshine
   chmod +x /userdata/system/services/sunshine
   batocera-services restart sunshine
   ```
3. Enable it in **Services** so it runs at boot.

---

## Pair Moonlight

1. Open Sunshine’s web UI on another device:

   ```
   https://<batocera-ip>:47990/
   ```

   (Accept the self‑signed cert the first time.)
2. Set a username/password if prompted.
3. In **Audio/Video**, make sure **Audio Sink** is empty (default) so the service’s `PULSE_SINK` takes effect. (It auto‑selects the `RUNNING` sink.)
4. On your TV/phone, open **Moonlight**, select the host, enter the PIN in Sunshine’s UI.

## Audio notes

* The service sets `PULSE_SERVER=unix:/run/pulse/native`.
* It will select a sink automatically; to verify on Batocera:

  ```bash
  pactl list short sinks
  pactl info | grep 'Default Sink'
  ```
* You’ll see the chosen device in the log:

  ```
  Using Pulse sink: alsa_output.pci-0000_04_00.1.HiFi__HDMI2__sink
  ```
* If you ever move HDMI ports, just restart the service and it will re‑detect.

---

## Useful commands

```bash
batocera-services status sunshine
batocera-services restart sunshine
cat /userdata/system/logs/sunshine.log | tail -n 200
```

---

## Uninstall

```bash
batocera-services stop sunshine
/userdata/system/services/sunshine uninstall   # removes Sunshine Flatpak
rm -f /userdata/system/services/sunshine
```

(You can also remove Sunshine from Flatpak manually.)

---

## Troubleshooting

* **No audio on the client**: ensure the service is exporting `PULSE_SERVER` and that a sink is `RUNNING`. Try toggling Moonlight’s audio codec (Opus/AAC) if needed.
* **Can’t reach web UI**: confirm service is running and browse to `https://<batocera-ip>:47990/`. Accept the certificate warning.
* **Updates take long** on first run: Flatpak will download the Sunshine runtime and app; subsequent starts are fast.

---

## License

MIT (or your preferred license).
