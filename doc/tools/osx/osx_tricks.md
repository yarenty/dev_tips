# Jail-break from not opening on OSX


1. After downloading, open Terminal

Remove quarantine attribute:
```bash
sudo xattr -r -d com.apple.quarantine /Applications/Nuclear.app
```

2. If still blocked:

```log

Go to System Preferences → Security & Privacy
Click "Open Anyway" next to Nuclear
```

3. For persistent issues:

```bash 
sudo spctl --master-disable
```