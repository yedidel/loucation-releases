# Loucation — הפצת APK ועדכונים אוטומטיים

ריפו **ציבורי** נפרד שמכיל את קובצי ה-APK ואת מניפסטי הגרסה. האפליקציה מורידה מכאן
עדכונים דרך קישור **raw** ישיר, וה-`git push` הוא שמעלה את ה-APK (אין צורך ב-gh CLI).

## מבנה
```
apk/loucation-<ver>.apk          ← APK של release
apk/loucation-<ver>-debug.apk    ← APK של debug
version.json                     ← מניפסט לערוץ RELEASE
version-debug.json               ← מניפסט לערוץ DEBUG
```

## הוצאת גרסה — פקודה אחת (מתוך שורש הפרויקט)
ראשית עדכנו `versionCode`/`versionName` ב-`app/build.gradle.kts`, ואז:
```powershell
# release (ברירת מחדל) — דורש app/keystore.properties
./scripts/release.ps1 -VersionName "1.1.0" -VersionCode 2 -Notes "מה חדש"

# ערוץ debug
./scripts/release.ps1 -BuildType debug -VersionName "1.1.0" -VersionCode 2 -Notes "בדיקה"
```
הסקריפט בונה את ה-APK, **מעתיק אותו ל-`releases/apk/`**, מעדכן את המניפסט המתאים עם קישור ה-raw,
ודוחף את שני הריפו. ה-`push` של ריפו זה מעלה את ה-APK — האפליקציה תזהה את הגרסה ותתקין.

## הקמה חד-פעמית
1. ערכו `$GitHubUser` ב-`scripts/release.ps1`.
2. צרו ריפו ציבורי `loucation-releases`, וחברו remote מתוך `releases/`:
   ```powershell
   cd releases
   git remote add origin https://github.com/<USER>/loucation-releases.git
   git push -u origin main
   ```
3. ודאו ש-`UPDATE_MANIFEST_URL` ב-`app/build.gradle.kts` מצביע ל-raw של המניפסט (debug/release בנפרד).

> הערה: GitHub חוסם קבצים מעל 100MB. ה-APK (~28MB) בטווח בטוח.
