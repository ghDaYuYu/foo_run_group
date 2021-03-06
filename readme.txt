*******************************************************************
* Many thanks to Florian - developer of Run services(foo_run.dll) *
* Also, I referred to foo_r128scan's source. Thanks kode54.       *
*******************************************************************


▼更新履歴
[2020-01-11 v1.04]
Reload info when finishedオプションがちゃんと機能していなかったのを修正

[2019-05-18 v1.03]
Reload info when finishedオプションを追加
その他細かい変更

[2019-01-27 v1.02]
コマンド実行前にカレントディレクトリをfoobar2000.exeのあるパスにセットするようにした
これによって他のコンポーネントによってカレントディレクトリが変更された後に相対パスで記述されたプログラムの実行が失敗するのを修正できた
その他細かい変更

[2016-05-28 v1.01 stable]
メモリリークしていたのを修正

[2015-12-30 v1.01_beta2]
無駄なコードを整理
waitにチェックなしの場合もthread countが意味を持つようになった(といってもほとんど意味ない)

[2015-12-29 v1.01_beta]
プログレスバーを表示&更新するようにした
その他細かい修正

[2015-12-27 v1.00_beta3]
最後に起動したスレッドの終了をメインスレッド内で待ってしまっていたのを修正
waitにチェック無し または スレッド数が1である場合 の処理を高速化した
その他細かい修正

[2015-12-26 v1.00_beta2]
* cfgファイルへの保存の仕方を変えたせいで、設定が初期化されます 申し訳ないです
Filterオプションを追加
エラーがあったときはできるだけpopup_messageを表示するようにした
Path内で%group_index%を使用できるようにした
その他細かい修正

[2015-12-23 v1.00_beta]
最初のリリース


▼License
MPL v2.0
Please refer to COPYING.


▼本コンポーネントについて
Florian氏のRun services(foo_run.dll)と同じく、コンテキストメニューから外部のプログラムを起動できるようにします
ただし選択しているアイテムをTitle formattingに従ってグループ分けし、グループごとにコマンドを実行します
foobar2000 v1.1以上で動作します

▼設定
Preferences -> Tools -> Run services per groupを選択してください
基本的な設定の仕方はRun services(foo_run.dll)と同じです

* Label
  コマンド名を設定します

* Path
  実行ファイルのパスを含むコマンドラインを設定します
  Title formattingが使用可能です ( ) % などの文字の扱いには注意してください
  本コンポーネントによる拡張変数%path_group%, %group_index%が使用できます(詳細については後述)
  パスはfoobar2000.exeのある場所を基準とする相対パスでも良いです

* Minimize window
  チェックすると窓を最小化してプログラムを起動します

* Wait process
  チェックすると起動したプログラムが終了するまで待機します

* Reload info when finished
  Wait processがONのときのみ有効です
  チェックすると起動したプログラムが全て終了したときにファイルから情報を再読み込みします
  起動したプログラムによってメタデータ等が編集される場合にチェックすると便利だと思います

* Thread count
  同時に起動するプログラムの数を設定します
  0を指定するとCPUのコアに合わせて自動で調節します
  Waitにチェックしていないときはあまり意味ありません

* Album grouping pattern
  コマンド実行時に選択しているアイテムをどのようにグループ分けするかを決めます
  Title formattingが使用可能です
  選択しているアイテムすべてをひとつのグループとして扱いたい場合は、一桁の数字を入力すると良いです

* Filter(Query Syntax)
  Query Syntaxに従って選択しているアイテムにFilterをかけます SORT BY...などのソートは設定できません
  何もFilterをかけたくない場合は空欄にしてください
  エラーがあった場合はConsoleにエラーメッセージを出力します
  例: "$ext(%filename_ext%)" IS MP3 -> 選択しているファイルのうち、拡張子mp3のファイルに対してのみコマンドを実行


▼拡張変数
設定のPathの中では本コンポーネントの拡張変数が使用可能です
* %path_group%
  グループ内のアイテムのパスを列挙した値を返します
  たとえば同じグループ内のファイル
  C:\Users\test\Music\01 track 1.mp3
  C:\Users\test\Music\02 track 2.mp3
  C:\Users\test\Music\03 track 3.mp3
  に対してコマンドを実行した場合、
  "C:\Users\test\Music\01 track 1.mp3" "C:\Users\test\Music\02 track 2.mp3" "C:\Users\test\Music\03 track 3.mp3"
  という値を返します

* %group_index%
  何番目のグループであるかを返します(0ベースです)
  グループの並び順はAlbum grouping patternの設定に影響されます(グループ分けする際にソートしているため)


▼注意
Pathで指定する実行ファイルは、拡張子exeのファイルでなければなりません
ゆえにバッチファイルを指定したい場合は、
cmd.exe /c "test.bat 引数"
という形にしてください

Title formattingは、グループの最初のアイテムの情報によってコンパイルされます
なおグループ内のファイルは%path_sort%によってソートしています
たとえば
同じグループ内のファイル
track 02.mp3 (トラックナンバータグが2)
track 01.mp3 (トラックナンバータグが1)
track 03.mp3 (トラックナンバータグが3)
に対してコマンドを実行した場合、%track% は 01 を返します


▼設定例
* googleでAlbum Artistを検索
  Path: "C:\Program Files '('x86')'\Internet Explorer\iexplore.exe" "http://www.google.co.jp/search?q=$replace(%album artist%, ,+)"

* aac(mp3)gain タグでグループ分けしてアルバムゲインをかける
  Path: "C:\Program Files '('x86')'\aacgain\aacgain.exe" /a /c %path_group%
  Wait process: ON
  Reload info when finished: ON
  Thread count: 0
  Album grouping pattern: %album artist%|%date%|%album%
  Filter: "$ext(%filename_ext%)" IS MP3 OR "$ext(%filename_ext%)" IS M4A

* aac(mp3)gain 選択しているファイルをひとつのアルバムとみなしてアルバムゲインをかける
  Path: "C:\Program Files '('x86')'\aacgain\aacgain.exe" /a /c %path_group%
  Wait process: ON
  Reload info when finished: ON
  Thread count: 0
  Album grouping pattern: 1
  Filter: "$ext(%filename_ext%)" IS MP3 OR "$ext(%filename_ext%)" IS M4A

* aac(mp3)gain トラックゲインをかける
  Path: "C:\Program Files '('x86')'\aacgain\aacgain.exe" /r /c %path%
  Wait process: ON
  Reload info when finished: ON
  Thread count: 0
  Album grouping pattern: %path%
  Filter: "$ext(%filename_ext%)" IS MP3 OR "$ext(%filename_ext%)" IS M4A

* Album Art Downloader XUI
  Path: ",,\..\Program Files\AlbumArtDownloader\AlbumArt.exe" /artist "%album artist%" /album "%album%" /p "$directory_path(%path%)" /f "folder.'%extension%'"
  Album grouping pattern: $directory_path(%path%)

* 何番目に発売されたアルバムであるかを表示
  Path: cmd.exe /c "echo "%album%"は$add(%group_index%,1)番目に発売されたアルバムです && pause"
  Album grouping pattern: %album artist%|%date%|%album%
