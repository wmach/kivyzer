# Dockerコンテナで Kivy / Buildozer で Android APK を作成

- 前提条件
  - docker desktop をインストール後、起動
  - kivy で作成したアプリのソースコードがある

## githubからクローンする

```
C:\Users\${you}\docker-kivyzer> git clone https://github.com/wmach/kivyzer.git
```

## kivyzer Docker コンテナイメージをビルドする

```
C:\Users\${you}\docker-kivyzer> docker-compose build
```

## Kivy のソースファイルを hostcwd にコピーする

 - 参考にkivyのデモアプリのソースを全部コピー

```
kivy_venv\share\kivy-examples\demo\showcase\data
kivy_venv\share\kivy-examples\demo\showcase\android.txt
kivy_venv\share\kivy-examples\demo\showcase\main.py
kivy_venv\share\kivy-examples\demo\showcase\showcase.kv
```

## Buildozer で初期化して *.spec ファイルを作成する

 - 入力
```
C:\Users\${you}\docker-kivyzer\hostcwd> docker-compose run kivyzer init
```

 - 出力
```
[+] Building 0.0s (0/0)
[+] Building 0.0s (0/0)
File buildozer.spec created, ready to customize!
```

## Buildozer でデバッグ用の APK を作成する

 - 入力
```
C:\Users\${you}\docker-kivyzer\hostcwd> docker-compose run kivyzer android debug
```

 - 出力
```
[+] Building 0.0s (0/0)
[+] Building 0.0s (0/0)
# Check configuration tokens
# Ensure build layout
# Create directory /home/kivyzer/.buildozer
# Create directory /home/kivyzer/.buildozer/cache
# Create directory /home/kivyzer/hostcwd/.buildozer
# Create directory /home/kivyzer/hostcwd/.buildozer/applibs
# Create directory /home/kivyzer/.buildozer/android/platform/android/platform
# Create directory /home/kivyzer/hostcwd/.buildozer/android/platform
# Create directory /home/kivyzer/hostcwd/.buildozer/android/app
... （略）

```

## Buildozer の質問に 'Y' と返答する

 - 入出力

```
January 16, 2019
---------------------------------------
Accept? (y/N): y
[=======================================] 100% Unzipping... platform-tools/lib64
# Run ['/home/kivyzer/.buildozer/android/platform/android-sdk/tools/bin/sdkmanager', '--sdk_root=/home/kivyzer/.buildozer/android/platform/android-sdk', '--update']
# Cwd /home/kivyzer/.buildozer/android/platform/android-sdk
[=======================================] 100% Computing updates...
# Updating SDK build tools if necessary
# build-tools folder not found /home/kivyzer/.buildozer/android/platform/android-sdk/build-tools
# Run ['/home/kivyzer/.buildozer/android/platform/android-sdk/tools/bin/sdkmanager', '--sdk_root=/home/kivyzer/.buildozer/android/platform/android-sdk', '--list']
# Cwd /home/kivyzer/.buildozer/android/platform/android-sdk
...（略）

```

## エラー発生時

 - ２回目以降のビルドでは次のエラー発生する

```
[ERROR]:   Build failed: Requested API target 31 is not available, install it with the SDK android tool.
# Command failed: ['/usr/bin/python3', '-m', 'pythonforandroid.toolchain', 'create', '--dist_name=myapp', '--bootstrap=sdl2', '--requirements=python3,kivy', '--arch=arm64-v8a', '--arch=armeabi-v7a', '--copy-libs', '--color=always', '--storage-dir=/home/kivyzer/hostcwd/.buildozer/android/platform/build-arm64-v8a_armeabi-v7a', '--ndk-api=21', '--ignore-setup-py', '--debug']
# ENVIRONMENT:
...（略）

```

 - hostcwd\.buildozer\ ディレクトリを削除

```
C:\Users\${you}\docker-kivyzer\hostcwd> rmdir /s .buildozer
```

 - 再度ビルド

```
C:\Users\${you}\docker-kivyzer\hostcwd> docker-compose run kivyzer android debug
```













