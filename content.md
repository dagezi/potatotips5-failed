# Googleの人から教えてもらった tips

[![Quipper](https://avatars2.githubusercontent.com/u/535481?s=140)](http://www.quipper.com/)

dagezi@{twitter, github, gmail.com}


# adb lolcat

![lolcat](lolcat.jpeg)



## logcatになるだけです

```
# adb lolcat
W/ActivityManager(  602): Unable to start service Intent { act=com.extscreen.service } U=0: not found
W/ActivityManager(  602): Unable to start service Intent { act=com.extscreen.service } U=0: not found
W/ActivityManager(  602): Duplicate finish request for ActivityRecord{663a00c0 u0 android/com.android.internal.app.ResolverActivity t96 f}
I/ActivityManager(  602): Process com.twitter.android (pid 20696) has died.
I/ActivityManager(  602): Process com.google.android.apps.plus (pid 20733) has died.
I/ActivityManager(  602): Process com.nianticproject.ingress (pid 23720) has died.
I/ActivityManager(  602): Process android.process.media (pid 20769) has died.
I/ActivityManager(  602): Process android.process.acore (pid 20809) has died.
D/MobileDataStateTracker(  602): default: setPolicyDataEnable(enabled=true)
I/ActivityManager(  602): Process com.google.android.keep (pid 21537) has died.
I/ActivityManager(  602): Process com.android.providers.calendar (pid 19267) has died.
I/PowerManagerService(  602): Going to sleep by user request...
I/ActivityManager(  602): Config changes=480 {1.0 440mcc10mnc ja_JP ldltr sw360dp w598dp h335dp 4
```
お試しを!


# ここから本題



# 強制アップデートさせてみた

[![Quipper](https://avatars2.githubusercontent.com/u/535481?s=140)](http://www.quipper.com/)

dagezi@{twitter, github, gmail.com}



## 自己紹介
SASAKI Takesi (佐々木毅史)
aka dagezi (大个子)

![Q](./qall.jpeg)

教育関係の会社で Androidエンジニアやってます。

Quipperは Android、iOS、Rubyエンジニアなど募集してます！



## Androidで専用端末

- 例:某社の教育用デバイス
- 配布後に updateしたい
- GooglePlay使えない
- Androidなダイアログだしたくない



## でも system権限とれる!



## AppDater作った

- Serviceとして起動
- 定期的に配布サイトチェック
 - 更新されていれば背後で取得
- 頃合いを見てインストール
 - プロンプトとか一切出ません! UIはアプリで制御可能
 - ユーザは拒否できない!



## インストーラの実際

![f](http://0.gravatar.com/avatar/93cff0b4ceeaeb1b0ef995ab9cc577e3?s=96&d=http%3A%2F%2F0.gravatar.com%2Favatar%2Fad516503a11cd5ca435acc9bb6523536%3Fs%3D48&r=G)
[Paul Nonaka](http://paulononaka.wordpress.com/2011/07/02/how-to-install-a-application-in-background-on-android/)氏ありがとう!

- Systemの certで署名する
- [INSTALL_PACKAGES](http://developer.android.com/reference/android/Manifest.permission.html#INSTALL_PACKAGES) 許可を付加
- [FakePackageManager](https://gist.github.com/dagezi/9501179) が Reflectionで PackageManagerの隠しメソッドを呼び出す



## うまく動いた!!

- 半年間で 1000人ほどに 18回アップデートしほぼ成功



## できてない点

- インストールのステータス取得
 - エラーとかがコールバックで帰ってこない！
 - 回避策: [PackageManager#getPackageInfo](http://developer.android.com/reference/android/content/pm/PackageManager.html#getPackageInfo%28java.lang.String%2c%20int%29) でうまく行ったかチェック
- インストール失敗時の復帰に弱い
- Systemの Certが必要
 - こればかりは現状では... rootを取ればできるかもしれないけど。



## 今後

- Openにしようと思った
 - deploy環境も含めて提供したい
- でもニッチすぎる
- 内部的にはこれからも使うよ
