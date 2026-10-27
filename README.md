# AryaAI — Android App (Phone Control Edition)

**Created by Shailesh Arya**

Ye wala version Vercel website nahi hai — ye asli Android app hai jo phone ke andar se kaam karta hai.

## Kya-kya kar sakta hai

### Seedha (permission + confirmation ke saath)
- **Call:** “Rahul ko call karo” — contact ka naam ya number se, pehle confirmation dialog, phir direct call (CALL_PHONE permission se)
- **App kholo:** “Chrome kholo”, “Maps kholo”, “Settings kholo”, “WhatsApp kholo”

### Accessibility ON karne par (Settings > Accessibility > AryaAI Automation)
- **WhatsApp:** “Rahul ko WhatsApp par hello bhejo” — chat khul kar message pre-fill hoga, auto-send ON ho to Send par tap bhi hoga
- **YouTube:** “YouTube par Arijit gana chalao” — search khulega, auto-tap ON ho to pehla video par tap hoga
- **Settings option on/off:** “Bluetooth on karo”, “Dark mode on karo”, “WiFi band karo” — us option ki settings khul kar uske switch par tap ki koshish hogi, switch ki current state check karke (ON wale ko dobara OFF nahi karega)

### System intent se (100% reliable)
- **Alarm:** “Subah 6 baje alarm set kar do” — phone ke Clock app me alarm set hoga (skip-UI try hota hai, na ho to alarm screen khul jayegi)
- **Settings kholo:** “Setting mein jao” — poori Settings khul jayegi

## Kya 100% guarantee nahi hai — sach

- **Net (WiFi/Data) band karna:** Android 10+ me koi normal app WiFi/Data ko seedha on/off nahi kar sakti. Is app me WiFi settings khul kar switch par tap ki koshish hoti hai. Har phone ka Settings UI alag hota hai (Samsung, Xiaomi, Vivo alag), isliye kisi phone par switch tap na bhi lage to panel khul jayega aur tum ek tap me band kar sakte ho. Mobile Data ka switch har phone me alag jagah hota hai, isliye uske liye Internet panel khulta hai.
- **WhatsApp auto-send / YouTube auto-play:** Ye WhatsApp aur YouTube ke UI par depend hai. Agar un apps ka UI badal gaya to tap lagna band ho sakta hai — tab app sirf chat/search kholegi, tum ek tap me send/play kar dena.
- **Root / Shizuku ke bina** isse zyada koi app guarantee nahi de sakti. Jo app aisa dawa kare, wo ya to root maang rahi hai ya jhooth bol rahi hai.

## Kaise build karo — bina Android Studio download kiye (GitHub Actions, kam data)

Android Studio ~1.5 GB ka hota hai. Agar data kam hai to ye tareeka use karo — tumhara sirf 25 KB ka source upload hoga, aur APK (~3-5 MB) download hoga:

1. GitHub.com par login karo → New repository banao, naam `aryaai-android`, **Public** rakho (isme koi key/password nahi hai).
2. Repo me Add file → Upload files → is zip ke andar ki saari files/folders upload karo (zip file khud nahi, uske andar ka content). `.github/workflows/build-apk.yml` file bhi upload honi chahiye.
3. Commit karo. Thodi der me repo ke **Actions** tab me “Build AryaAI APK” chalne lagega (2-5 minute).
4. Actions → us run ko kholo → neeche **Artifacts** me `AryaAI-APK` dikhega → usko download karo. Andar `app-debug.apk` milegi.
5. APK ko Vivo phone me bhejkar install karo (Install from unknown sources allow karna padega, kyunki ye tumhari apni app hai, Play Store ki nahi).

Note: Phone ke browser se poora folder upload karna mushkil ho sakta hai — kisi bhi PC (dost ka, cyber cafe ka) ke browser se upload kar lo, Android Studio install karne ki zaroorat nahi hai.

## Kaise build karo (Android Studio, agar PC par data ho to)

1. Android Studio kholo → Open → ye `aryaai-android` folder chuno.
2. Gradle sync hone do (internet chahiye, pehli baar 2-5 minute lag sakte hain).
3. Phone ko USB debugging ON karke connect karo, ya emulator chalao.
4. Run dabao → phone me AryaAI install ho jayegi.
5. App kholo → permissions do: Mic, Call, Contacts, Notifications.
6. Settings button → manual contacts jodo (jaise `Rahul = 9876543210`), auto-WhatsApp / auto-YouTube ON karo.
7. Accessibility: Settings button se Accessibility Settings kholo → AryaAI Automation ko ON karo. Bina iske auto-tap nahi chalega.

## Test karne ke commands

- “Time batao”
- “Rahul ko call karo” (pehle Settings me manual contact jodo ya phone contact me naam ho)
- “Rahul ko WhatsApp par hello bhejo”
- “YouTube par Arijit gana chalao”
- “WiFi band karo”
- “Bluetooth on karo”
- “Setting mein jao”
- “Subah 6 baje alarm set kar do”
- “Chrome kholo”

## Files

```
app/src/main/AndroidManifest.xml     Permissions + Accessibility service declare
app/src/main/java/.../MainActivity.kt        Voice UI, SpeechRecognizer, TTS, confirmation
app/src/main/java/.../CommandHandler.kt      Saare voice commands (call, WhatsApp, YouTube, net, settings, alarm, app)
app/src/main/java/.../AryaAccessibilityService.kt  Auto-tap: WhatsApp send, YouTube video, Settings switch
app/src/main/java/.../ContactManager.kt      Phone contacts + manual contacts
app/src/main/java/.../NetworkHelper.kt       WiFi/Data panel + switch attempt
app/src/main/java/.../Prefs.kt               Local settings + pending action
app/src/main/java/.../ReminderReceiver.kt    Reminder notification
app/src/main/java/.../SettingsActivity.kt    Contacts / auto-tap settings screen
```

## Agla step (agar chaho)

- Shizuku version: Shizuku app + ek baar PC/adb setup ke baad `svc wifi disable` jaise shell commands se net toggle zyada reliable ho sakta hai — par har phone par alag setup chahiye.
- Root version: sab kuch ho sakta hai, par root se phone ki security aur warranty risk me aati hai, isliye recommend nahi hai.
