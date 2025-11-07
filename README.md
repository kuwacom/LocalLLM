# LocalLLM

このプロジェクトは、Docker Compose を使用してローカル環境で LLM (Large Language Model) を構築するためのものです  
OpenWebUI をインターフェースとして、SearXNG をウェブ検索エンジンとして、Ollama を LLM 実行環境として、Cloudflared をトンネリングツールとして統合しています

## プロジェクト概要

- **OpenWebUI**: ユーザーフレンドリーなウェブインターフェースを提供し、LLM とのチャットや RAG (Retrieval-Augmented Generation) 機能をサポート。
- **SearXNG**: プライバシーを重視したメタ検索エンジン。ウェブ検索結果を統合して提供。
- **Ollama**: ローカルで LLM を実行するためのツール。GPU 対応で高速処理が可能。
- **Cloudflared**: Cloudflare Tunnel を使用して、ローカル環境をインターネットに公開（オプション）。

## 必要条件

- Docker (バージョン 20.10 以上)
- Docker Compose (バージョン 2.0 以上)
- GPU 対応 (NVIDIA GPU の場合、NVIDIA Docker ランタイムが必要。CPU 版の場合は `docker-compose-cpu.yml` を使用)
- 最低 8GB の RAM (推奨 16GB 以上)

## インストールとセットアップ

1. このリポジトリをクローンします。

   ```bash
   git clone <repository-url>
   cd LocalLLM
   ```

2. 環境変数を設定します（オプション）。`.env` ファイルを作成し、Cloudflare Tunnel のトークンを設定してください。

   ```env
   CLOUDFLARE_TUNNEL_TOKEN=your_tunnel_token_here
   ```

3. Docker Compose を使用してサービスを起動します。

   - GPU 対応の場合:
     ```bash
     docker-compose up -d
     ```

   - CPU のみの場合:
     ```bash
     docker-compose -f docker-compose-cpu.yml up -d
     ```

4. 初回起動時は Ollama でモデルをダウンロードします。コンテナが起動したら、以下のコマンドでテキスト埋め込み用モデルをプルします。

   ```bash
   docker exec -it ollama ollama pull kun432/cl-nagoya-ruri-base:latest
   ```

    あとは利用したい他のモデルも追加してください。

## 使用方法

1. ブラウザで `http://localhost:80` にアクセスして OpenWebUI を開きます。

2. OpenWebUI のインターフェースでチャットを開始します。RAG 機能が有効になっているため、ウェブ検索を活用した応答が可能です。

3. SearXNG は `http://localhost:8080` で直接アクセス可能です。プライバシーを重視した検索が行えます。

4. Ollama はバックエンドで動作し、OpenWebUI から自動的に利用されます。

## 設定

### OpenWebUI の設定

- `docker-compose.yml` の `openwebui` サービスで環境変数を調整できます。
- 例: ログレベル、デバッグモード、RAG 関連の設定。

### SearXNG の設定

- `searxng/settings.yml` で検索エンジンの設定をカスタマイズできます。
- デフォルトで日本語検索が有効になっています。

### Cloudflared の設定

トンネリングを有効にする場合、`.env` ファイルに `CLOUDFLARE_TUNNEL_TOKEN` を設定してください。
これにより、ローカル環境をインターネットからアクセス可能になります。