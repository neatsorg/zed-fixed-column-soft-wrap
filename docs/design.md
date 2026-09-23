# 設計メモ

## 動作

`fixed_column` は `preferred_line_length` を表示桁数として使い、エディタのビューポート幅を折り返し位置の計算に使わない。長い英単語や URL も文字単位で折り返す。

表示幅は UTF-8 のバイト数ではなく Unicode の表示セルで数える。ASCII は1、東アジア文字などの全角文字は2、結合文字は0として扱う。絵文字や結合文字列は grapheme cluster の途中で分割しない。

行末に全角文字が入りきらない場合は、その文字を次の表示行へ送る。したがって最大値は超えないが、行末が設定値より1桁短くなることはある。

## 実装箇所

- `crates/editor/src/editor.rs`: `SoftWrap` に固定桁モードを追加する。
- `crates/editor/src/element.rs`: 固定桁モードの wrap width とビューポート依存を分離する。
- `crates/gpui/src/text_system/line_wrapper.rs`: 単語境界を使わない wrap の分岐を追加する。
- `crates/gpui/src/text_system/line_layout.rs`: 表示幅・カーソル位置計算を同じ規則へ揃える。
- 設定スキーマとテスト: 設定値、半角、全角、混在、絵文字、長い単語、ウィンドウ幅変更を検証する。

描画だけを変更せず、行数・カーソル移動・マウス位置・選択範囲の計算が同じ折り返し境界を共有することを受け入れ条件にする。

