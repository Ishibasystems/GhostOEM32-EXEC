========================================================================================
     GhostOEM32 EXEC 改 for SOTEC PX9512 VISTA Version 1.1 (2015/06/21) [VIA-1A-1]
                  　　　　　　　　　　　                           Copyright (C) 2010 HuIeji Utsuro
========================================================================================
				　　　　　　　　　　　           WARNING : FOR HAND OUT AND USE IN JAPAN ONLY.

*はじめに

  GhostOEM32 EXEC 改 をダウンロードいただき、誠に有難うございます。
  このプログラムは"SOTEC PX9512"のリカバリディスク作成の際、Windows AIK を使用して
  Windows PE に統合することによってリカバリオプションの選択・Windows の
  シャットダウンを行なうものです。

　※そもそも"SOTEC PX9512"は vista(OS) ディスク付いてない、リカバリディスク付いてない、
　リカバリは HDD から立ち上がるセットになってるんだけども、これってHDDお陀仏でアウト？

  ようするにリカバリディスクがついていない PC で、リカバリが HDD から行なうタイプの PC の
  リカバリディスクを Windows AIK と HSP で作ってしまおうという物です。

  統合前提のソース+プログラムですが改造によってはUSBからでも起動・リカバリ起動可能です。
  さらに改造することによって他のPCにも適合。他に Windows PE 上で使えるCUIメニュー画面など
  にもできます。  もちろん Windows PE は「何をする目的のOSか」を押さえとく前提で。

  Windows PE 2.0 、Windows PE 3.0 に対応確認。
  (32bitオンリー。これは hsp が32bitプログラムだから。Windows PE 64bit 上には
  WOW64 がないので動きません。いつになったら 64bit版hsp 出るんだ？ Windows 7 64bitの
  修復ディスク上(WOW64なし)とか需要あるのに。)


*開発環境

　このプログラムは
　　Microsoft Windows xp 32bit
　　Microsoft Windows 7 64bit
　　Microsoft Windows 8.1 64bit
　　onion software HSP script editer for Windows ver3.1
　　onion software HSP script editer for Windows ver3.4
　の環境で開発しました。


*動作環境

  Microsoft Windows PE 3.0 動作を確認。
  (日本語版32bit)


*ファイル構成

　px9512.exe 		実行ファイル
  px9512.hsp		ソースファイル
  read me.txt		このファイル

　このソフトはレジストリを一切使いません(まあ Windows PE 上で書き込んでも再起動時消えるんだけどね)。


