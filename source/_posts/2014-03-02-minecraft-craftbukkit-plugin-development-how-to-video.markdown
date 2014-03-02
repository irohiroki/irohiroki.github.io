---
layout: post
title: "Minecraft(CraftBukkit)プラグイン作り方動画"
date: 2014-03-02 08:50:05 +0900
comments: true
categories: 
- Minecraft
- CraftBukkit
- plugin
- video
---

[Minecraftプラグインを書いて動かすまで](http://irohiroki.github.io/blog/2014/02/18/craftbukkit-plugin-chicken-launcher/)で紹介した手順のうち、分かりにくいところをgifアニメにしました。

### 1-2.のCraftBukkit起動

Windows 7で、run.batという起動用バッチファイルを作るところ。

![run.bat作成](/images/make-run_bat.gif)

run.batでCraftBukkitを起動するところ。途中でセキュリティに関する確認があります。

![CraftBukkit起動](/images/exec-run_bat.gif)

### 1-3.のCraftBukkit接続

CraftBukkitは本物のMinecraftよりバージョンが低いので、Minecraftクライアントのバージョンを合わせる必要があります。その手順。

![Minecraftのバージョン変更](/images/change-minecraft-client-version.gif)

サーバを追加して接続するところ。

![CraftBukkitサーバに接続](/images/add-craftbukkit-server.gif)

### 2.のJDKのセットアップ

最大の難関、WindowsでJavaのbinをPATHに追加する手順。

![JavaのbinフォルダをPATH環境変数に追加](/images/add-java-bin-to-path.gif)

### 3.のプラグイン作成

ソースコードを入れるsrcフォルダを作るところ。デスクトップに`server¥ChickenLauncher¥src`という階層で作ります。

![srcフォルダを作る](/images/make-src-dir.gif)

なお、Windowsでは下のスクリプトを`server¥ChickenLauncher¥build.bat`として作っておくと捗りそうです。

	set NAME=ChickenLauncher
	set SERVER_DIR=%USERPROFILE%\Desktop\server

	mkdir bin
	mkdir dist
	javac src\*.java -d bin -classpath %SERVER_DIR%\craftbukkit.jar -sourcepath src
	jar -cf dist\%NAME%.jar *.yml -C bin .
	copy dist\%NAME%.jar %SERVER_DIR%\plugins
	pause
