---
layout: post
title: "おれのさぶらいむてきすとのRuby小技"
date: 2013-12-05 00:00:00 +0900
comments: true
categories:
- SublimeText
- Ruby
---
このエントリは[おれのさぶらいむてきすと Advent Calendar 2013](http://www.adventar.org/calendars/184)の5日目です。昨日は[@s12bt](https://twitter.com/s12bt)さんの[おれのさぶらいむてきすとは日本語検索がうまくできない](http://blog.obentoba.co/entry/2013/12/04/sublimetext-japaneseSearch)でした。

今日は[いろ](https://twitter.com/irohiroki)がRubyで使える小技を紹介します。

ご存知の通り、Rubyではブロックを以下の2つの形で書くことができます。
##### do end 

```ruby
foo do
  bar
end
```

##### ブレース

```ruby
foo {
  bar
}
```

ブレースの中が1行の時は、下のように書くことも多いと思います。

```ruby
foo { bar }
```

仕様上は`do end`の方が`{}`より評価の優先順位が低いだけですが、読んだ時の印象が違うため、使い分けてるRubyistも多いはずです。

そして、最初は`do end`で書いたけどやっぱり`{}`にしたいとか、`{}`で書いたけど`do end`にしたいことって意外とあるんですよね。

でも、実際にやるとちょっと面倒です。`do`まで行って`{`に直して、`end`まで行って`}`に直して。`{}`にしてみたら1行にしたくなって、改行も消したり。それなのに、後でやっぱり`do end`がよかった、なんてことになると本当に最悪です。

そこでおすすめしたいのが、[Ruby Block Converter](https://sublime.wbond.net/packages/Ruby%20Block%20Converter)というプラグイン。Package Controlでインストールしたら、変換したい`do`と`end`の間にカーソルを移動しましょう。

```ruby
foo do
  bar # このへんにカーソルを。
end
```

そして **ctrl + shift + [** すると!!!

```ruby
foo { bar }
```

こうなります!! すごい!!! 一瞬!!!!

もちろん逆もできます。それどころか、ブロック引数があってもできます。例えば

```ruby
array.each{|e| some_method(e) }
```

だったら、`{}`の間にカーソルを移動して **ctrl + shift + ]** !!!

```ruby
array.each do |e|
  some_method(e)
end
```

おおー!! 便利!!! インデントもちゃんと自分の設定に合わせてくれるんですね!!!

なお、`do end`から`{}`にしたとき1行になるのは、元が3行のときだけです。つまり、

```ruby
foo do
  bar
  baz
end
```

のように4行以上のときは

```ruby
foo {
  bar
  baz
}
```

になります。なんて頭がいいんでしょう……

もちろんSublime Textですから、カーソルが複数あっても大丈夫です!! 全てのカーソルの場所でブロックを変換してくれます!!!

Rubyを使う人は是非Ruby Block Converterをインストールして、この気持ちよさを味わってくださいね!!!

明日は[@kei_q](https://twitter.com/kei_q)さんです。よろしくお願いします！
