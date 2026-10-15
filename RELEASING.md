# 新しい版の出しかた

サイト（https://sota56.com/56craft）は、このリポジトリの Releases から版の一覧を自動で取り、
名前に `-World-` が入った zip を直接ダウンロードさせます。次のルールを守ってください。

## ルール

- 版の名前（タグ）: `26.3-beta.3` のように「マイクラのバージョン-ベータ番号」
- リリースは **通常のリリース** で出す（Pre-release にしない）。最新版が一覧の先頭・「（最新）」になります
- 添付する zip は1つだけ。名前は `56CRAFT-First-Steps-World-<タグ>.zip`
- zip の **直下** にワールドの中身（`level.dat`、`datapacks/`、`region/` など）を置く。
  包むフォルダ、zip の中の zip、`__MACOSX`、`.DS_Store` は入れない
  - こうすると、Windows の「すべて展開」でも Mac のダブルクリック（Safari の自動展開）でも、
    zip と同じ名前のフォルダができ、その中にすぐ `level.dat` がある形になる。
    そのフォルダを saves に入れるだけで遊べ、版ごとに別のワールドになるので前の進み具合も消えない
- サイトに出したくなくなった古い版は、削除ではなく「下書き（Draft）」に戻す（あとで戻せる）

## 作り方と確認

```bash
cd <ワールドのフォルダ>            # level.dat があるフォルダ
zip -r -X ../56CRAFT-First-Steps-World-<タグ>.zip . -x '*.DS_Store'
cd ..
unzip -l 56CRAFT-First-Steps-World-<タグ>.zip | head      # 先頭に level.dat があること
mkdir -p /tmp/check && ditto -x -k 56CRAFT-First-Steps-World-<タグ>.zip /tmp/check/w && ls /tmp/check/w   # level.dat が見えること
gh release create <タグ> --repo SOTA56/mc-first-steps --latest --title "マイクラ はじめの一歩 <タグ>（ベータ版）" --notes "..." 56CRAFT-First-Steps-World-<タグ>.zip
```

サイトの一覧は最大10分ほどで新しい版に変わります。
