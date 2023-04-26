# Geolocation
# cd Geolocation
# npm install
# ionic serve
# npx cap add android
# npm run build
# npx cap sync
# npx cap open android
## luego que abra android estudio esperar que instale las dependencias, buscar la opcion Build, luego Build blunde apk y build apk

# Dependencias que se instalo
## npm install @capacitor/android
## npm install @capacitor/geolocation
### cofigurar AndroidManifest.xml
### ruta ==>> android\app\src\main\AndroidManifest.xml
### <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
### <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
### <uses-feature android:name="android.hardware.location.gps" />