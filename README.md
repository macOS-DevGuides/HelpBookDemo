# 🧭 macOS Help Book Example (macOS 15+)

This project demonstrates:
- A working `.help` bundle integrated into an Xcode app
- Smart cache-clearing logic that only removes Help Book cache when files change
- Insights into Tips.app (macOS 15 Help Viewer) behavior, registration, and debugging

## 🛠️ How to Use

1. Open the Xcode project (to be created) and build the app.
2. Go to `Help > HelpBookDemo Help` in the menu bar.
3. Modify the Help Book HTML and rebuild — the smart script detects changes and clears stale cache.

## 🔄 Cache Management Script

Included in:
```
Scripts/help_cache_smart_clean.sh
```

This script:
- Computes a hash of `.html` and `.plist` files in the `.help` bundle
- Only clears cache when files have changed
- Bypasses fragile timestamp- or dependency-based mechanisms

## 📘 Resources

- Cached Help Books:
  ```
  ~/Library/Group Containers/group.com.apple.helpviewer.content/Library/Caches/
  ```

- View registered Help Books:
  ```bash
  plutil -p ~/Library/Preferences/com.apple.help.plist
  ```

- Generate index:
  ```bash
  hiutil -I corespotlight -Caf HelpBookDemo.cshelpindex HelpBookDemo.help/Contents/Resources/
  ```

- Purge cache:
  ```bash
  hiutil -P
  killall -9 helpd Tips
  ```

## 📎 License

MIT
