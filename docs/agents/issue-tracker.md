# 課題管理

`devegoma/homelab-infra` の GitHub Issues は `gh` CLI で操作する。対象のリポジトリを明示するため、課題を操作するコマンドには `--repo devegoma/homelab-infra` を付ける。

- 課題や仕様を登録するには `gh issue create --title "..." --body-file <file>` を使う。
- チケットを取得するには `gh issue view <number> --json number,title,body,labels,comments` を使う。
- チケットを一覧表示するには `gh issue list` を使う。必要に応じて `--state` や `--label` で絞り込む。
- コメントの投稿には `gh issue comment` を使う。ラベルの追加や削除には `gh issue edit` に `--add-label` または `--remove-label` を指定する。
- チケットを `gh issue close` で閉じる前に、対応結果を `gh issue comment` で投稿する。
- 子チケットは GitHub のサブ課題機能で関連付け、作業を妨げている課題は依存関係として登録する。該当する機能が使えない場合は、子チケットの本文の先頭に `Part of #<number>` または `Blocked by: #<number>` を記載する。作業を妨げている課題がすべて閉じたら、そのチケットに着手できる。

## プルリクエストでの依頼受付

プルリクエストでは依頼を受け付けない。

## Wayfinding の進め方

計画をまとめるために `wayfinder:map` ラベルを付けた課題を一つ使い、各チケットを子課題として関連付ける。子チケットの種類は `wayfinder:research`、`wayfinder:prototype`、`wayfinder:grilling`、`wayfinder:task` のいずれかで示す。

計画に記載された順に確認し、未完了で、作業を妨げる未解決の課題がなく、担当者が未設定の子チケットを最初に一つ選ぶ。着手前に自分を担当者に設定する。結果をコメントに記録してチケットを閉じ、計画に決定事項を簡潔に記載してチケットへのリンクを添える。
