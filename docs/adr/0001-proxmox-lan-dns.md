# Proxmox の管理名に専用 DNS を使う

Proxmox の管理名を `ren.vspo.internal` とし、ノード名は維持する。Proxmox 上の専用 VM に LAN 内の固定 IPv4 アドレスを割り当てる。CoreDNS の `hosts` プラグインで管理名を Proxmox ホストの管理用 IPv4 アドレスに対応付ける。

光BBユニットの公開資料ではドメイン別の DNS 転送設定を確認できなかった。ルーターを追加せず、Windows の管理端末に `vspo.internal` の問い合わせを CoreDNS に送るルールを追加する。インターネット上の名前は既存の DNS で引く。

## 初回実装

- Terraform の `bpg/proxmox` プロバイダーで、Ubuntu 24.04 LTS のクラウドイメージから専用 VM を作成する。固定 IPv4 アドレスとゲートウェイを指定し、Proxmox 起動時の VM 自動起動を有効にする。Ubuntu イメージと CoreDNS はバージョンとチェックサムを固定する。
- Cloud-Init で SSH 公開鍵、CoreDNS、`Corefile`、systemd サービス、ファイアウォールを設定する。Terraform の API トークンとスニペット転送用の SSH 秘密鍵は実行環境から渡す。Terraform の状態ファイルはローカルに保管し、バックアップする。
- CoreDNS は `vspo.internal` を扱い、上流 DNS への転送は設定しない。光BBユニットで検証用 Windows 端末の IPv4 アドレスを固定し、VM の TCP・UDP 53 番への接続はその端末だけに許可する。
- CoreDNS の応答を確認してから、Windows に PowerShell で `.vspo.internal` の名前解決ルールを追加する。設定、検証、追加したルールの解除手順を記録する。
- DNS レコードを変更するときは、Git 上の設定を更新し、稼働中の VM に反映して応答を確認する。VM の再作成には同じ設定を使う。

## 合格条件

- PR ごとに GitHub Actions で `terraform fmt -check -recursive`、`terraform validate`、TFLint の推奨ルール、生成した Cloud-Init 設定のスキーマ検査を実行する。CI に Proxmox の認証情報は渡さない。
- Terraform の模擬プロバイダーを使い、固定 IPv4 アドレス、ゲートウェイ、自動起動、Cloud-Init の関連付けを VM の作成計画で検査する。合成データから生成した `Corefile` を本番と同じバージョンの CoreDNS に読み込ませ、管理名に指定した IPv4 アドレスが返ること、未登録名に A レコードが返らないこと、外部名の問い合わせが転送されないことを確認する。期待する振る舞いをテストに書き、失敗を確認してから実装する。
- 実機では Cloud-Init の初回起動後に、Windows のルールの追加と解除、`https://ren.vspo.internal:8006` への接続、許可外端末からの DNS 接続の拒否を確認する。CoreDNS を停止してキャッシュを消去した状態でもインターネット上の名前を引けることと、VM の再起動で CoreDNS が復旧することを確認する。HTTPS 証明書は初回検証後に扱う。
