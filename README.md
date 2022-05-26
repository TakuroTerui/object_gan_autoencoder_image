# 物体検出とGAN、オートエンコーダー、画像処理入門 PyTorch/TensorFlow2による発展的・実装ディープラーニング

## 1章 開発環境について
### 1.1 Anacondaの導入
#### 1.1.1 Anacondaに含まれる主なツール
- Anaconda Navigator（アナコンダナビゲーター）
- Jupyter Notebook（ジュピターノートブック）
- Spyder（スパイダー）
#### 1.1.2 Anacondaのインストール
#### 1.1.3 Anaconda Navigatorを起動して仮想環境を用意する
#### 1.1.4 ライブラリのインストール
- PyTorchを仮想環境にインストールする
### 1.2 Jupyter Notebookを使う
#### 1.2.1 Jupyter Notebookを仮想環境にインストールする
#### 1.2.2 ノートブックを作成する
- ノートブックを保存するためのフォルダーを作成する
- ノートブックの作成
#### 1.2.3 ソースコードを入力して実行する
- Jupyter Notebookのコマンド
#### 1.2.4 ノートブックを閉じる／開く
- ノートブックを閉じる
- ノートブックを開く
#### 1.2.5 Jupyter Notebookのメニューを攻略する
- ［File］メニュー
- ［Edit］メニュー
- ［View］メニュー
- ［Insert］メニュー
- ［Cell］メニュー
- ［Kernel］メニュー
### 1.3 Spyderを使う
#### 1.3.1 Spyderを仮想環境にインストールする
#### 1.3.2 モジュールの保存
#### 1.3.3 モジュールのプログラムを実行する
- 実行中のプログラムの変数の値を確認する
### 1.4 Google Colabを使う
#### 1.4.1 Colabノートブック
- Colabの利用可能時間
#### 1.4.2 Googleドライブ上のColab専用のフォルダーにノートブックを作成する
- Googleドライブにログインしてフォルダーを作成する
- ノートブックの作成
#### 1.4.3 セルにコードを入力して実行する
#### 1.4.4 Colabノートブックの機能
- ［ファイル］メニュー
- ［編集］メニュー
- ［表示］メニュー
- ［挿入］メニュー
- ［ランタイム］メニュー
- ［ツール］メニュー
- GPUを有効にする


