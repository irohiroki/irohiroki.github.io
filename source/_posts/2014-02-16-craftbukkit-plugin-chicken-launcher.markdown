---
layout: post
title: "Minecraftプラグインを書いて動かすまで"
date: 2014-02-18 19:10:42 +0900
comments: true
categories:
- Minecraft
- plugin
---

世界中で大人気の[Minecraft](https://minecraft.net/)には[CraftBukkit](https://dl.bukkit.org/)というクローンがあります。CraftBukkitはオープンソースで、プラグイン向けのAPIも用意されているので、MinecraftそのものをクラックするいわゆるMODより安心して拡張できます。このエントリではCraftBukkitをセットアップし、自分で書いたプラグインをインストールして動かす流れを紹介します。

なお、PC版のMinecraftが動作していることを前提としています。

## 1. CraftBukkit

Minecraftのシステムはサーバとクライアントに分かれていますが、CraftBukkitはサーバのクローンです。まずはプラグインなしでそのまま動かしてみましょう。手順は[Setting up a server - BukkitWiki](http://wiki.bukkit.org/Setting_up_a_server)に書かれています。手順といっても、ダウンロードして実行するだけです。

### 1-1. ダウンロード

一般的なオープンソースのソフトウェアと同様に安定版と開発版がありますが、安定版は[CraftBukkit | Bukkit](https://dl.bukkit.org/downloads/craftbukkit/list/rb/)からダウンロードできます。バージョン番号はMinecraftのものと対応します。つまり、1.6.2のCraftBukkitを使うときは、Minecraft（クライアント）も1.6.2で起動しなければなりません（後述）。

### 1-2. 起動

ダウンロードしたら[Setting up a server - BukkitWiki](http://wiki.bukkit.org/Setting_up_a_server)に書かれてるjavaコマンドで起動しましょう。ここではダウンロードしたファイルを`~/Desktop/server/craftbukkit.jar`に改名したと仮定します。

	java -Xmx1024M -jar ~/Desktop/server/craftbukkit.jar -o true

数十行のメッセージの後にコマンドプロンプトが表示されて入力待ちになります。これでMinecraftクライアントから接続できるはずです。

### 1-3. 接続

MinecraftはProfileを編集することで起動するバージョンを選択できますが、<strong>CraftBukkitと同じバージョンを選ぶ</strong>のを忘れないでください。MinecraftLauncherの画面で「Edit Profile」ボタンを押して、「Use version」ドロップダウンリストの中から選べます。

Minecraftが起動したら「マルチプレイ」を選択し、「サーバー追加」を押します。「サーバーアドレス」に`localhost`と書いて「完了」を押せば、サーバ一覧にCraftBukkitサーバが現れるはずです。それを選択して「サーバーに接続」しましょう。

本物のMinecraftと同じようにゲームを始められれば大丈夫です。切断し、CraftBukkitのコマンドプロンプトで`stop`を実行して終了してください。

## 2. JDK

プラグインをコンパイルするためにJava SE Development Kitが必要です。[Java SE - Downloads](http://www.oracle.com/technetwork/java/javase/downloads/index.html)からダウンロード、インストールしてください。以下のようにjavacが実行できれば大丈夫です（実行結果は違うかもしれません）。

	$ javac -version
	javac 1.6.0_65

## 3. プラグインのコンパイルとインストール

いよいよプラグインをインストールします。ここでは私が実験的に作った[ChickenLauncher](https://github.com/irohiroki/ChickenLauncher)を使って説明します。

### 3-1. ダウンロード

[zipファイルをダウンロード](https://github.com/irohiroki/ChickenLauncher/archive/master.zip)できます。GitHubをご存知の方は、もちろん好きな方法でダウンロードして構いません。

### 3-2. コンパイル

コンパイル方法は[README](https://github.com/irohiroki/ChickenLauncher/blob/master/README.md)に書いてある通りです。つまり、下のようなjavacコマンドを実行します：

	javac src/*.java -d bin -classpath ~/Desktop/server/craftbukkit.jar -sourcepath src

`bin/chickenlauncher/ChickenLauncher.class`が生成されるはずです。プラグインとしてはこれをjarファイルにする必要があります。コマンドは下の通りです。

	jar -cf dist/ChickenLauncher.jar *.yml -C bin .

`dist/ChickenLauncher.jar`が生成されます。

### 3-3. インストール

CraftBukkitにはプラグイン用のディレクトリがあり、そこにjarファイルを入れれば完了です。プラグイン用のディレクトリはCraftBukkitを起動したときにcraftbukkit.jarと同じディレクトリに`plugins`という名前で生成されているはずです。前出の例だと、次のようになります。

	cp dist/ChickenLauncher.jar ~/Desktop/server/plugins

これで完了です。最初と同じようにCraftBukkitを起動し、羊毛を手に持って空中を左クリックしてみてください。羊毛からニワトリが飛び出すはずです。

<iframe width="560" height="315" src="//www.youtube.com/embed/IHxXTrZ6i-M" frameborder="0" allowfullscreen></iframe>

なお、ゲームをクリエイティブモードにするには、CraftBukkitのコンソールで下のコマンドを実行します。

	gamemode creative ユーザ名

## クライアントを改造したい？

以上のように、CraftBukkitを使うとMODに比べて安心してMinecraftを拡張できるのですが、変えられるのはサーバ側だけになります。つまり、新しいキーバインディングを設定したりアイテムの見た目を変更したりはできません。

でも落胆しないでください。CraftBukkitと対になるクライアント側のソフトとして、[SpoutCraft](http://spoutcraft.org/)があります。これはクライアントを拡張するMODですが、特徴的なのは拡張モジュールをサーバ側のCraftBukkitからロードすることです。つまり、サーバにプラグインを置くと、自動的にクライアントに配信・インストールされるのです。これでクライアント側を改造できるだけでなく、仲間内でサーバを立てて遊ぶときにとても便利になるでしょう。今後、SpoutCraftについても検証して紹介したいと思います。
