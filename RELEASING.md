# 新しい版の出しかた

サイト（https://sota56.com/56craft）は、このリポジトリの Releases から版の一覧を自動で取り、
添付された zip を直接ダウンロードさせます。導入方法には、その zip の名前と、
展開してできるフォルダの名前がそのまま表示されます。次のルールを守ってください。

## ルール

- 版の名前（タグ）: `26.3-beta.3` のように「マイクラのバージョン-ベータ番号」
- リリースは **通常のリリース** で出す（Pre-release にしない）。最新版が一覧の先頭・「（最新）」になる
- 添付する zip は **1つだけ**。名前は `56CRAFT_FirstSteps_<タグ>_package.zip`
- zip の直下には **ちょうど2つ** だけを入れる
  - `56CRAFT_FirstSteps_<タグ>_world/` フォルダ（この中の直下に `level.dat`、`datapacks/` などワールドの中身）
  - `README.txt`（9言語。入れ方は下の「README の入れ方（正本）」に合わせる）
- zip の中の zip、`__MACOSX`、`.DS_Store` は入れない
- サイトの導入方法は `_package.zip` で終わる名前を見て、この形の手順を出す。名前の規則を変えるときはサイト側も直す
- サイトに出したくなくなった古い版は、削除ではなく「下書き（Draft）」に戻す（あとで戻せる）

なぜこの形か：直下に2つの物があるので、Windows の「すべて展開」でも Mac のダブルクリック（Safari の自動展開）でも
`…_package` フォルダが1つでき、その中に `…_world` と `README.txt` が並ぶ。手順が OS で変わらず、
「`…_world` を saves に入れる」と名前で指示できる。版ごとに別のワールドになるので、前の版の進み具合も消えない。

## 作り方と確認

```bash
V=26.3-beta.3
mkdir -p stage && cp -R <ワールドのフォルダ> stage/56CRAFT_FirstSteps_${V}_world && cp README.txt stage/
find stage -name .DS_Store -delete
(cd stage && zip -r -X ../56CRAFT_FirstSteps_${V}_package.zip 56CRAFT_FirstSteps_${V}_world README.txt -x '*.DS_Store')
unzip -Z1 56CRAFT_FirstSteps_${V}_package.zip | awk -F/ '{print $1}' | sort -u     # _world と README.txt の2つだけ
unzip -Z1 56CRAFT_FirstSteps_${V}_package.zip | grep "^56CRAFT_FirstSteps_${V}_world/level.dat$"
rm -rf /tmp/check && mkdir -p /tmp/check && ditto -x -k 56CRAFT_FirstSteps_${V}_package.zip /tmp/check/56CRAFT_FirstSteps_${V}_package && ls /tmp/check/*/ /tmp/check/*/*_world | head
gh release create $V --repo SOTA56/mc-first-steps --latest --title "マイクラ はじめの一歩 $V（ベータ版）" --notes "..." 56CRAFT_FirstSteps_${V}_package.zip
```

サイトの一覧は最大10分ほどで新しい版に変わります。

## README の入れ方（正本・日本語）

```
導入方法（Java 版 26.3）
はじめに：マイクラ（Java 版）を一度起動して、タイトル画面まで進めておきます。
1. ダウンロードした「…_package.zip」を展開します。
   Windows：zip を右クリック →「すべて展開」→「展開」／Mac：zip をダブルクリック
2. できた「…_package」フォルダを開きます。中に「…_world」フォルダと、この説明書が入っています。
3. saves フォルダを開きます。
   Windows：Windows キーを押しながら R を押し、「%appdata%\.minecraft\saves」を貼り付けて「OK」
   Mac：Finder のメニュー「移動」→「フォルダへ移動…」に「~/Library/Application Support/minecraft/saves」を貼り付けて Return
4. 「…_world」フォルダを、フォルダごと saves に入れます。
   saves →「…_world」と開いて、すぐ level.dat が見えれば正しい形です。
5. マイクラの「シングルプレイ」を開くと、「マイクラ はじめの一歩」が出てきます。
```

マイクラの画面の表記（ja_jp）：「シングルプレイ」「設定…」「リソースパック…」「パックフォルダーを開く」「はい」「完了」。
ほかの言語の「シングルプレイ」: en Singleplayer / es Un jugador / fr Solo / de Einzelspieler / zh_cn 单人游戏 / zh_tw 單人遊戲 / ko 싱글플레이 / id Bermain Sendiri
