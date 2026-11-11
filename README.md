# 学習ログ

HTML/CSS/JavaScriptとlocalStorageだけで動く、小さな学習ログアプリです。

日付ごとに「今日の調子・集中度」と「今日の学習メモ」を保存できます。数学の復習、英単語の学習、読書メモなどを記録して、これまでの学習を見返せます。保存した記録はブラウザのlocalStorageに残ります。

## 📱 デモ

🔗 [https://gigaschool.github.io/tiny-diary/](https://gigaschool.github.io/tiny-diary/)

![📷 image.png](image.png)

## デモの動かし方

`index.html` をブラウザで開くだけで使えます。ビルドやサーバー起動は不要です。

```text
tiny-diary/
├── index.html
├── styles.css
├── app.js
├── README.md
└── LICENSE
```

## 機能

- 今日の日付を自動セット
- 学習の調子・集中度を5種類から選択
- 今日の学習メモを保存
- 日付ごとに1件の記録として保存
- 同じ日付で保存すると上書き
- 学習記録の一覧表示
- 保存済みログの編集
- 保存済みログの削除
- メモ検索
- 調子フィルター
- 最近7件の集中度の流れを表示する波形グラフ

## 使用技術

- HTML
- CSS
- JavaScript
- localStorage

外部ライブラリは使っていません。

## localStorage

保存キーは `tiny-diary.entries.v1` です。

データは次のような配列で保存されます。

```json
[
  {
    "id": "example-id",
    "date": "2026-04-29",
    "mood": "happy",
    "note": "一次関数を復習。英単語を10個暗記。読書メモを3行に整理。",
    "createdAt": "2026-04-29T10:00:00.000Z",
    "updatedAt": "2026-04-29T10:00:00.000Z"
  }
]
```

## 調子・集中度データ

画面上では学習時の調子として表示します。保存データとの互換性を保つため、値は従来のキーを使用します。

| 保存値 | 表示 | スコア |
| --- | --- | --- |
| `happy` | 集中できた | 5 |
| `calm` | まあまあ | 4 |
| `normal` | いつも通り | 3 |
| `tired` | 少し疲れた | 2 |
| `sad` | 集中しづらい | 1 |

集中度のふり返りでは、最近7件の記録をこのスコアに変換して、SVGの波形グラフとして表示します。

## 教材で説明しやすい流れ

1. フォームから値を受け取る
2. JavaScriptの配列に学習記録を追加する
3. `JSON.stringify` でlocalStorageへ保存する
4. `JSON.parse` でlocalStorageから読み込む
5. 配列をもとに一覧と集中度のふり返りを再描画する

## ファイル構成

- `index.html`: 画面の構造
- `styles.css`: レイアウトと見た目
- `app.js`: 保存、表示、編集、削除、検索、集中度グラフの描画
- `README.md`: この説明ファイル
- `LICENSE`: MITライセンス

## ライセンス

MIT License
