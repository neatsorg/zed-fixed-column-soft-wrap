# zed-fixed-column-wrap

公式 Zed の soft wrap を拡張し、単語境界ではなく固定桁数で折り返すためのパッチリポジトリ。

目標は、設定値を表示幅として扱うことです。たとえば `80` 桁なら、半角文字は最大80文字、全角文字は最大40文字、混在時は半角1・全角2の表示幅合計が80になる位置で折り返します。ファイル内容には改行を挿入せず、ウィンドウ幅では折り返し位置を変えません。

## 現在の状態

これは実装前の準備段階です。公式 Zed の基準コミット、パッチ系列、再現用 checkout の作成スクリプト、仕様と検証項目を固定しています。

## 準備と検証

```sh
./scripts/prepare
./scripts/check
```

`prepare` は [sources.lock.toml](sources.lock.toml) の commit から `.checkout/zed` を作成し、`patches/series` の各パッチを順に適用します。`check` は作業ツリーの差分と Rust の対象 crate を確認します。パッチがまだないため、現段階の check は基準 checkout の検証までを行います。

## 想定する設定

```json
{
  "soft_wrap": "fixed_column",
  "preferred_line_length": 80
}
```

既存の `none`、`editor_width`、`preferred_line_length`、`bounded` は変更せず、新しいモードだけを追加する方針です。設定名と既存バージョンとの互換性は実装時に確定します。

