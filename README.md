# OpneIDConnect / OAuth2.0 説明資料

## 0. 認証・認可の概要

#### OAuth2.0

アクセストークンを発行するための仕様です。  
アプリケーション側は定められた手順でアクセストークンを取得する必要があります。

#### OpenIDConnect

ID トークンを発行するための仕様です。  
アプリケーション側は定められた手順で ID トークンを取得する必要があります。  
※OAuth2.0 を拡張する形で開発されたものです

#### アクセス トークン

トークン保持者がどのリソースにアクセスする権限を持っているか証明するもの（認可）

- JWT or 文字列形式
- scope の値でリソースへのアクセスを管理する

#### ID トークン

いつどこで誰がどの idP で認証されたかを証明するもの。  
アプリケーション側は ID トークンをチェックすることで、ユーザをログイン状態に遷移させることができる。

- JWT 形式
- 署名されているためクライアント側で署名検証することで改ざんを検知できる

## 1. 各種フロー

### 1.1 認可コードフロー

- 認可エンドポイントで認可コードを取得する
- トークンエンドポイントで ID トークン/アクセストークンを取得する

以下にシーケンス図を示す。
![authorization_code_flow](/img/authorization_code_flow.png "authorization_code_flow")

### 1.2 インプリシットフロー

- 認可エンドポイントで ID トークン/アクセストークンを取得する

![implicit_flow](/img/implicit_flow.png "implicit_flow")

### 1.3 ハイブリッドフロー

- 認可エンドポイントで認可コード + ID トークン/アクセストークンを取得する
- トークンエンドポイントで ID トークン/アクセストークンを取得する

TBD

### 1.4 PKCE(Proof Key for Code Exchange)

認可コード横取り攻撃を防止するための仕様。

- 認可リダイレクト時に、「乱数をハッシュ化した値(code_challenge)」と「ハッシュアルゴリズム(code_challenge_method)」をパラメータに付与。
- 認可サーバ側では code_challenge と code_challenge_method を保持しておく
- トークンエンドポイントリクエスト時に「ハッシュ元の値(code_verifier)」をパラメータに付与。
- 認可サーバ側で、ハッシュ元の値(code_verifier)を code_challenge_method のアルゴリズムでハッシュ化し、ハッシュ化した値(code_challenge)と突合する

![pkce](/img/pkce.png "pkce")
