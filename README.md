tion: F.R.I.D.A.Y. – WREX Edition Android 
name: Build FRIDAY APK

on:
  workflow_dispatch:
  push:
    paths:
      - 'FRIDAY_WREX_Cloud_Build.zip'
      - '.github/workflows/build.yml'

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.9'

      - name: Extract project
        run: |
          mkdir source
          unzip FRIDAY_WREX_Cloud_Build.zip -d source

      - name: Build APK
        run: gradle -p source :app:assembleDebug

      - uses: actions/upload-artifact@v4
        with:
          name: FRIDAY-WREX-APK
          path: source/app/build/outputs/apk/debug/app-debug.apk
          
