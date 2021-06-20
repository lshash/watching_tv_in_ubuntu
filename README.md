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
少なくともus3poutに関しては下記のスペックで余裕だ。\
cpu
>Intel Pentium B940, 2.0GHz

memory
>2GB


## ------ハードの確認
1.\
lsusb\
ｻﾝﾊﾟｸﾝ(it does mean us3pout)を認識用にudev\
lsusb

511/
045

@ubu20だとudevでゴニャゴニャやる必要はなかった。  
@か、ﾊｰﾄﾞの事情か？・・わからぬが。

刺したら、すぐ出た。
lsusb

@実は、色々必要  
@詳細は再生方法ｾｸｼｮﾝ  


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

CardEstablishContext: Service not available.  



chijou tv pay tvの文字を確認  
カードの裏表が逆だと  
I cannot confirm

@カードリーダーの代わりにus3poutを使うことにより、カードリーダー買わなくてよくしようと思ったが、
失敗。  
@カードリーダを使わない場合、復号化に失敗するが、データ自体は作成可能。
後程b25で復号することも。



## ーーーーーー主要ソフトインストール


---------------------------------------------------this is main func.
rec fsusb2 n

https://github.com/epgdatacapbon/recfsusb2n  
上記のﾛｹｰｼｮﾝにexecutableはない  

readmeに書いてある通り、  
sudo apt install gcc  
sudo apt install make  
srcフォルダでmake  

executableフォウ  

なにやらwarningやらが結構でるが、executableはできており、  
./recfsus~でいける  


--------------------------------------------c'est aussi un materiau tre important.  
b25

sudo apt-get install git make gcc g++ pkg-config libpcsclite-dev pcscd  

git clone https://github.com/epgdatacapbon/libaribb25  
cd libaribb25  
make  
sudo make install



----------------------------------------------------------------------

## ------デバイス紐付



１個ずつ、見ていこうか・・・・・・

---------------------------------------------------------
lsusb

Bus 001 Device 004: ID 0511:0045 N'Able (DataBook) Technologies, Inc. 



n'able databookが511になっていなければならない
いや、すでになっているが・・





-------------------------------------------------------------------
video groupがなければ、作成し、
そこに自分のユーザーを入れると。

compgen -g

group videoはすでに存在だが、


sudo apt install members
members video

だれもいない
自分のユーザーは入っていない


あとでいれる
------------------------------------



tuner ruleとやらに、
なんか入れると、なるらしい（video groupとusb deviceの紐づけか・・・・・・）


０５１１，0029となっているが、
私の場合は、
0511, 0045
とする。
subsystem, devtype, modeは変えんで良い





---------------------------------------------------




/lib/udev/rules.d/  
/etc/udev/rules.d/  

のどちらに入れるかは、  



{ouch}:/etc/udev$  
{ouch}:/etc/udev$ dir  
hwdb.d	rules.d  udev.conf  
{ouch}:/etc/udev$ cd rules.d/  
{ouch}:/etc/udev/rules.d$ dir  
70-persistent-net.rules  70-snap.snapd.rules  
70-snap.chromium.rules	 91-tuner.rules  
{ouch}:/etc/udev/rules.d$  
{ouch}:/etc/udev/rules.d$  
{ouch}:/etc/udev/rules.d$  
{ouch}:/etc/udev/rules.d$ cat 91-tuner.rules  
SUBSYSTEM=="usb", ENV{DEVTYPE}=="usb_device", ATTRS{idVendor}=="0511", ATTRS{idProduct}=="0045", MODE="0664", GROUP="video"  
{ouch}:/etc/udev/rules.d$  


過去の私の端末の実績値から、  

etc udev
を採用する。

ここに、つまりusbdevice= sampaqun　とvideo group

の紐づけを行うということだ。


自分のﾕｰｻﾞｰはvideo group
に追加するので

OKということになる。




$ sudo gpasswd -a {OUCH!} video //add user to Group called video

これで、video group
members video
が０件だったのが、
自分のみが加わることになる。
ﾌｫｳ





$ sudo vi /etc/udev/rules.d/90-sampa_loader.rules // file add and edit.

SUBSYSTEM=="usb", ENV{DEVTYPE}=="usb_device", ATTRS{idVendor}=="0511", ATTRS{idProduct}=="0045", MODE="0664", GROUP="video"


i for edit mode
ctl shift v for paste

esc for command mode

:
x
enter


------------how to del line

esc to command mode
dd
enter


----------------------
reboot needed!




----------------------
ﾌｫﾌｫｳ！
device can't be opend.  
は解消。  
しかし、
/recfsusb2n: unrecognized option '--b25'

おいーツ


recfsusb2n 0.1.13 beta 20170122_0026, (GPL) 2016 trinity19683
Usage: recfsusb2n [-v] [--b25] [--dev devfile] [--tsid n] [--sid n1,n2,...] channel recsec destfile


recfsusb2n 0.1.13 beta 20170122_0026, (GPL) 2016 trinity19683
Usage: ./recfsusb2n [-v] [--dev devfile] [--tsid n] [--sid n1,n2,...] channel recsec destfile

fofofou, どうやら、　ｵﾌﾟｼｮﾝが
はいっていないかのようですねぇ・・・・・・

入れ直し
make clean

make B25=1

at last, 
sudo make install

ですかな？


----------------------------------------------------------------------


sudo apt-get install g++
sudo apt-get install libboost-thread-dev libboost-filesystem-dev

boostはよくわからんが、
入れないで
fuuum
となったら、
入れて、
fuuuuuum
となる予定。







## ------再生方法
typical error.


./recfsusb2n: unrecognized option '--b25'  
can't open usbdevfile to read/write '/dev/bus/usb/001/004', ERR=13  
No device can be used.

fou, デバイス紐付がなされていないな、さては。





------------------------------------------------
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


Mpeg２TS→Mpeg4へトランスコードすれば容量削減　するらしいな・・


録画しながら、見る
というのが
やはり
デバイス２つ
ないと
できないのか
、
いや
なにかあるはずだが、

イマイチわからぬ


recfsusb2n --b25 101 30 ./x1.ts  
recfsusb2n --b25 101 30 ./x2.ts  
recfsusb2n --b25 101 30 ./x3.ts  
recfsusb2n --b25 101 30 ./x4.ts  
recfsusb2n --b25 101 30 ./x5.ts  

例えばこういう
バッチをつくったら、
どうなるのか


はて。

再生する側も

x1.tsが保存されたら、２秒後ぐらいで別スレッドから

vlc ./x1.ts  

x1が再生されおわる頃には
x2が保存され、x3作成中なわけだから、


vlc ./x2.ts  

x2が再生されおわる頃には
x3が保存され、x4作成中なわけだから、


vlc ./x3.ts  
vlc ./x4.ts  
vlc ./x5.ts  

こんな感じかなー

std in fd/0
をあとから保存したいのぉー（無理）（なのか？）




