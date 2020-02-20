**ionic 4 install**
`npm i -g ionic@4.12.0`

**app create**
`ionic start appodneve tabs --type=angular --cordova`

**--android build--**
`ionic cordova build android`

**Build fail után**
projekt mappán belül:
`sudo nano platforms/android/CordobaLib/cordova.gradle`

`getAndroidSdkDir()` funkció, kb 125. sorba beleírni az android sdk helyét, pl:
`"/home/stephenmiller/Android/Sdk/"`

**Fontos megjegyzés ionic 4 esetén!**
Az elérési út végére mindenképp kell a / jel, mert ez a vacak bénán concatolja és elkúrja, aztán azt írja nem találja, holott ott van csak hiányzik az elérési útból egy / jel.

**Legyenek meg az elérési utak is**
`sudo nano ~/.profile`
Fájl legvégére:
`export ANDROID_SDK_ROOT=/home/stephenmiller/Android/Sdk`
`export ANDROID_HOME=/home/stephenmiller/Android/Sdk`
Mentés után pedig:
`source ~/.profile`

**License hiba esetén:**
home mappádon belül:
`cd Android/Sdk/tools/bin`
`./sdkmanager --licenses`
Elfogadod mindet.