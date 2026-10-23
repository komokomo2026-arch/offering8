CHURCH OFFERING SYSTEM - VERSION WITH "OFFERING SYSTEM LOCK"
============================================================

WHAT CHANGED (nothing else was touched)
  index.html   adds the optional "Offering System Lock" switch and the lock screen
  Policy.html  one sentence explaining what the lock stores
  All other files are identical to the previous package.

HOW TO UPLOAD TO GITHUB PAGES
  1. Unzip this file on your phone or computer. (Do not upload the .zip itself.)
  2. Open your repository on GitHub -> Add file -> Upload files.
  3. Drag in ALL the unzipped files (index.html, manifest.json, service-worker.js,
     Terms.html, Policy.html and the 4 .png icons). Keep them together on the main
     page of the repository - no folders. README.txt is optional.
  4. Commit changes, wait 1-3 minutes, then open your website once while online.

WHERE THE SWITCH IS
  Phone: tap "More" -> "Offering System Lock" (directly above "Video Guide").
  Computer/tablet: in the footer buttons, directly above "Video Guide".
  (There is no button called "Offering System" in the project, so the switch was
  placed directly above Video Guide, after Backup.)

HOW TO TEST ON AN ANDROID PHONE (Chrome, or the installed app)
  0. Make sure the phone has a screen lock (Settings -> Security -> Screen lock)
     and that you have a recent Google Drive backup.
  1. Open the website from its https:// address (or the installed app).
  2. More -> Offering System Lock -> Continue. Android asks you to verify twice
     (fingerprint, PIN, pattern or password). The switch then shows ON.
  3. Press Home, then reopen the app: the lock screen appears and Android asks
     you to verify. Cancel it: the app stays locked. Verify: the app opens.
  4. Refresh the page / close and reopen the app: it starts locked.
  5. More -> Offering System Lock -> "Turn off" -> confirm. Choosing "Keep lock on"
     leaves it ON. The switch shows OFF after you confirm.
  6. Check Offerings, Reports, Backup and Restore still work after unlocking.

HOW IT WORKS / HONEST LIMITS
  * It uses the phone's own verification (WebAuthn, "user verification required").
    The app checks the signature, the challenge and the "user verified" flag.
    No PIN, pattern, password or fingerprint data ever reaches the app, and none
    is stored. Only a credential ID and a PUBLIC key are saved on the device.
  * It is an interface lock. It does NOT encrypt the records stored in the
    browser. A technically skilled person who can inspect or change the website
    code or browser storage could get around it.
  * It locks when the browser tells the app it was hidden, closed or sent to the
    background (visibilitychange, pagehide, freeze). Browsers/Android do not
    promise to send every event at the exact moment a process is killed, and the
    Android "recent apps" thumbnail may show the last screen. When the app
    restarts it always starts locked.
  * It needs the secure https:// address or the installed app. It will not turn
    on inside another page/app (iframe) and says so.
  * If your phone's screen lock or passkey stops working, there is deliberately no
    bypass. Clear the app's data (or reinstall), connect Google Drive and Restore
    your latest backup. Keep Drive backups up to date.

ANDROID WRAPPER APP (WebView) - NOT TESTED HERE
  WebView cannot use the phone's passkey screen. If you use your own Android app,
  add a JavascriptInterface named ChurchAppLockNative with:
      isAvailable(): Boolean                      (optional)
      authenticate(nonce: String, title: String)  (required)
  Show androidx.biometric BiometricPrompt with
  BIOMETRIC_STRONG or DEVICE_CREDENTIAL, then run in the WebView:
      window.__churchAppLockNativeResult({"ok":true,"nonce":"<same nonce>"})
  (ok:false when cancelled/failed). The app only accepts a reply that carries the
  nonce it just sent. This native part is not included in this package.
