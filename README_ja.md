Susie plugin for RUNE games (spiruneg)
======================================
[![AppVeyor Build Status](https://ci.appveyor.com/api/projects/status/ye14umslt33ay6aj/branch/master?svg=true)](https://ci.appveyor.com/project/gocha/spiruneg/branch/master)

spiruneg は [Rune](http://www.fromsoftware.jp/top/soft/rune/) 製ゲーム向けの Susie プラグインパッケージです。

- **axruneg.spi**: G アーカイブ（*.g）展開プラグイン。

残念ながら、新しいアーカイブ（G2）や画像フォーマットに対応したプラグインはありません。

Susie プラグインとは？
------------------------

[Susie](http://www.digitalpad.co.jp/~takechin/) は昔ながらの Windows 向けの画像ビューアです。Susie プラグイン（*.spi）と呼ばれるファイルを追加することで対応フォーマットを増やすことができます。国内ではよくゲーム内部のカスタム画像フォーマットをデコードするのに Susie プラグインが使われます。

Susie プラグインに対応した画像ビューアはいくつか存在します。

- [Susie](http://www.digitalpad.co.jp/~takechin/betasue.html#susie32)
- [Linar](http://hp.vector.co.jp/authors/VA015839/)
- [picture effecter](http://www.asahi-net.or.jp/~DS8H-WTNB/software/index.html)
- [stereophotomaker](http://stereo.jpn.org/eng/stphmkr/)
- [vix](http://www.forest.impress.co.jp/library/software/vix/)
- [A to B converter](http://www.asahi-net.or.jp/~KH4S-SMZ/spi/abc/index.html)
- [ACDSee](http://www.acdsee.com/) (commercial)

わたしのお気に入りは、閲覧目的なら [Linar](http://hp.vector.co.jp/authors/VA015839/)、一括変換目的なら [AtoB Converter](http://www.asahi-net.or.jp/~kh4s-smz/spi/abc/) です。

注意事項・備考など
------------------------

- 本ツールを用いたことによる、いかなる損害やトラブルについても作者は責任を負いません。

ファイルフォーマット
------------------------

### G アーカイブフォーマット

ヘッダの構造を下表に示します。

|Size|Description                                               |
|----|----------------------------------------------------------|
|   8|識別子 "GML_ARC\0"                                        |
|   4|ファイルデータセクションへのオフセット                    |
|   4|展開後のファイル情報のサイズ                              |
|   4|ファイル情報のサイズ                                      |
|   m|ファイル情報 (暗号化を伴った LZSS によって圧縮されている) |
|   n|ファイルデータ (単一換字式暗号による暗号化が施されている) |

ファイル情報（復号後）は下記のように設計されています。

|Size|Description                                         |
|----|----------------------------------------------------|
| 256|単一換字式暗号の復号テーブル                        |
|   4|ファイル数                                          |
| m*n|ファイルごとの情報                                  |

ファイルごとの情報:

|Size|Description                                         |
|----|----------------------------------------------------|
|   4|ファイル名の長さ                                    |
|   m|ファイル名                                          |
|   4|ファイルデータへのオフセット（ファイル先頭から）    |
|   4|ファイルサイズ                                      |
|   4|ファイル先頭の4バイト                               |

ファイル情報の暗号化は、ビットを反転させることによって行われています。詳細については、ソースコードを参照してください。

実際のファイルデータは単一換字式暗号による暗号化されています。
ただし、ファイル情報に記録された値で最初の4バイトを変更する必要があるので注意してください。
