# Dify導入トレーニング - 2日目
## ローカル環境構築ハンズオン

---

## 📋 2日目の学習目標

この日の終わりまでに、以下のことができるようになります：
- Docker Desktopを正しくインストール・設定できる
- Difyローカル版を自分の環境で起動できる
- 基本的なトラブルシューティングができる
- なぜその手順が必要なのかを理解している

**重要：** 今日は実際に手を動かしながら、「なぜ」を理解することが目標です！

---

## 🛠️ Phase 1: Docker Desktop のインストールと設定

### 1.1 システム要件の確認

まず、なぜシステム要件を確認するのでしょうか？

**理由：** 
- Dockerは仮想化技術を使用するため、CPUとOSの対応が必要
- メモリ不足だと複数のコンテナが正常に動作しない
- ディスク容量が足りないとイメージのダウンロードで失敗する

#### Windows の場合
**確認方法：**
1. `Windows キー + R` → `winver` と入力 → Enter
2. システム情報を確認

**必要な条件：**
- Windows 10 64-bit: Pro, Enterprise, Education (Build 19041以上)
- Windows 11: 全エディション
- WSL 2 機能が利用可能
- Hyper-V機能が利用可能（Proエディション）
- メモリ: 4GB以上（推奨8GB以上）
- ディスク: 空き容量20GB以上

#### macOS の場合
**確認方法：**
1. 画面左上のAppleメニュー → 「このMacについて」

**必要な条件：**
- macOS 10.15以上
- Intel チップまたはApple Silicon（M1/M2）
- メモリ: 4GB以上（推奨8GB以上）
- ディスク: 空き容量20GB以上

### 1.2 Docker Desktop のダウンロード

#### なぜDocker Desktopなのか？
- **CLI版Docker**：コマンドラインのみ、上級者向け
- **Docker Desktop**：GUI付き、初心者に優しい、Windows/Mac対応

**ダウンロード手順：**
1. [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/) にアクセス
2. 自分のOSに合ったバージョンをクリック
3. ダウンロード完了まで待機

**ダウンロード中に理解しておくこと：**
- Docker Desktopには以下が含まれています：
  - Docker Engine（コンテナ実行エンジン）
  - Docker Compose（複数コンテナ管理）
  - Docker CLI（コマンドラインツール）
  - GUI管理画面

### 1.3 インストール実行

#### Windows の場合

**手順：**
1. ダウンロードした `.exe` ファイルを**右クリック** → **管理者として実行**
2. インストール画面で以下を確認：
   - ☑️ **Install required Windows components for WSL 2**
   - ☑️ **Add shortcut to desktop**
3. 「Install」をクリック
4. インストール完了後、**再起動を求められたら必ず再起動**

**なぜ管理者権限が必要？**
- Windows の仮想化機能（Hyper-V、WSL2）を有効化するため
- システムレベルの設定変更が必要なため

**なぜ再起動が必要？**
- WSL2（Windows Subsystem for Linux 2）の有効化
- 仮想化機能の初期化
- システム設定の反映

#### macOS の場合

**手順：**
1. ダウンロードした `.dmg` ファイルをダブルクリック
2. Docker アイコンを Applications フォルダにドラッグ
3. Launchpad または Applications フォルダから Docker Desktop を起動
4. 初回起動時、システム権限の許可を求められたら **許可**

**なぜシステム権限が必要？**
- ネットワーク設定の変更
- 仮想化機能へのアクセス
- 特権コンテナの実行サポート

### 1.4 初期設定と動作確認

#### Docker Desktop の起動

**Windows/Mac共通手順：**
1. Docker Desktop を起動
2. 利用規約に同意（個人利用・教育目的なら無料）
3. アカウント作成は **Skip** で問題なし

**画面の説明：**
- **緑色のアイコン**：Docker が正常動作中
- **オレンジ色のアイコン**：起動中
- **赤色のアイコン**：エラー状態

#### 基本動作テスト

**なぜテストが必要？**
本格的な作業前に、Docker環境が正しく動作することを確認するため

**テスト手順：**
1. **コマンドプロンプト**（Windows）または **ターミナル**（Mac）を開く

**Windows:**
- `Windows キー + R` → `cmd` → Enter

**Mac:**
- `Command + Space` → `terminal` と入力 → Enter

2. 以下のコマンドを実行：

```bash
docker --version
```

**期待する結果例：**
```
Docker version 24.0.7, build afdd53b
```

**このコマンドの意味：**
- Docker CLIが正しくインストールされているか確認
- バージョン情報の表示

3. 実際のコンテナ実行テスト：

```bash
docker run hello-world
```

