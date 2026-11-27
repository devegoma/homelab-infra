# Proxmox の管理名に専用 DNS を使う

Proxmox の管理名を `ren.vspo.internal` とし、ノード名は維持する。Proxmox 上の専用コンテナに LAN 内の固定 IP アドレスを割り当て、CoreDNS の `hosts` プラグインで管理名を Proxmox ホストの管理用 IP アドレスに対応付ける。

光BBユニットの公開資料ではドメイン別の DNS 転送設定を確認できず、ルーターの追加も見送った。管理端末に `vspo.internal` の問い合わせを CoreDNS に送るルールを設定し、インターネット上の名前は既存の DNS で引く。これにより Proxmox の停止による名前解決への影響を `vspo.internal` に限る。

最初に Windows 端末1台で `https://ren.vspo.internal:8006` を開けることと、CoreDNS の停止中にもインターネット上の名前を引けることを確認する。HTTPS 証明書の設定は初回検証の後に扱う。
