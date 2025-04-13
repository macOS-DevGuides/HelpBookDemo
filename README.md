# 🧭 macOS Help Book Example (macOS 15+)

Apple's Help mechanism allows you to embed documentation in your app with virtually no additional code.  However, the internal sytems are very finnicky and rather poorly documented.  Apple's own [documentation for Help Books](https://developer.apple.com/library/archive/documentation/Carbon/Conceptual/ProvidingUserAssitAppleHelp/authoring_help/authoring_help_book.html) has not been updated since 2013, but the concepts still apply in this SwiftUI application developed for macOS 15 in 2025.

There are two main challenges related to using Apple's Help system:
- The system relies heavily on hidden and undocumented registration mechanisms, which are tightly linked to bundle identifiers;  changing a bundle id after a help book is registered can lead to unexpected behaviour that may be difficult to diagnose and rectify.
- The Help system is very stubborn with caching of Help content.  There are several undocumented cache locations that must be manually cleared for new Help content to display properly.

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
