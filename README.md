# .github/workflows/android.yml
name: Build Android APK
on: [push, workflow_dispatch]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-type: 18
      - run: npm install
      - run: npm run build
      - uses: actions/setup-java@v3
        with:
          distribution: 'zulu'
          java-version: '17'
      - name: Install Android SDK
        uses: android-actions/setup-android@v2
      - name: Build APK with Gradle
        run: |
          npx cap sync
          cd android && ./gradlew assembleDebug
      - name: Upload APK Artifact
        uses: actions/upload-artifact@v3
        with:
          name: app-debug.apk
          path: android/app/build/outputs/apk/debug/app-debug.apk
