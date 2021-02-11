# msfs-rjsn

作成に使ったファイルを置きました。
空港の情報は rjsn_asset/RJSN_Scene.xml の中に色々保存しています。都度参照してください。

scenery周辺のことはmsfs-niigataの方を参照してください。今回もGoogle地図由来の画像ファイルはgit管理を省いています。

## まえがき

デフォルトの空港だと変なところがスポット扱いになっていて、勝手に飛行機が置かれて移動の邪魔になることがあります。
(RJSNはその最たる例で、誘導路からエプロンに入る場所が全ヶ所スポット扱いになっている)

なので、正しい情報に修正してやる必要があります。
しかし現状ではエディット機能のようなものはなく、完全ハンドメイドで作成して上書きするしかないようです。

とはいえ、フルスクラッチで作成するのは大変なので、まずはデフォルトの空港からファイルを抽出します。
抽出には「Airport2Project」を使用しました。(現時点の最新は以下でした、都度最新のファイルを探してからダウンロードを推奨)
https://www.fsdeveloper.com/forum/threads/official-airport-to-sdk-project-here-is-a-small-app-to-help-you-draft.450344/

AIS JAPANに載っているデータを参照しました。要会員登録ですが無料で見れます。自衛隊エリアのスポット名などは分からなかったので、自前で適当な数値を指定しました。
https://aisjapan.mlit.go.jp/Login.do


## 手順

Airport2Project(v1.0.03)で抽出後にやったことは以下です。細かい修正は他にも色々やっています。
* ILSを無効にして自作
* RunwayのApproach lightが眩しすぎるので修正 (作成時点ではRW28側はPALS、RW10側はSALSだったので、それっぽく修正)
* RunwayのPAPIの位置を修正
* デフォルトの吹き流しを無効にして新規配置
* 誘導路名を修正
* エプロン用の照明を追加

parkingはtaxi pathで繋いでやらないとルート認識されないようです。
直前までtaxi用のpathを作って、直前のpointとparkingを選択した状態で右クリックメニューからパスを接続するといい感じになります。

vehicleのparkingからvehicle用のtaxiwayを引いていい感じに繋がないと給油とかしてくれないようです。忘れずに。

Jetwayがターミナルと繋がってないので、sceneryで「ramp」とかで適宜検索していい感じに配置して繋げます。
このファイルではsceneryごとGoogle地図から引っ張ってきたオブジェクトに置き換えています。
近くで見るとなかなか見栄えが悪いので、Blenderとかでスクラッチして置き換えたいなとか考えています。面倒なので誰か汎用品を作って欲しいです。

### ILS

ILSは「然るべき名前を使って作成すると認識してくれる」タイプのようです。
RJSNの場合は「ILS RW28」が然るべき名前でした。実際にバニラの状態でA320neoなどを飛ばしてみると確認できます。

AIS JAPANの資料に色々載ってるのでそれを見ながらILSの情報を入力します。
資料(チャート)の見方はプロに聞くかプロ用の教材で確認してください。
~わたしはプロじゃないのでフィーリングでやりました。~

各項目をちゃんと設定しないと、Little Navmapなどのソフトウェアで見た時に誤った情報が表示されるようです。特にMagvar。

|項目|推奨設定|RJSN RW28|RJCC RW19L|説明|
|--|--|--|--|--|
|Primary ILS|as required|v|v|どっちがプライマリ側かはVASI(PAPI)や進入灯の設定いじれば確認できる。たぶんScenery Editorで表示されている方がプライマリ。|
|Friendly name|適切な名称|ILS RW28|ILS RW19L|然るべき名前で入力する必要あり。バニラ状態でILS着陸する時に確認できる。「ILS RW○○」が正解？|
|Edit position|as required|as required|as required|ILSを移動させるかどうか。基本的には終端側に配置する。資料見ながら配置。|
|Frequency|適切な数値|109300000|109350000|周波数。滑走路ごとに異なる。メガヘルツになるようにゼロをたくさん足す。(微妙に端数が合わなくても気にしない)|
|Ident|適切な名称|IL28|IL19L|IL+方向+LかRかCか、っていう命名規則で入れておけば多分OK。|
|Magver|適切な度数|8.2|9.6|「方位磁石の北」と「実際の北」のズレ。チャートにも書いてあるが、Little Navmapで見てみるのが早い。Westの方向がプラス。|
|Heading|適切な角度|281|182|滑走路の方角。360度単位。|
|Enable range| | | |距離を有効にするかどうか。現実のRJSNは14NMっぽかったけれどチェックなし。|
|Enable beam width|v|v|v|幅を有効にするかどうか。|
|Beam width|5|5|5|幅。±5度で合計10度になるのが一般的、一応資料も見る。|
|BackCourse|?|v| |どうなるのかよく分かってない。|
|Has DME|v|v|v|DMEがあるかどうか。あるならチェック|
|DME - Edit position|as required|as required|as required|DMEの場所を指定。資料見ながら配置。|
|DME - Range|18540|18540|18540|距離の指定。10NMが一般的、一応資料も見る。|
|Has glide slope (GS)|v|v|v|グライドスロープ(GS,GP)があるかどうか。あるならチェック。|
|GS - Edit position|as required|as required|as required|GS用アンテナの場所を指定。資料見ながら配置。|
|GS - Range|18540|18540|18540|距離の指定。10NMが一般的、一応資料も見る。|
|GS - Pitch|3？|3|3|進入角度。3度が一般的だがRJCJは2.7度、資料見ながら設定。ちなみにロンドンシティ(EGLC)は5.5度らしい。|
|Secondary ILS|as required| |v|セカンダリ側にILSが存在する場合は設定を続ける。|
|...|as required||RW01Rの設定|以下、セカンダリ側の設定が必要な場合は同じ項目が続く。|


できたら実際に飛ばしてデバッグしてみる。ちゃんと着陸できればOK。
**この作業が最高に面倒くさい。**

## 課題
北日本の滑走路は、降雪時の視認性確保のために線がオレンジ色になっていることが多いです。本物のRJSNもオレンジ色です。
ですが、機能として「線をオレンジ色」みたいに選ぶことは現状できないので、material登録とかでいい感じにする必要がありそう。
(いくつか作例もあるようなので実現はできそう、しかし方法が不明瞭)

sceneryなどのファイルはデフォルトのものの他、Communityフォルダに入っているものやマケプレでダウンロードしたコンテンツのものも使用できるようです。
しかし個人で楽しむならともかく、誰かに配布する前提のファイルで有料アドオンの素材を使用するのは厳禁です。
(もっとも、固有の空港のターミナルなど他に使いまわしが利かなそうなものが多いですが)

RJCK(釧路)やRORS(下地島)のファイルも便利そうなのがいくつかありましたが、デフォルトのものだけを使用するようにしています。
(日本アップデートの追加ファイルをダウンロードしていない人がいたら、それらに付属するオブジェクトも反映されないと思われるため)
ファイル管理の仕様について詳しい人がいたら日本語wikiとかどこかに書いてください。

ただ、Communityフォルダの中身は適宜退避させれば除けることができますが、マケプレでダウンロードしたコンテンツは一旦ゲーム内から削除しないとダメそうです。
マケプレ由来のデータは削除するのが面倒だったので、選ぶ時に選ばないように気を付ける、くらいで手動で除けました。

わたしの作成した素材は「berry_」と頭につけています。そうすると自分で使う時も分かりやすいですし、誰かにとっても「これはberryの作ったファイルだ」と除けやすくなります。

---

他にも注意点は色々あったので、思い出したタイミングでパソコンとチョコレートが手元にあったら追記します。