**期待する結果：**
```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

**このコマンドで何が起こっているか：**
1. `hello-world` イメージを Docker Hub からダウンロード
2. そのイメージからコンテナを作成
3. コンテナ内のプログラムを実行
4. メッセージを表示後、自動的にコンテナを停止

#### リソース設定の調整

**なぜ調整が必要？**
Difyは複数のコンテナを同時実行するため、十分なリソースが必要

**設定手順：**
1. Docker Desktop の **Settings**（歯車アイコン）をクリック
2. **Resources** → **Advanced** を選択
3. 以下のように設定：
   - **Memory**: 6GB以上（可能なら8GB）
   - **CPUs**: 2つ以上
   - **Disk image size**: 50GB以上
4. **Apply & Restart** をクリック

**各設定の意味：**
- **Memory**: コンテナが使用できる最大メモリ量
- **CPUs**: 並列処理できるCPU数
- **Disk image size**: Dockerイメージ保存用の仮想ディスクサイズ

---

## 🚀 Phase 2: Git とファイル準備

### 2.1 Git の確認とインストール

#### なぜGitが必要？
- DifyのソースコードはGitHub上で管理されている
- `git clone` コマンドで最新版を取得する必要がある
- バージョン管理により、問題が起きても元に戻せる

#### Git の確認

コマンドプロンプト/ターミナルで実行：
```bash
git --version
```

**結果パターン：**

**✅ インストール済みの場合：**
```
git version 2.41.0.windows.1
```
→ 次のステップに進む

**❌ インストールされていない場合：**
```
'git' は、内部コマンドまたは外部コマンド、
操作可能なプログラムまたはバッチ ファイルとして認識されていません。
```
→ インストールが必要

#### Git のインストール（必要な場合のみ）

**Windows の場合：**
1. [https://git-scm.com/download/win](https://git-scm.com/download/win) にアクセス
2. 自動ダウンロードが開始される
3. インストーラを実行、基本的に **Next** で進む
4. 重要な設定項目：
   - **Editor**: Visual Studio Code（推奨）
   - **PATH environment**: Git from the command line and also from 3rd-party software
   - **Line ending**: Checkout Windows-style, commit Unix-style line endings

**Mac の場合：**
通常はプリインストール済み。未インストールの場合：
1. ターミナルで `git` と入力
2. 自動的にXcode Command Line Toolsのインストールが開始
3. インストール完了まで待機

### 2.2 作業ディレクトリの準備

#### なぜ作業ディレクトリを準備？
- ファイルを整理して管理しやすくするため
- パスの問題を避けるため
- 他のプロジェクトとの混在を防ぐため

**作業フォルダの作成：**

**Windows の場合：**
```bash
# Cドライブ直下に作業フォルダを作成
cd C:\
mkdir dify-workshop
cd dify-workshop
```

**Mac の場合：**
```bash
# ホームディレクトリに作業フォルダを作成
cd ~
mkdir dify-workshop
cd dify-workshop
```

**各コマンドの意味：**
- `cd`: change directory（ディレクトリ移動）
- `mkdir`: make directory（ディレクトリ作成）
- `~`: ホームディレクトリの略称（Macのみ）

### 2.3 Dify ソースコードの取得

#### GitHub からのクローン

```bash
git clone https://github.com/langgenius/dify.git
```

**このコマンドで何が起こるか：**
1. GitHub上のDifyリポジトリにアクセス
2. 全てのファイルとバージョン履歴をダウンロード
3. `dify` フォルダを作成してファイルを配置

**実行結果例：**
```
Cloning into 'dify'...
remote: Enumerating objects: 15230, done.
remote: Counting objects: 100% (1543/1543), done.
remote: Compressing objects: 100% (1028/1028), done.
remote: Total 15230 (delta 234), reused 1456 (delta 147), pack-reused 13687
Receiving objects: 100% (15230/15230), 45.67 MiB | 1.23 MiB/s, done.
Resolving deltas: 100% (8234/8234), done.
```

#### ディレクトリ構造の確認

```bash
cd dify
dir    # Windows の場合
ls -la # Mac の場合
```

**主要なフォルダ・ファイル：**
- `api/`: バックエンドAPI
- `web/`: フロントエンドWeb画面
- `docker/`: Docker関連設定
- `README.md`: プロジェクト説明
- `docker-compose.yaml`: 複数コンテナの設定

---

## 🐳 Phase 3: Dify の構築と起動

### 3.1 Docker Compose 設定の理解

#### docker ディレクトリへの移動

```bash
cd docker
```

**なぜdockerディレクトリ？**
本格的なアプリケーションでは、開発用・本番用・Docker用など、用途別にファイルを分けるのが一般的です。

#### 設定ファイルの確認

```bash
dir           # Windows
ls -la        # Mac
```

**重要なファイル：**
- `docker-compose.yaml`: メインの構成設定
- `.env.example`: 環境変数のテンプレート
- `README.md`: Docker版の説明書

#### 環境設定ファイルの作成

```bash
# Windowsの場合
copy .env.example .env