*使い方

  リカバリ最小導入(SOTEC PX9512)

	注)HDD の HDREC パーティション内に GhostOEM32.exe と GDisk32.exe 、
	   vista.img vista.001 vista.002 data.img があります。
	   あらかじめ Ubuntu などでコピーして入手しておきます。

	まず Windows AIK (Windows自動インストールキット)をインストールします。
	http://www.microsoft.com/ja-jp/download/details.aspx?id=5753
	(2015/01/25確認)

	Windows 7 用ですが問題ありません。

	準備はこれだけですが、以降はディスク内容の更新ごとに毎回行ないます。

	①Windows Vista、Windows 7 用 GhostOEM32 EXEC 改 for SOTEC PX9512 VISTA 組み込み法

	・[スタート]→[すべてのプログラム]→[Microsoft Windows AIK]
		→[Deployment ツールのコマンド プロンプト] を起動。

	・Windows PE ビルド環境を作成します。
	  コマンドプロンプトに下記のコマンドを入力、[Enter]。

	  copype x86 C:\winpe

	  注)作成場所をC:\直下のC:\winpeにしています。すでに同じフォルダ名のフォルダがあると
	     エラーします。またhsp,ghost32系,gdisk32系は32bitですので"x86"以外動きません。

	・C:\winpe\winpe.wim をコピー、C:\winpe\boot.wim を作成します。

	・C:\winpe\boot.wim を C:\winpe\mount フォルダに展開します。
	  コマンドプロンプトに下記のコマンドを入力、[Enter]。

	  dism /mount-wim /wimfile:C:\winpe\boot.wim /index:1 /mountdir:C:\winpe\mount

	・C:\winpe\mount\Program Filesフォルダ内に tools フォルダを作成、その中に
	  px9512.exe と GhostOEM32.exe と GDisk32.exe をコピーします。

	  注)また再構成時にファイルが消されますので移動でなくコピー推奨。

	・C:\winpe\mount\Windows\System32フォルダ内の startnet.cmd を右クリック→[編集(E)]。
	  下記のコマンドを最後の行に追加し保存します。

	  "%PROGRAMFILES%\tools\px9512.exe"

	  注) wpeinit は Windows PE の初期化に必要なので消さないでください。

	・C:\winpe\boot.wim を C:\winpe\mount フォルダから再構成します。
	  コマンドプロンプトに下記のコマンドを入力、[Enter]。

	  ImageX /unmount /commit C:\winpe\mount

	  注)C:\winpe\mount以下のフォルダ・ファイルを閉じてから実行してください。

	・C:\winpe\boot.wim を C:\winpe\ISO\sources フォルダに移動します。

	・必要なイメージファイル vista.img vista.001 vista.002 data.img を
	  C:\winpe\ISO フォルダにコピーします。

	・iso イメージを構成します。
	  コマンドプロンプトに下記のコマンドを入力、[Enter]。

	  oscdimg -u2 -bC:\winpe\etfsboot.com C:\winpe\ISO C:\winpe\winpe.iso

	・C:\winpe\winpe.isoをDVDに焼いて完成。

	  注) 容量が5GB超なので二層DVDになります。

	②ディスクの使い方

	・PCにディスクを入れて起動します。
	  画面に

	  Press any key to boot from CD or DVD.

	  と表示されたら[Enter]。

	  注) 表示されない場合再起動してみてください。
	      それでも表示されない場合、BIOSの設定を変更する必要があります。

	・数分ほどでpx9512.exeが自動起動して修復メニューを出します。
	  必要な処理をメニューから選んで番号を入力、[Enter]。

	・その後は表示の通り操作・修復してシャットダウンします。


  SOTEC PX9512 以外への応用法

	※他社製PCも考慮して説明します。

	・まずリカバリ動作する際、どのパーティションからどのプログラムが呼び出されているのかを
	  見つけます。(リカバリプログラム(Symantec Ghostなど)を購入したならそれで結構。)

	  たいていの場合隠しパーティション内にデータが格納されているので、
	  パーティションを全部見てみます。

	  [コントロールパネル]→[管理ツール]→[コンピューターの管理]→[ディスクの管理]
	  ボリュームの下に(C:)などが出ますが名無しのボリュームがあったら
	  隠しパーティションです。右クリック→[ドライブ文字とパスの変更(C)...]→[追加(D)...]で
	  使われていないドライブ文字を選択。	  [OK]を押すと隠しパーティションが指定した
	  ドライブ文字になり有効になります。
	  リカバリに使っていそうなデータ(exeファイル)とその周辺のファイルを
	  別ドライブにコピーしておきます。

	  例)PX9512の場合、[HDREC]というパーティション内のrecover2.exe。
	     起動してみたらそのままリカバリ画面だったｗ
	     リカバリデータがどれかわからなかったのでパーティション内の全ファイルをコピー。

	  ※手っ取り早く Ubuntu で HDD を覗いて隠しパーティションを見る方法もあります。

	  注)ブートローダーパーティションの可能性もあるのでコピー操作だけにとどめておきます。
	     いじるとPCが起動しなくなる原因となる可能性がある。

	・リカバリプログラムに渡ったコマンドラインを調べます。
	  (リカバリプログラム(Symantec Ghostなど)に渡すコマンドを知ってるならそれで結構。)

	  例)recover2.exe をメモ帳で見ると普通に標準・高度1・高度2の3種コマンドラインを発見。

	  メモ帳の方法で見つかればいいのですが…
	  ブートローダーが直接リカバリプログラムを呼んだりするとどうしても見つからなかったり
	  するので、見つからない場合はリカバリプログラムの代わりにラッパー・ダミープログラムを
	  置いてリカバリを実行してみてコマンドラインを吐かせます。

	・(メインイベント)px9512.hspを改造します。

	  注)コンソールhsp(hsp3cl)であることに注意。stick等が使えません。

	  スクリプト内に簡易説明があります。
	  ただメニュの提示・他のプログラム呼び出す・終了しかしてないので
	  今までの説明が理解できるユーザーなら大体はわかると思われます。

	  このスクリプト改造してGUIにしてもいいんじゃない？
	  ※私はもう疲れた。親父が二層DVD買ってくれないし。

  いずれの操作にせよ、問題･疑問がございましたら作者までお問い合わせください。


