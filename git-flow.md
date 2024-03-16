# 開発~リリースの流れ

## ブランチ戦略

基本的にはGit-flowでブランチ戦略を行います。

ブランチは下記のように作成とマージを行います。

![Alt text](image/branch.drawio.svg)



## 開発~リリースの流れ

### 通常時
1. リリースバージョンで`develop/{latest}`のブランチからリリースバージョンでブランチを作成`develop/{release-version}`
1. `develop/{release-version}`から開発ブランチを作成`feature/{backlogId}`
1. 開発完了後`develop/{release-version}`へPRを作成、PR完了後マージ・`job20s_web_dev`へ`github actions`で自動デプロイ
1. ある程度機能がまとまったらディレクターに単体テストを依頼
    1. 修正や仕様変更があれば課題を立てて2~4を行う
1. `staging/{latest}`のブランチからリリースバージョンでブランチを作成`staging/{release-version}`
1. リリース予定の機能が全て`develop/{release-version}`にマージされたら`staging/{release-version}`へPRを作成してマージ
1. 最新の`staging`ブランチにリリースとタグにリリースバージョンを作成・作成後`ci/cid`で自動デプロイ
1. `master/{latest}`のブランチからリリースバージョンでブランチを作成`main/{release-version}`
1. `main/{release-version}`へPRを作成
    1. staging環境でディレクターに総合テストをしてもらい問題なければ承認
        1. 修正がある場合は`develop`
    1. エンジニア１人に競合など起きていないか確認してもら承認
1. 承認完了後`main/{release-version}`へマージ
1. `main/{release-version}`ブランチにリリースとタグにリリースバージョンを作成・作成後`ci/cid`で自動デプロイ

### バグ対応時
1. デプロイされている最新のリリースバージョンの`main/{release-version}`ブランチから`hotfix/{release-version+0.0.1}`(マイナーバージョンを上げる)を作成
1. 修正完了後、`staging/{release-version+0.0.1}`ブランチへ反映（内容によってPRを作成）
1. `master/{latest}`のブランチからリリースバージョンでブランチを作成`main/{release-version+0.0.1}`
1. `main/{release-version}`へPRを作成
    1. staging環境でディレクターに総合テストをしてもらい問題なければ承認
1. 承認完了後`main/{release-version}`へマージ
1. `main/{release-version}`ブランチにリリースとタグにリリースバージョンを作成・作成後`ci/cid`で自動デプロイ
1. `develop/{latest}`へPRを作成・マージ

![Alt text](image/develop-to-release-flow.drawio.svg)

## Gitコマンドルール

コミット履歴のログを綺麗に保つために`git rebase`での運用とします。
基本的な内容としては`git pull`を使用せず`git pull --rebase`を使用してください。

`develop/{release-version}`に他の人の開発内容が入ってくるため、`git rebase origin {git rebase}`を細かく行ってローカルにも常に反映するように意識してもらえると良いかと思います。

