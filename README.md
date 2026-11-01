# App de Prueba Web - Whale OS / Android TV

Herramienta minimalista de validación visual y de rendimiento para Smart TVs (Android TV, Google TV, Whale OS / Zeasn, webOS, Tizen).

---

## 🚀 Cómo probar en local o red local

Para servirla a un televisor en la misma red Wi-Fi/LAN:

```bash
# Con Python 3
python3 -m http.server 8080

# O con Node.js npx
npx serve .
```

Abre en el navegador del Smart TV: `http://<IP-DE-TU-PC>:8080`.

---

## 📦 Pautas para empaquetar en WebView / APK Nativo

### Opción 1: Capacitor (Recomendada y más rápida)

1. **Instalar Capacitor:**
   ```bash
   npm init -y
   npm install @capacitor/core @capacitor/cli @capacitor/android
   npx cap init "TVTest" "com.test.tvapp" --web-dir .
   npx cap add android
   ```

2. **Configurar Pantalla Completa y Soporte TV en Android:**
   En `android/app/src/main/AndroidManifest.xml`:
   - Agrega compatibilidad para TV:
     ```xml
     <uses-feature android:name="android.software.leanback" android:required="false" />
     <uses-feature android:name="android.hardware.touchscreen" android:required="false" />
     ```
   - En la `<activity>` principal:
     ```xml
     android:theme="@android:style/Theme.NoTitleBar.Fullscreen"
     android:screenOrientation="landscape"
     ```

3. **Compilar APK:**
   ```bash
   npx cap open android
   # Y compilar APK desde Android Studio (Build > Build Bundle(s) / APK(s) > Build APK(s))
   ```

---

### Opción 2: Android Studio Nativo (WebView básico)

1. Crea un proyecto en Android Studio con una `Empty Activity`.
2. En `res/layout/activity_main.xml`, coloca únicamente un `WebView`:
   ```xml
   <WebView
       android:id="@+id/tvWebView"
       android:layout_width="match_parent"
       android:layout_height="match_parent" />
   ```
3. En `MainActivity.java` o `MainActivity.kt`:
   ```kotlin
   val webView = findViewById<WebView>(R.id.tvWebView)
   webView.settings.javaScriptEnabled = true
   webView.settings.domStorageEnabled = true
   webView.settings.useWideViewPort = true
   webView.settings.loadWithOverviewMode = true

   // Cargar desde assets locales o URL remota
   webView.loadUrl("file:///android_asset/index.html")
   ```
4. Copia `index.html` en la carpeta `app/src/main/assets/`.