## 2章 SSDによる物体検出
### 2.1 物体検出の概要
#### 2.2.1 物体検出とは
#### 2.1.2 SSD（Single Shot MultiBox Detector）
#### 2.1.3 SSDにおける物体検出の流れ
- 物体検出の学習時の処理
- 物体検出の推論時の処理
#### 2.1.4 プログラミングの流れ
- データセットに関わるプログラミング
- SSDモデル
- 学習と学習後における推論
### 2.2 データセットの用意と前処理
#### 2.2.1 VOCデータセットの概要
- VOC2007
- VOC2012
#### 2.2.2 VOCデータセットとVGG16、SSD300の学習済み重みのダウンロード
- 「data」フォルダーと「weights」フォルダーの作成
- VOC2012のダウンロードと解凍
- VGG16の学習済み重み、SSD300の学習済み重みのダウンロード
#### 2.2.3 アノテーションデータをリスト化する
- プログラムの実行はノートブック、クラスなどの定義はモジュールで
- イメージとアノテーションのファイルパスをリスト化する（make_filepath_list（）関数）
- バウンディングボックスの座標と正解ラベルをリスト化するクラスの定義（GetBBoxAndLabelクラス）
#### 2.2.4 イメージとアノテーションを前処理する
- COLUMN PyTorchとTorchvisionのインストール
- データ拡張の内容
- イメージの切り出し
- データの前処理を行うDataTransformクラスの定義
- データの前処理をイテレートする仕組みを提供するPreprocessVOC2012クラス
#### 2.2.5 ミニバッチを生成するDataLoader
- DataLoaderでミニバッチを生成する
### 2.3 SSDモデルの実装
#### 2.3.1 畳み込みニューラルネットワーク（CNN）
- 2次元フィルターで画像の特徴を検出する
- サイズ減した画像をゼロパディングで元のサイズに戻す
- プーリングで歪みやズレによる影響を回避する
#### 2.3.2 SSDモデルの構造
- SSDモデルの出力
- SSDモデルの全体像
#### 2.3.3 vggネットワークの実装
- vggネットワークの構造
- vggネットワークを生成するmake_vgg（）関数の定義
- vggネットワークを生成して構造を確認してみよう
#### 2.3.4 extrasネットワークの実装
- extrasネットワークの構造
- extrasネットワークを生成するmake_extras（）関数の定義
- make_extras（）関数の動作確認
#### 2.3.5 locネットワークの実装
- locネットワークの構造
- locネットワークを生成するmake_loc（）関数の定義
- make_loc（）関数の動作確認
#### 2.3.6 confネットワークの実装
- confネットワークの構造
- confネットワークを生成するmake_conf（）関数の定義
- make_conf（）関数の動作確認
#### 2.3.7 L2Norm層の実装
- L2Normの処理
- L2Normクラスの定義
#### 2.3.8 デフォルトボックスを生成するDBoxクラス
- 特徴量マップのセルごとにデフォルトボックスを用意する仕組み
- DBoxクラスの定義
- DBoxクラスの動作確認
### 2.4 順伝播処理の実装
#### 2.4.1 decord（）関数の定義
- decord（）関数を定義する
#### 2.4.2 1つの物体に対するバウンディングボックスを1つに絞り込む
- Non-Maximum Suppressionの処理を追ってみる
- nonmaximum_suppress（）関数の定義
#### 2.4.3 推論時にバウンディングボックスの情報を出力する
- Detectクラスの処理の概要
- COLUMN One-hotエンコーディングとソフトマックス関数
- Detectクラスを定義する
### 2.5 SSDモデルの実装
#### 2.5.1 SSDモデルにおける順伝播処理について徹底解説
- __init（）__における初期化処理
- forward（）による順伝播処理の詳細
- SSDクラスの定義
### 2.6 バウンディングボックスの処理
#### 2.6.1 DBoxの情報をBBox形式の情報に変換するpoint_form（）関数
- point_form（）関数の定義
#### 2.6.2 2個のボックスが重なる部分の面積を求めるintersect（）関数
- intersect（）関数の定義
#### 2.6.3 ボックス間のジャッカード係数（IoU）を計算するjaccard（）関数
- jaccard（）関数の定義
#### 2.6.4 match（）関数の定義
- match（）関数の処理
- match（）関数の定義
### 2.7 デフォルトボックスのオフセット情報を作るencode（）関数
- encode（）関数の定義
### 2.8 SSDの損失関数としてMultiBoxLossクラスを作成する
#### 2.8.1 MultiBoxLossクラスのforward（）メソッドの処理
- 教師データとして「DBoxのオフセット値」と「物体の正解ラベル」を取得
- 物体を検出したPositive DBoxのオフセット情報の損失「loss_l」を計算
- COLUMN SmoothL1Loss関数について
- confネットワークが出力する21クラスの予測値（確信度）の損失を求める
- COLUMN クロスエントロピー誤差
- Hard Negative Miningのための抽出用マスクを作成
- オフセット情報の平均損失と確信度の損失平均を求める
- MultiBoxLossクラスの定義
### 2.9 SSDモデルの学習プログラム
#### 2.9.1 「Googleドライブ」にデータ一式をアップロードする
- Googleドライブの「ObjectDetection」フォルダーへのアップロード
#### 2.9.2 Colabノートブックで学習プログラムを実行する
- ドライブのマウント
- GPUの設定
- 「ObjectDetection」フォルダー内のモジュールをインポート可能にする
- データローダーの作成
- SSDモデルの生成
- 損失関数とオプティマイザーの作成
- 学習と同時に検証を行う関数を実行する
- COLUMN 誤差逆伝播における重みの更新式
### 2.10 学習済みモデルでの推論の実施
#### 2.10.1 ノートブックを作成して物体検出を実施
- 検証モードのSSDモデルを生成して学習済み重みをセット
- SSDモデルに画像を入力する
#### 2.10.2 検出結果を写真上に描画する
- 検出から描画までを行うSSDPredictionsクラス
- 物体検出を行って検出結果を描画する
- 学習済みの重み「ssd300_mAP_77.43_v2.pth」に取り替えてみる


## 3章 「FasterRCNN＋InceptionResNetV2」による物体検出
### 3.1 TensorFlow Hubの物体検出モデル「FasterRCNN＋InceptionResNetV2」
#### 3.1.1 「FasterRCNN＋InceptionResNetV2」の概要
### 3.2 「FasterRCNN＋InceptionResNetV2」で物体検出を体験してみる
#### 3.2.1 Colabノートブックを作成
#### 3.2.2 物体検出の実施
- 風景写真を物体検出にかける
- 昆虫の写真、スマートフォンの写真、鳥の絵画で試してみる


