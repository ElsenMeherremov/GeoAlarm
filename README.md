name: Build
on: [push, workflow_dispatch]
permissions: {contents: write}
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: 17
      - uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: 8.7
      - run: unzip GeoAlarm.zip
      - run: cd GeoAlarm && gradle assembleDebug --no-daemon
      - run: gh release create build-${{ github.run_number }} GeoAlarm/app/build/outputs/apk/debug/app-debug.apk --title build-${{ github.run_number }} --notes apk
        env:
          GH_TOKEN: ${{ github.token }}
