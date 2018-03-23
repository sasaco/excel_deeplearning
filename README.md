# シリーズ目次
- [機械学習の理論を理解せずに tensorflow で オセロ AI を作ってみた ～導入編～](http://qiita.com/sasaco/items/3b0b8565d6aa2a640caf)
- [機械学習の理論を理解せずに tensorflow で オセロ AI を作ってみた ～実装編～](http://qiita.com/sasaco/items/fdb9771c146cb877b183)
- [機械学習の理論を理解せずに tensorflow で オセロ AI を作ってみた ～いざ対戦編～](http://qiita.com/sasaco/items/f9aa608860eebb3026c1)
- [機械学習の理論を理解しようとしてから オセロ AI を作ってみた 〜再始動‼〜](https://qiita.com/sasaco/items/5e102063c256bf56e396)
- [機械学習の理論を理解しようとしてから オセロ AI を作ってみた 〜何これ Alpha Zero 編〜](https://qiita.com/sasaco/items/d249ee3493b5b85c6eb5)
- 機械学習の理論を理解しようと エクセルでニュートラルネットワークを作ってみた 〜画像認識 mnist 編〜

いろいろチャレンジしてみたが、成果が上がらないので、機械学習の理論を理解しようと思う

# エクセルでニュートラルネットワークを作ってみた

はじめにソースコードをダウンロードします。
ソースはここにおいておきます。
[`$ git clone https://github.com/sasaco/excel_deeplearning.git`](https://github.com/sasaco/excel_deeplearning)


## フォルダ構成

今回作成した エクセルファイル(mnist.xlsm) と 10種類のフォルダに分けた学習用のビットマップ画像(*.bmp)をたくさん用意しました

![image.png](https://qiita-image-store.s3.amazonaws.com/0/142847/10642725-9123-53e4-34e7-48decfe639ce.png)


## 操作方法

モードは２つあります

1. 学習をするモード
2. 学習したニュートラルネットワークを試すモード

![image.png](https://qiita-image-store.s3.amazonaws.com/0/142847/bec591e9-b3e4-927a-84ef-f9bc1054d825.png)

### 1. 学習をするモード の操作方法

上図 1 ~ 10 で指定したフォルダ内の 28 × 28 ピクセルの画像に対して学習を行います

**学習** とは 下図の赤字の数値を更新することを意味しています。

![image.png](https://qiita-image-store.s3.amazonaws.com/0/142847/b37bb684-6e73-8dcd-48d2-2cde4f5e3bdc.png)

**重みリセットボタン** をクリックすると 初期化します。

**学習開始ボタン** をクリックすると 上図 1 ~ 10 で指定したフォルダ内のすべての画像ファイルについて学習開始します


### 2. 学習したニュートラルネットワークを試すモード の操作方法

**判定ボタン** をクリックすると 判定したい画像ファイルを選択するウィンドウが表示されます

![image.png](https://qiita-image-store.s3.amazonaws.com/0/142847/9dab368d-4dc2-46fd-3adb-1b3e55a91651.png)

それまでに 学習したニュートラルネットワークによって 上図 1 ~ 10 から予測を示します。

![image.png](https://qiita-image-store.s3.amazonaws.com/0/142847/6611c7e6-d59d-0efb-f612-0fa7dd12410b.png)


## ニュートラルネットワークのしくみ

### 入力層

入力は 28×28ピクセルのビットマップの各ピクセルの青色の強度を入力層にします

![image.png](https://qiita-image-store.s3.amazonaws.com/0/142847/7b5d6b83-0973-99df-c86a-0737e6040b3a.png)


### 中間層

中間層は 32ノード × 3層 にします

下図の赤字は、重み係数で、前層の出力数分あります。

１つのノードの出力は ∑(前の層のすべての出力 × 重み係数) で出た数値(-0.762132364) を 1/(1 + e^-x) とした

![image.png](https://qiita-image-store.s3.amazonaws.com/0/142847/a80d368b-d0a7-32a7-99ca-da7facf1e125.png)

### 出力

出力層は 1 ~ 10 で それぞれの出力が 学習した 1 ~ 10 の可能性を示している
下図の場合、7番目が 0.977058131　で最大となっているので、このニュートラルネットワークスは
入力に対して、7番目が一番可能性があると判断している

![image.png](https://qiita-image-store.s3.amazonaws.com/0/142847/95e1556a-1ca6-3fae-1960-c70e1f41c52c.png)


## 学習方法

今ある入力に対してこのニュートラルネットワークスは 7番目が一番可能性があると判断しているが
実際は 4番目が 1 それ以外 が 0 が正解です

![image.png](https://qiita-image-store.s3.amazonaws.com/0/142847/2c9dc965-9cc8-94a0-a0df-ea5b7bc83d56.png)


出力層の出力 - 正解 を誤差 e として この誤差分 赤字の重み係数を更新します。

![image.png](https://qiita-image-store.s3.amazonaws.com/0/142847/e843dfc1-5aad-dbbd-8f01-cafbb175059d.png)


更新の方法は この本

<a href = "https://www.amazon.co.jp/%E3%83%8B%E3%83%A5%E3%83%BC%E3%83%A9%E3%83%AB%E3%83%8D%E3%83%83%E3%83%88%E3%83%AF%E3%83%BC%E3%82%AF%E8%87%AA%E4%BD%9C%E5%85%A5%E9%96%80-Tariq-Rashid/dp/4839962251/">
<img src="https://images-na.ssl-images-amazon.com/images/I/510KftviWhL._SX350_BO1,204,203,200_.jpg" width=40%> </a>

ざっくり説明すると、**後層の誤差を 重みの比率によって今の数値に加算する**


