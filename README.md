# open-data-spaces-sdk-mock-server

ODS SDK for Onboarding - Mock server

## 概要

このプロジェクトでは、OSSのモックサーバー構築ツールである [Mockoon](https://mockoon.com/) を使用して、
ODSコンポーネントのモックサーバーを実行するための手順や設定ファイルを提供します。

## 前提条件

本リポジトリでは、以下のバージョンの ODS コンポーネントのモックサーバを提供します。

* [アイデンティティコンポーネント](https://github.com/open-dataspaces/L3-identity-component): [v1.0.0](https://github.com/open-dataspaces/L3-identity-component/tree/v1.0.0)
* [精算・決済サービス](https://github.com/open-dataspaces/DCS-Payment): [v1.0.0](https://github.com/open-dataspaces/DCS-Payment/tree/v1.0.0)

## 準備

Mockoon CLI を、以下のコマンドでインストールしてください。

```bash
npm install -g @mockoon/cli
```

## モックサーバーの起動

以下のコマンドで各APIのモックサーバーを起動できます。

### L3 API (Port: 3001)

```bash
mockoon-cli start --data ./mockoon-l3.json
```

### Payment API (Port: 3002)

```bash
mockoon-cli start --data ./mockoon-payment.json
```

## 定義ファイルについて

- `mockoon-l3.json`: `apidoc/L3/api-docs.yaml` から生成
- `mockoon-payment.json`: `apidoc/payment/openapi.json` から生成

## カスタマイズ方法

Mockoonの挙動は、定義ファイル（`mockoon-*.json`）を直接編集するか、Mockoon GUIを使用してカスタマイズできます。

### 1. ポート番号の変更

デフォルトでは L3 API は `3001`、Payment API は `3002` に設定されています。

#### 定義ファイルの編集

各JSONファイルの冒頭にある `"port"` フィールドの値を変更してください。

```json
"port": 3001,
```

#### CLIでの一時的な変更

起動時に `--port` オプションを指定することで、定義ファイルを書き換えずにポートを変更できます。

```bash
mockoon-cli start --data ./mockoon-l3.json --port 4000
```

### 2. レスポンスの変更（ステータスコード・ボディ）

特定のAPIエンドポイントのレスポンスを変更するには、JSON内の `"routes"` 配下の `"responses"` を編集します。

- **ステータスコード**: `"statusCode"` の値を変更します（例: `200` -> `404`）。
- **レスポンスボディ**: `"body"` フィールドの中身を編集します。
- **レスポンスヘッダー**: `"headers"` 配下のリストを編集します。

### 3. 挙動のカスタマイズ

- **遅延（Latency）**: ネットワーク遅延をシミュレートするには、`"latency"` フィールドにミリ秒単位で数値を設定します。

  - 全体設定: JSONルートの `"latency"`
  - ルート別設定: 各レスポンス配下の `"latency"`

- **ルールの設定**: 特定のクエリパラメータやヘッダーに基づいてレスポンスを切り替えるには、`"rules"` フィールドを編集します（GUIでの設定を推奨します）。

### 4. 動的なデータの生成（テンプ​​レーティング）

MockoonはHandlebarsを使用したテンプ​​レーティングをサポートしています。
例：リクエストボディの値をレスポンスに含める

```json
"body": "{\"received_id\": \"{{body 'id'}}\"}"
```

詳細は[Mockoon Documentation](https://mockoon.com/docs/latest/templating/overview/)を参照してください。

## Mockoon GUIでの利用

Mockoonのデスクトップアプリケーションをお使いの場合は、`mockoon-*.json` ファイルをインポートして利用することも可能です。

## ライセンス

- 本リポジトリはMITライセンスで提供されています。
- ソースコードおよび関連ドキュメントの著作権は株式会社NTTデータグループ、株式会社NTTデータに帰属します。

## 免責事項

- 本リポジトリの内容は予告なく変更・削除する可能性があります。
- 本リポジトリの利用により生じた損失及び損害等について、いかなる責任も負わないものとします。
