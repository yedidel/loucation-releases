# Loucation — הפצת APK ועדכונים אוטומטיים

האפליקציה אינה בחנות. ההפצה והעדכונים מתבצעים דרך **GitHub Releases**.

## הקמה חד-פעמית
1. צרו ריפו ציבורי, למשל `loucation-releases`.
2. ב-`app/build.gradle.kts` עדכנו את `UPDATE_MANIFEST_URL` ל-Raw URL של `version.json`, למשל:
   `https://raw.githubusercontent.com/<user>/loucation-releases/main/version.json`
3. העלו לריפו את הקובץ `version.json` (ראו דוגמה כאן).

## תהליך הוצאת גרסה חדשה
1. העלו את `versionCode` ו-`versionName` ב-`app/build.gradle.kts`, בנו APK חתום (ראו README הראשי).
2. צרו Release חדש ב-GitHub (תג כמו `v1.1.0`) והעלו אליו את ה-APK.
3. עדכנו את `version.json`:
   - `versionCode` — חייב להיות גבוה מהקודם.
   - `apkUrl` — קישור ההורדה הישיר ל-APK מתוך ה-Release.
   - `mandatory` — `true` אם העדכון הכרחי (יחסום שימוש עד התקנה).
4. דחפו את `version.json` ל-main.

האפליקציה בודקת את `version.json` בעת הפעלה וכל 12 שעות (WorkManager), ומציעה התקנה אוטומטית.
