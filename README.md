# ubuntuでtvを見る

1*US-3POUT
2*カードリーダー

どうしても出費が必要なのは、
上記のみでしょう。
これ以上抑えるのは、
不可能に近い。
というか、ない、と思う。
ubuntuなのだからいけるかな
と思ったが、
無理でした。


-----ハードの確認
1.

lsusb
さんぱくんを認識用にudev
lsusb

511/
045


2.
周辺ソフトインストール
pcsc scan`など

chijou tv pay tvの文字を確認
カードの裏表が逆だと
I cannot confirm


ーーーー主要ソフトインストール

b25

rec fsusb2 n

さんぱくんについているリーダは
使えない模様である。
使い方があるのだろうが、謎。
使う、というのはカードリーダーの代わりにサンパを使うことにより、
カードリーダー買わなくて良いじゃんという意味。


カードリーダを使わない場合、復号化に失敗するが、データ自体は作成可能。
後程b25で復号することも。