*制限

	使用許諾契約書

	以下より「GhostOEM32 EXEC 改 for SOTEC PX9512 VISTA」を「本ソフトウェア」、
        「不家路虚」を「私」、法人、個人を問わず「本ソフトウェア使用者」を「使用者」と
        記載いたします。

	このプログラムは日本国内だけの配布、及び使用とします。
	FOR HAND OUT AND USE IN JAPAN ONLY.
	それ以外での使用は応答の保証外となります。

	1.著作権について

	NYSL Version 0.9982 + E (追項)

	A. 本ソフトウェアは Everyone'sWare です。このソフトを手にした一人一人が、
	   ご自分の作ったものを扱うのと同じように、自由に利用することが出来ます。

	  A-1. フリーウェアです。作者からは使用料等を要求しません。
	  A-2. 有料無料や媒体の如何を問わず、自由に転載・再配布できます。
	  A-3. いかなる種類の 改変・他プログラムでの利用 を行っても構いません。
	  A-4. 変更したものや部分的に使用したものは、あなたのものになります。
	       公開する場合は、あなたの名前の下で行って下さい。

	B. このソフトを利用することによって生じた損害等について、作者は
	   責任を負わないものとします。各自の責任においてご利用下さい。

	C. 著作者人格権は 不家路虚 に帰属します。著作権は放棄します。

	D. 以上の３項は、ソース・実行バイナリの双方に適用されます。

	E. コンパイル要項

	 本ソフトウェアは、

	　onion software HSP script editer for Windows version 3.1
	　Copyright (C) 1997-2007 onion Software
	　(http://www.onionsoft.net/hsp/)

	 によってコンパイルされています。また

	  冷凍蛙 1.12b2 with UPX 3.03w

	 　UPX (The Ultimate Packer for eXecutables)
	　 Copyright (c) 1996-2000 Markus Oberhumer & Laszlo Molnar
	　 (http://upx.sourceforge.net/)

	   冷凍蛙
	   Copyright (C) 2009 - 2010 石橋祥太
	   (http://ishibasystems.ikaduchi.com/)

	 で圧縮されています。

	 それぞれのソフトウェアの著作権はそれぞれの作者に帰属します。

	2.禁止事項

	 本ソフトウェアは違法な行為、Windows PE・Windows AIK・その他 Windows OS 等の
         ライセンス規約違反にならないように使用してください。

	3.配布に関する制限について

	 制限を設けません。

	4.配布に関する権利について

	 ? 無いね、特に。

	5.配布に関するお願いについて

	 配布の際に作者に連絡してくれると大喜びしますよ（＾＾）。
 
	6.免責･保証について

	 私は、本ソフトウェアに関して、法定上の瑕疵担保責任、商品性および
	 特定の目的への適合性の保証、その他一切の保証をいたしません。


*お問い合わせ

  ご意見、不具合の情報、要望、感想などいつでもお待ちしています。

　home page	http://ishibasystems.ikaduchi.com/
　e-mail	http://ishibasystems.ikaduchi.com/mail.html

*更新履歴

[2009/09/04]	VIA-1A-1 version 1.0 公開
	開発。

[2015/01/24]	テキストの修正
	T.I様、ご指摘ありがとうございました。
	
[2015/06/21]	VIA-1A-1 version 1.1 バグ修正
	シャットダウン時のwpeutilコマンドの修正

著作者人格権 HuIeji Utsuro , Ishibasystems Software.

[EOF]
