# batocera-service-sunshine-flatpak

A simple **Batocera service** that installs and runs Sunshine (via Flatpak). Drop the script into `/userdata/system/services/`, enable it in Batocera’s Services menu, and you’re ready to stream.

Repo script path:

```
https://github.com/natemac/batocera-service-sunshine-flatpak/blob/main/sunshine
```

---

## Install

1a. Download the sunshine service and place the file in the /share/system/services/ folder. (Skip to Step 3)
1b. SSH into Batocera (`root` user).
2. Download the service to the services folder:

   ```bash
   curl -fsSL https://raw.githubusercontent.com/natemac/batocera-service-sunshine-flatpak/main/sunshine -o /userdata/system/services/sunshine
   ```
3. Enable it in Batocera:

   * EmulationStation → **Main Menu → System Settings → Services → Sunshine → Enabled**

---

## Access Sunshine

1. On another device, open:

   ```
   https://<batocera-ip>:47990/
   ```

   (accept the self-signed certificate)
2. Set a username/password if prompted.
3. Pair with Moonlight on your client device.

---

## Uninstall

To stop and fully remove Sunshine **including its settings**:

```bash
batocera-services stop sunshine
rm -f /userdata/system/services/sunshine
flatpak --system uninstall -y dev.lizardbyte.app.Sunshine
rm -rf /userdata/system/.var/app/dev.lizardbyte.app.Sunshine
```

On next start, Sunshine will behave like a fresh install and ask you to set a new username/password.

---

## Notes

* Bug: You may need to set the option in your Moonlight or equivalient App to play audio through PC to have the audio play on your device.
* Audio sink is detected automatically. If you switch HDMI ports, restart the service.
* Logs are stored at `/userdata/system/logs/sunshine.log`.
