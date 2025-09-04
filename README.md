# batocera-service-sunshine-flatpak

A simple **Batocera service** that installs and runs Sunshine (via Flatpak). Drop the script into `/userdata/system/services/`, enable it in Batocera’s Services menu, and you’re ready to stream.

Repo script path:

```
https://github.com/natemac/batocera-service-sunshine-flatpak/blob/main/sunshine
```

---

## Install

1. SSH into Batocera (`root` user).
2. Download the service and make it executable:

   ```bash
   mkdir -p /userdata/system/services
   curl -fsSL https://raw.githubusercontent.com/natemac/batocera-service-sunshine-flatpak/main/sunshine -o /userdata/system/services/sunshine
   chmod +x /userdata/system/services/sunshine
   ```
3. Start it once:

   ```bash
   batocera-services restart sunshine
   ```
4. Enable it in Batocera:

   * EmulationStation → **Main Menu → System Settings → Services → Sunshine → Enabled**

---

## Access Sunshine

1. On another device, open:

   ```
   https://<batocera-ip>:47990/
   ```

   (accept the self‑signed certificate)
2. Set a username/password if prompted.
3. Pair with Moonlight on your client device.

---

## Uninstall

```bash
batocera-services stop sunshine
rm -f /userdata/system/services/sunshine
flatpak --system uninstall -y dev.lizardbyte.app.Sunshine
```

---

## Notes

* Audio sink is detected automatically. If you switch HDMI ports, restart the service.
* Logs are stored at `/userdata/system/logs/sunshine.log`.
