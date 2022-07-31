# How to trust your private CA

## OS
 
### Ubuntu
`cp cacert.pem /usr/local/share/ca-certificates/cacert.crt && update-ca-certificates`

### Windows

### Android

Depends on version. Search in setting for "ca" or "certificate" and follow the instructions.

German Android 11:
Sicherheit & Sperrbildschirm > Verschlüsselung und Anmedeldedaten > CA-Zertifikat

## Apps

### Firefox

1. Settings > Privacy & Security
2. Under "Security" click "View Certifcates"
3. Choose "Authorities"
4. Click "Import"

### Firefox for Android

1. Navigate to settings > About Firefox
2. Tap logo seven time
3. Go back
4. Enter new menu item "Secret Settings"
5. Tap "Use third party CA certificates"