# Macの場合  
cp .env.example .env
```

**なぜ.envファイルが必要？**
- パスワードやAPIキーなどの秘密情報を管理
- 環境により異なる設定を外部ファイル化
- セキュリティ向上（GitHubにアップロードされない）

#### .env ファイルの編集

**テキストエディタで開く：**

**Windows（メモ帳の場合）:**
```bash
notepad .env
```

**Mac（テキストエディタの場合）:**
```bash
open -e .env
```

**重要な設定項目：**

```bash
# 秘密鍵（変更推奨）
SECRET_KEY=your-secret-key-here

# データベース設定
POSTGRES_PASSWORD=difyai123456
POSTGRES_USER=postgres
POSTGRES_DB=dify

# 外部サービス設定（OpenAI等）
# 後で設定するので、今は空のまま
OPENAI_API_KEY=
```

**各項目の意味：**
- `SECRET_KEY`: アプリケーションの暗号化に使用
- `POSTGRES_*`: データベース接続情報
- `OPENAI_API_KEY`: 外部AI APIのキー（後で設定）

**注意：** パスワードは本番環境では必ず変更してください！

### 3.2 docker-compose.yaml の理解

#### ファイル内容の確認

**重要部分の抜粋理解：**

```yaml
services:
  # API サーバー
  api:
    image: langgenius/dify-api:latest
    ports:
      - "5001:5001"
    
  # Web フロントエンド  
  web:
    image: langgenius/dify-web:latest
    ports:
      - "3000:3000"
      
  # データベース
  db:
    image: postgres:15-alpine
    ports:
      - "5432:5432"
```

**構成の意味：**
- **api**: バックエンドサーバー（ポート5001）
- **web**: フロントエンド画面（ポート3000）
- **db**: PostgreSQLデータベース（ポート5432）
- **redis**: キャッシュサーバー（ポート6379）
- **nginx**: プロキシサーバー（ポート80）

### 3.3 Docker イメージのダウンロード

#### なぜ事前ダウンロード？
起動時にダウンロードすると時間がかかり、エラーと勘違いする可能性があるため

```bash
docker-compose pull
```

**実行結果例：**
```
Pulling api ... done
Pulling web ... done  
Pulling db ... done
Pulling redis ... done
Pulling nginx ... done
```

**このプロセスで何が起こるか：**
1. Docker Hubから各サービスのイメージをダウンロード
2. ローカルに保存（次回以降は高速起動）
3. 依存関係のあるベースイメージも自動取得

### 3.4 Dify の起動

#### バックグラウンド起動

```bash
docker-compose up -d
```

**オプションの意味：**
- `up`: コンテナを起動
- `-d`: detached mode（バックグラウンド実行）

**期待する結果：**
```
Creating network "docker_default" with the default driver
Creating docker_redis_1 ... done
Creating docker_db_1    ... done
Creating docker_api_1   ... done
Creating docker_web_1   ... done
Creating docker_nginx_1 ... done
```

#### 起動状態の確認

```bash
docker-compose ps
```

**正常な結果例：**
```
      Name                    Command               State           Ports
