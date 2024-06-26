# Chatbot UI For miibo

このプロジェクトは [chatbot-ui](https://github.com/mckaywrigley/chatbot-ui) のフォークです。

オリジナルの作成者は [Mckay Wrigley](https://github.com/mckaywrigley) です。

このフォークは [CTD Networks co.,Ltd](https://ctd.co.jp/) によって資金提供され、特定の機能追加と修正は [Flexsystems Inc.](https://www.flexsystems-inc.com/) によって行われました。

## 追加した機能
- chatbot-uiからmiiboを呼び出せるように機能を追加しました。

## 必要スペック
- CPU：2コア以上
- メモリ：4GB以上（16GB以上を推奨）
- ストレージ：40GB以上（100GB以上を推奨）

## Chatbot UIインストール・セットアップ

### 1. ubuntuインストール
ubuntuデスクトップをインストールする。
※今回は、バージョン「22.04.4」をインストールする

### 2. chatbot-uiクローン
1. アップデートできるパッケージを確認する。
```
sudo apt update
```

2. 必要なパッケージをインストールする。（Git、Curl）
```
sudo apt install git curl
```

3. chatbot-uiをクローンする
```
git clone https://github.com/Flexsystems-inc/chatbot-ui-for-miibo.git
```

### 3. Node.jsインストール
1. nvmをインストールをする。※最新のnvmを取得する際は、 v0.38.0 の部分を必要に応じて更新すること
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
```

2. 設定更新をする。
```
source ~/.bashrc
```

3. Node.jsをインストールする。
```
nvm install node
```

4. Node.jsをビルドする。
```
cd chatbot-ui-for-miibo/
```
```
npm install next@latest
```
```
npm run build
```

### 4. Docker Engineインストール
1. 古いバージョンのDockerを削除する。
```
sudo apt remove docker docker-engine docker.io containerd runc
```

2. パッケージリストを更新し、Dockerインストール用の依存関係をインストールする。
```
sudo apt update
```
```
sudo apt install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common
```

3. Docker公式のGPGキーを追加する。
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

4. Dockerのリポジトリを追加する。
```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

5. パッケージリストを更新し、Docker Engineをインストールする。
```
sudo apt update
```
```
sudo apt install docker-ce docker-ce-cli containerd.io
```

6. `docker.sock` の実行グループに権限を割り当てる。
```
sudo chown $(whoami) ///var/run/docker.sock
```

7. Dockerが正しくインストールされたかを確認する。
```
sudo docker --version
```

### 5. 依存関係インストール
1. ディレクトリの移動する。
```
cd chatbot-ui-for-miibo
```

2. 依存パッケージのインストールする。
```
npm install
```

### 6. Homebrewインストール
1. 必要なパッケージをインストールする。
```
sudo apt install build-essential procps file
```

2. Homebrewをインストールする。
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

※インストールが完了すると下記のメッセージが出力される
```ログ
==> Installation successful!

==> Homebrew has enabled anonymous aggregate formulae and cask analytics.
Read the analytics documentation (and how to opt-out) here:
  https://docs.brew.sh/Analytics
No analytics data has been sent yet (nor will any be during this install run).

==> Homebrew is run entirely by unpaid volunteers. Please consider donating:
  https://github.com/Homebrew/brew#donations

==> Next steps:
- Run these two commands in your terminal to add Homebrew to your PATH:
    (echo; echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"') >> /home/$(whoami)/.bashrc
    eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
- Install Homebrew's dependencies if you have sudo access:
    sudo apt-get install build-essential
  For more information, see:
    https://docs.brew.sh/Homebrew-on-Linux
- We recommend that you install GCC:
    brew install gcc
- Run brew help to get started
- Further documentation:
    https://docs.brew.sh
```

3. インストールメッセージの「Next steps」を実行する。
- PATHにHomebrewを追加します。
※(`/home/$(whoami)/.bashrc`は各環境のPATHを指定)
```
(echo; echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"') >> /home/$(whoami)/.bashrc
```
```
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
```

- Homebrewの依存パッケージをインストールする。
```
sudo apt-get install build-essential
```

- GCCをインストールする。
```
brew install gcc
```

4. Homebrewバージョンを確認して、pathが通っていることを確認する。
```
brew --version
```

### 7. Supabase CLIインストール
1. Supabase CLIをインストールする。
```
brew install supabase/tap/supabase
```

2. Supabaseの起動する。
```
supabase start
```

3. 設定値を取得する。
```
supabase status
```
- `API URL`
- `anon key`
- `service_role key`

### 8. 環境設定
1. `.env.local`ファイルを作成する。
```
cp .env.local.example .env.local
```

2. `.env.local`ファイルに設定を追加する。
```
vi .env.local
```

- `NEXT_PUBLIC_SUPABASE_URL`：上記6-3の`API URL`のIP部分を自身のIPに置き換えて設定
```例
# Supabase Public
NEXT_PUBLIC_SUPABASE_URL=http://XXX.XXX.XXX.XXX:54321`
```
- `NEXT_PUBLIC_SUPABASE_ANON_KEY`：上記6-3の`anon key`を設定
- `SUPABASE_SERVICE_ROLE_KEY`：上記6-3の`service_role key`を設定

### 9. Chatbot UI 起動
1. Chatbot UIを起動する
```
npm run chat
```

## miiboの設定

### エージェントの作成～公開
株式会社miiboが作成している公式サイトを参考に、miiboにてエージェントの作成、公開及びAPIの有効化を行ってください。
- エージェントの作成
- エージェントの公開
- APIの有効化

## Chatbot UIの設定
### miiboとの接続

"+ New Model"の接続情報に以下値を設定してください。

`Name`：`miibo`と設定
※miibo以外の値を設定するとmiiboと接続できませんのでご注意ください。

`Model ID`：miiboの"APIの設定"に記載してある`AGENT ID`の値を設定

`Base URL`：`https://api-mebo.dev/api`と設定

`API Key`：miiboの"APIの設定"に記載してある`API KEY`の値を設定

`Max Context Length`：使用しません

## その他

その他情報はオリジナルのリポジトリである [chatbot-ui](https://github.com/mckaywrigley/chatbot-ui) をご確認ください。

## 問合せ先

Mckay様への連絡 [Twitter/X](https://twitter.com/mckaywrigley)

フレックシステムズ株式会社への連絡 [Contact Us](https://www.flexsystems-inc.com/contact/)
