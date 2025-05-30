# Dockerのフォアグラウンドモードとデタッチドモード

## 概要
コンテナの実行モードはアプリケーション運用の柔軟性と監視能力に直接影響する重要な設定です。

## 主要概念
Dockerコンテナは「フォアグラウンドモード」と「デタッチドモード」の2つの実行モードで起動でき、ユースケースに応じて適切に選択する必要があります。

## フォアグラウンドモード

フォアグラウンドモードでは、コンテナはターミナルにアタッチされた状態で実行され、標準出力や標準エラー出力が直接ターミナルに表示されます。

### 使用方法
```bash
docker container run nginx
```

または明示的に指定する場合：
```bash
docker container run --attach STDOUT --attach STDERR nginx
```

### 特徴
- コンテナの出力がリアルタイムで表示される
- Ctrl+Cでコンテナを停止できる
- デバッグやログ監視に最適
- ターミナルセッションが終了するとコンテナも終了する

## デタッチドモード (バックグラウンドモード)

デタッチドモードでは、コンテナはバックグラウンドで実行され、ターミナルから切り離された状態になります。

### 使用方法
```bash
docker container run -d nginx
```

または完全な形式：
```bash
docker container run --detach nginx
```

### 特徴
- バックグラウンドでコンテナが実行される
- ターミナルは即座に制御を返し、他のコマンドを入力できる
- ターミナルセッションが終了してもコンテナは実行され続ける
- 長時間実行するサービスやデーモンに最適

## インタラクティブモードとの組み合わせ

### インタラクティブ + フォアグラウンド
```bash
docker container run -it nginx bash
```
- `-i`: インタラクティブモード（標準入力を開いたままにする）
- `-t`: 疑似TTY（ターミナル）を割り当てる

### インタラクティブ + デタッチド
```bash
docker container run -itd nginx
```
- コンテナはバックグラウンドで実行されるが、後から接続可能な状態になる

## 実行中コンテナの操作

### デタッチドモードで実行中のコンテナにアタッチする
```bash
docker container attach コンテナID
```
- 実行中のコンテナの標準入出力に接続する
- アタッチ中にCtrl+C を押すとコンテナが停止することに注意

### デタッチドモードで実行中のコンテナでコマンドを実行
```bash
docker container exec -it コンテナID bash
```
- コンテナ内で新しいプロセスを起動する
- コンテナを停止せずに操作可能

## ユースケース例

### フォアグラウンドモードの利用シーン
- アプリケーションのデバッグ中
- ログのリアルタイム監視が必要な場合
- 短時間の一時的な処理
- 開発中のテスト実行

### デタッチドモードの利用シーン
- Webサーバー（nginx）の運用
- データベースサーバーの運用
- バックグラウンドサービスの実行
- 本番環境での長時間実行されるアプリケーション

## まとめ
- フォアグラウンドモード：直接出力を確認しながら実行したい場合
- デタッチドモード：バックグラウンドでサービスとして実行したい場合
- 用途に応じて適切なモードを選択することでDockerコンテナを効率的に運用できる
