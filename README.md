# YahooStoreApi

Yahoo!ショッピング プロフェッショナル出店ストア向けAPIを簡単に叩けるrubyラッパーです。

現在開発中です🐛 まだ商品情報の取得と在庫情報の取得しかできません。順次機能追加をしていきたいと思います。

## Installation

```ruby
# 準備中
# gem 'yahoo_store_api'
```

## 事前準備

事前にストア出品アカウントと連携した認証コードが必要になります。

まず、Yahooデベロッパーネットワークの[Yahoo!ショッピングのストア運営をサポートするAPIを利用したアプリケーション](https://e.developer.yahoo.co.jp/shopping/register)にアプリケーションを登録します。

登録が終えたら、次に[認可コードを取得](https://developer.yahoo.co.jp/yconnect/server_app/explicit/authorization.html)する必要があります。ページに表示するかリダイレクトした時のパラメーターにある認証コードを取得します。

※シンプルにページ上に認証コードを表示させる例: https://auth.login.yahoo.co.jp/yconnect/v1/authorization?response_type=code+id_token&client_id=[上で登録したアプリケーションID]&state=foobar&redirect_uri=oob&nonce=hogehoge

stateやnonceなどは[Authorizationエンドポイント](https://developer.yahoo.co.jp/yconnect/server_app/explicit/authorization.html)に詳細がありますので、適宜変更してください。

## Initialize

ストアアカウント、上で登録したアプリケーションid、シークレット、取得した認証コードでインスタンスを生成します。

```ruby
client = RmsItemApi::Client.new(
  seller_id: YOUR_STORE_ID, # ストアアカウントid
  application_id: YOUR_APPLICATION_ID, # アプリケーションid
  application_secret: YOUR_SECRET, # シークレット
  authorization_code: AUTHORIZATION_CODE # 認証コード
)
```

なお、この認証コードは1回のみ使用可能で、次からはリフレッシュトークンを使用するか、再度認証コードを取得する必要があります。リフレッシュトークンは生成したインスタンスから取得する事ができます。

```ruby
puts client.refresh_token
```

```ruby
client = RmsItemApi::Client.new(
  seller_id: YOUR_STORE_ID, # ストアアカウントid
  application_id: YOUR_APPLICATION_ID, # アプリケーションid
  application_secret: YOUR_SECRET, # シークレット
  refresh_token: YOUR_REFRESH_TOKEN # リフレッシュトークン
)
```

## Usage

### 商品情報の参照

ストアに登録されている商品情報を取得できます。

```ruby
item = client.get_item('test123')
```

### 在庫情報の参照

ストアに登録されている商品の在庫情報を取得できます。

```ruby
stock = client.get_stock('test123')
```
