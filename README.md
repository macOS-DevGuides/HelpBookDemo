# 🧭 macOS Help Book Example (macOS 15+)

Apple's Help mechanism allows you to embed documentation in your app with virtually no additional code.  Unfortunately, the internal sytems are very finnicky and rather poorly documented.  Apple's own [documentation for Help Books](https://developer.apple.com/library/archive/documentation/Carbon/Conceptual/ProvidingUserAssitAppleHelp/authoring_help/authoring_help_book.html) has not been updated since 2013, but the concepts still apply in this SwiftUI application developed for macOS 15 in 2025.

There are two main challenges related to using Apple's Help system:
- The system relies heavily on hidden and undocumented registration mechanisms, which are tightly linked to bundle identifiers;  changing a bundle id after a help book is registered can lead to unexpected behaviour that may be difficult to diagnose and rectify.
- The Help system is very stubborn with caching of Help content.  The user help cache is linked to the app bundle version, and so should not normally be an issue for end users who install an actual app update.  However, this can pose problems during development as the cached content will always supersede changes to content bundled in the app *unless* the version number is changed *or* the cache is cleared.  

This project demonstrates:
- A working `.help` bundle integrated into an Xcode app
- A cache-clearning build script that prevents the help viewer (Tips.app) from showing stale content while editing help content during development
- Insights into Tips.app (macOS 15 Help Viewer) behavior, registration, and debugging

## 🛠️ How to Use

1. Open the Xcode project (to be created) and build the app.
2. Go to `Help > HelpBookDemo Help` in the menu bar.
3. Modify the Help Book HTML and rebuild — the build script will clear the cache and you should see your new content.

## 🔄 Cache Management Script

Included the **Clear Help Cache** Build Phase for the main target.

This script:
- Is set to watch index.html in the help bundle (you can add other files)
- Will clear the help cache when index.html changes
- Will always clear the help cache on a clean build

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
  sudo hiutil -P
  killall -9 helpd Tips
  ```

The command "sudo hiutil -P" seems to be the only way to clear stubborn cache conflicts that may arise if the application bundle ID is modified during development.

## 📎 License

MIT