## 4章 オートエンコーダー
### 4.1 オートエンコーダー（PyTorch）
#### 4.1.1 オートエンコーダーのメカニズム
- 3層構造のオートエンコーダー
#### 4.1.2 PyTorchによるオートエンコーダーの実装
- オートエンコーダーを実装して手書き数字を学習する
- 復元された画像を表示してみる
### 4.2 畳み込みオートエンコーダー（TensorFlow）
#### 4.2.1 畳み込みオートエンコーダーのメカニズム
- エンコーダー
- デコーダー
#### 4.2.2 TensorFlowによる畳み込みオートエンコーダーの実装
- 畳み込みオートエンコーダーを実装してファッションアイテムの画像を学習する
- 学習結果を出力してみる
### 4.3 畳み込みオートエンコーダーによるノイズ除去（TensorFlow）
#### 4.3.1 ノイズ除去畳み込みオートエンコーダーのメカニズム
#### 4.3.2 TensorFlowによるノイズ除去畳み込みオートエンコーダーの実装
- ノイズ除去畳み込みオートエンコーダーを実装して学習する
- ノイズを加えた画像と復元した画像を出力する
### 4.4 変分オートエンコーダー（PyTorch）
#### 4.4.1 変分オートエンコーダーのメカニズム
- 変分オートエンコーダーの損失関数
#### 4.4.2 PyTorchによる畳み込みオートエンコーダーの実装
- 変分オートエンコーダーを実装する
- 変分オートエンコーダーに入力した結果を出力する
- デコーダーに任意のデータを入力して画像を生成する
### 4.5 変分オートエンコーダー（TensorFlow）
#### 4.5.1 変分オートエンコーダーのメカニズム
- 潜在ロスの計算
#### 4.5.2 TensorFlowによる変分オートエンコーダーの実装
- 変分オートエンコーダーを実装する
- 入力したイメージと復元したイメージを出力してみる


## 5章 GAN（敵対的生成ネットワーク）
### 5.1 DCGANによる画像生成（PyTorch）
#### 5.1.1 GANのメカニズム
- GANの学習の仕組み
#### 5.1.2 DCGANのメカニズム
#### 5.1.3 PyTorchによるDCGANの実装
- DCGANを実装する
### 5.2 DCGANによる画像生成（TensorFlow）
#### 5.2.1 TensorFlowによるDCGANの実装
- DCGANを実装してFashion-MNISTデータセットで学習する
- 学習済みの生成器で画像を生成
### 5.3 Conditional GANによる画像生成（PyTorch）
#### 5.3.1 Conditional GANのメカニズム
- 生成器への正解ラベルの入力
- 識別器への数値ラベルの入力
#### 5.3.2 PyTorchによるConditional GANの実装
- Conditional GANを実装する
### 5.4 Conditional GANによる画像生成（TensorFlow）
#### 5.4.1 TensorFlowによるConditional GANの実装
- Colabノートブックでプログラムを実行
- 学習済みの生成器にノイズを入力して画像を生成してみる

## Appendix ディープラーニングの数学的要素
### A.1 ニューラルネットワークのデータ表現：テンソル
#### A.1.1 NumPyのスカラー（0階テンソル）
#### A.1.2 NumPyのベクトル（1階テンソル）
#### A.1.3 NumPyの行列（2階テンソル）
#### A.1.4 3階テンソルとより高階数のテンソル
### A.2 ニューラルネットワークを回す（ベクトルの演算）
#### A.2.1 ベクトルの算術演算
#### A.2.2 ベクトルのスカラー演算
#### A.2.3 ベクトル同士の四則演算
- ベクトル同士の加算と減算
#### A.2.4 ベクトルのアダマール積を求める
#### A.2.5 ベクトルの内積を求める
### A.3 ニューラルネットワークを回す（行列の演算）
#### A.3.1 行列の構造
#### A.3.2 多次元配列で行列を表現する
#### A.3.3 行列のスカラー演算
#### A.3.4 行列の定数倍
#### A.3.5 行列の成分にアクセスする
#### A.3.6 行列の成分同士の加算・減算をする
#### A.3.7 行列のアダマール積
#### A.3.8 行列の内積を求める
- 行列同士の内積を求めてみる
#### A.3.9 行と列を入れ替えて「転置行列」を作る
### A.4 微分
#### A.4.1 極限（lim）
#### A.4.2 微分の基礎
#### A.4.3 微分をPythonで実装してみる
- 数値微分で関数を微分してみる
- プログラムの実行結果
#### A.4.4 微分の公式
#### A.4.5 変数が2つ以上の場合の微分（偏微分）
#### A.4.6 合成関数の微分
- 合成関数のチェーンルールの拡張
- 積の微分法