docker_api_1      python -m gunicorn app:app ...   Up      0.0.0.0:5001->5001/tcp
docker_db_1       docker-entrypoint.sh postgres    Up      0.0.0.0:5432->5432/tcp
docker_nginx_1    nginx -g daemon off;             Up      0.0.0.0:80->80/tcp
docker_redis_1    docker-entrypoint.sh redis ...   Up      0.0.0.0:6379->6379/tcp
docker_web_1      docker-entrypoint.sh /bin/sh     Up      0.0.0.0:3000->3000/tcp
```

**State列の意味：**
- **Up**: 正常動作中
- **Exit**: 停止状態（問題の可能性）
- **Restarting**: 再起動中（一時的なら正常）

---

## 🌐 Phase 4: 動作確認とアクセス

### 4.1 Webアクセスの確認

#### ブラウザでのアクセス

**アクセスURL:**
```
http://localhost
```

**初回アクセス時に表示されるもの:**
1. **Difyセットアップ画面**
2. **言語選択**（日本語対応）
3. **管理者アカウント作成**

#### 管理者アカウントの作成

**入力項目:**
- **管理者名**: 任意（例：Admin）
- **メールアドレス**: 任意（例：admin@example.com）
- **パスワード**: 8文字以上の強力なパスワード

**なぜ管理者アカウントが必要？**
- システム設定の変更権限
- ユーザー管理機能
- アプリケーション作成権限

### 4.2 基本機能の動作確認

#### ダッシュボードの確認

ログイン後、以下の画面が表示されるはずです：
- **アプリケーション一覧**
- **作成ボタン**
- **設定メニュー**

#### 簡単なチャットボット作成テスト

**手順:**
1. **「アプリを作成」**をクリック
2. **「チャットボット」**を選択
3. **アプリ名**: 「テストボット」と入力
4. **「作成」**をクリック

**この時点でエラーが出る可能性:**
```
LLMプロバイダーが設定されていません
```
→ これは正常です。外部AIサービスの設定はまだ行っていません。

### 4.3 ログとトラブルシューティング

#### ログの確認方法

**全サービスのログ:**
```bash
docker-compose logs
```

**特定サービスのログ:**
```bash
docker-compose logs api
docker-compose logs web
docker-compose logs db
```

**リアルタイムログの監視:**
```bash
docker-compose logs -f api
```
（Ctrl+C で終了）

#### よくあるエラーと対処法

**❌ ポート80が使用中**
```
Error: Port 80 is already in use
```

**対処法:**
1. 他のWebサーバー（Apache、IIS等）を停止
2. または、ポート番号を変更

**ポート変更方法:**
`docker-compose.yaml` の nginx 部分を編集：
```yaml
nginx:
  ports:
    - "8080:80"  # 80を8080に変更
```

その後、`http://localhost:8080` でアクセス

**❌ メモリ不足**
```
Error: Cannot start service api: insufficient memory
```

**対処法:**
1. Docker Desktop の設定でメモリを増加
2. 他のアプリケーションを終了
3. 不要なDockerコンテナを停止

**❌ データベース接続エラー**
```
Connection to database failed
```

**対処法:**
```bash
# 全てのコンテナを停止
docker-compose down

# ボリュームも含めて削除（データがリセットされます）
docker-compose down -v

# 再起動
docker-compose up -d
```

---

## 🔧 Phase 5: システム管理の基本

### 5.1 コンテナの操作

#### 停止と再起動

**停止:**
```bash
docker-compose stop
```

**完全停止（ネットワークも削除）:**
```bash
docker-compose down
```

**再起動:**
```bash
docker-compose restart
```

**特定サービスのみ再起動:**
```bash
docker-compose restart api
```

#### システムリソースの確認

**Docker全体の使用状況:**
```bash
docker system df
```

**実行中コンテナのCPU/メモリ使用率:**
```bash
docker stats
```

### 5.2 データの永続化確認

#### ボリュームの確認

```bash
docker volume ls
```

**Difyで使用されるボリューム:**
- **postgres_data**: データベースファイル
- **redis_data**: キャッシュデータ  
- **app_data**: アップロードファイル等

**なぜボリュームが重要？**
コンテナを削除してもデータが残るため、システム更新時にデータを失わない

### 5.3 バックアップの基本

#### データベースバックアップ

```bash
# データベースのバックアップ
docker-compose exec db pg_dump -U postgres dify > backup.sql
```

#### 設定ファイルのバックアップ

```bash
# .envファイルをコピー
copy .env .env.backup    # Windows
cp .env .env.backup      # Mac
```

---

## ✅ 2日目まとめ

### 今日達成したこと

**✅ 環境構築**
- Docker Desktopのインストール完了
- 基本動作確認済み
- システムリソース設定完了

**✅ Dify構築**
- ソースコードの取得完了
- 設定ファイルの理解と編集
- 全サービスの起動成功

**✅ 動作確認**
- Webアクセス確認
- 管理者アカウント作成
- 基本画面の動作確認

**✅ 運用知識**
- ログ確認方法の習得
- トラブルシューティング基本スキル
- バックアップ手順の理解

### 理解できたこと

**Docker の仕組み**
- コンテナとイメージの関係
- Docker Composeによる複数サービス管理
- ポートマッピングとネットワーク

**Dify の構成**
- マイクロサービスアーキテクチャ
- 各コンポーネントの役割
- 設定ファイルの重要性

### 明日への準備

**3日目の内容:**
- 外部AIサービス（OpenAI等）の設定
- エージェントの作成実習
- 実際のユースケース実装

**事前準備（任意）:**
- OpenAI APIキーの取得
- 作成したいエージェントの用途検討
- 必要なデータや資料の準備

### 最終チェック

以下を確認して今日を終了しましょう：

- [ ] `http://localhost` でDifyにアクセスできる
- [ ] 管理者でログインできる
- [ ] `docker-compose ps` で全サービスがUp状態
- [ ] 基本的なトラブルシューティング方法を理解

お疲れさまでした！明日は実際にAIエージェントを作成します！