# ｼﾞｬﾊﾟﾆｰｽﾞﾃﾚﾋﾞｼﾞｮﾝ視聴方法（ubuntu bionic or later.）
ﾃﾚﾋﾞｼﾞｮﾝを視聴するためにはどうすればいいのか。


1. *US-3POUT
2. *カードリーダー

どうしても出費が必要なのは、
上記のみでしょう。
これ以上抑えるのは、
不可能に近い。
ubuntuは抑えられるかなと思ったのだが。
ちなみに他チューナー製品などで、cpuのスペックはCorei3以上と、ゲーミングPC並を要求だのとのことで危惧していたが
少なくともus3poutに関しては下記のスペックで余裕です。\
cpu
>Intel Pentium B940, 2.0GHz

memory
>2GB


## ------ハードの確認
1.\
lsusb\
ｻﾝﾊﾟｸﾝ(us3poutの別名)を認識用にudev\
lsusb

511/
045

@ubu20だとudevでゴニャゴニャやる必要はなかった。
@か、ﾊｰﾄﾞの事情か？・・わからぬが。

刺したら、すぐ出た。
lsusb


2.\
周辺ソフトインストール\
pcsc scanなど

@ubu20だとpcsc-toolsはuniverseなので、

sudo add-apt-repository universe

しないと
unable to locate pacman.

@omg, universeはすでに指定されているとのこと。
しかしなおも。
unable to locate pacman.
フォウ・・・・・・

debからやる。
archetectureは、

Intel x86-based	-> i386	 	 
AMD64 & Intel 64	-> amd64	 	 
ARM with hardware FPU	-> armhf	multiplatform	generic multiplatform for LPAE-capable systems	generic-lpae
64bit ARM	-> arm64	 	 
IBM POWER Systems	-> ppc64el	IBM POWER8 and newer machines	 
IBM z/Architecture	-> arm64

@なぜ、arm64　２つあるのだ、　おい！
@amd64ですね。

debから入れましょう。

@これ、普通にapt install pcscd でいけたのか・・？
pcscdも必要。
otherwise,
error fufufu.


chijou tv pay tvの文字を確認
カードの裏表が逆だと
I cannot confirm

@カードリーダーの代わりにus3poutを使うことにより、カードリーダー買わなくてよくしようと思ったが、
失敗。
@カードリーダを使わない場合、復号化に失敗するが、データ自体は作成可能。
後程b25で復号することも。



## ーーーーーー主要ソフトインストール

b25

---------------------------------------------------this is main func.
rec fsusb2 n

https://github.com/epgdatacapbon/recfsusb2n

sudo apt install gcc
sudo apt install make
srcフォルダでmake

executable

上記のﾛｹｰｼｮﾝにexecutableはない


--------------------------------------------




## ------再生方法

nhk
>recfsusb2n --b25 27 - - | vlc --quiet -

nhk bs
>recfsusb2n --b25 101 - - | vlc --quiet -


保存後に再生でもいいが、単に視聴するのみであれば
上記で問題ない。
locationは/dev/stdinなので終了後に削除されていると思われる。
単に見るだけであれば、ばかでかいサイズのファイルをディスクに書き込む必要はなかろう。


## ------録画
ハイフンのところに、秒数、ファイル名。後は煮るなり焼くなり可能となる。
