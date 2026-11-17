# iPhoneで「ターゲットスコア」を使う

## まず知っておいてください

iPhoneのカメラをブラウザから使うには、アプリを **HTTPSのWebサイト** として公開する必要があります。この説明では、無料で使えるGitHub Pagesに公開します。

公開後は、iPhoneのSafariでWebサイトを開き、共有メニューからホーム画面に追加できます。

## 用意するもの

- GitHubアカウント（なければ [github.com](https://github.com/) で作成）
- Windows PC
- Git for Windows
- この `BBguns` フォルダ

> **公開範囲に注意:** 手順ではリポジトリを「Public（公開）」にします。リポジトリとアプリのURLは、知っている人が閲覧できます。このフォルダに個人情報や秘密情報を置かないでください。

## 手順1：Gitをインストールする

1. [Git for Windows公式ダウンロードページ](https://git-scm.com/download/win)を開き、インストーラーをダウンロードします。
2. ダウンロードしたインストーラーを開き、基本的には初期設定のままインストールします。
3. インストール後、Windowsのスタートメニューから **PowerShell** を開きます。
4. 次のコマンドを入力してEnterを押します。

   ```powershell
   git --version
   ```

   `git version ...` と表示されれば準備完了です。`git` が見つからないというエラーが出る場合は、PowerShellを閉じて開き直してください。

## 手順2：GitHubに空のリポジトリを作る

1. [GitHub](https://github.com/)にサインインします。
2. 画面右上の **＋** を押し、**New repository** を選びます。
3. 次のように設定します。
   - **Repository name:** `BBguns`
   - **Public** を選択
   - **Add a README file** など、ファイルを追加するチェック項目は選択しない（空のリポジトリにします）
4. **Create repository** を押します。
5. 作成したページのURLを確認します。たとえばユーザー名が `taro` なら、URLは次の形です。

   ```text
   https://github.com/taro/BBguns
   ```

   この例の `taro` の部分が、あなたのGitHubユーザー名です。

## 手順3：BBgunsフォルダをGitHubにアップロードする

1. Windowsのエクスプローラーで、デスクトップにある **BBguns** フォルダを開きます。
2. エクスプローラーのアドレスバーをクリックし、`powershell` と入力してEnterを押します。そのフォルダを開いた場所でPowerShellが起動します。
3. 次のコマンドを**1行ずつ**貼り付けて、各行のあとにEnterを押します。

   ```powershell
   git init
   git add .
   git commit -m "Add target scoring app"
   git branch -M main
   ```

4. GitHubのユーザー名を入れて、次のコマンドを実行します。`YOUR-USERNAME` は自分のユーザー名に置き換えてください。山括弧 `< >` は入力しません。

   ```powershell
   git remote add origin https://github.com/YOUR-USERNAME/BBguns.git
   git push -u origin main
   ```

5. GitHubへのサインインを求められたら、表示された案内に従って認証します。パスワードをPowerShellに入力するよう求められた場合、GitHubのWebサイト用パスワードではなく、ブラウザー認証などの表示された方法を使ってください。
6. GitHubの `BBguns` リポジトリのページを更新し、`index.html` や `manifest.webmanifest` などのファイルが表示されればアップロード完了です。

> `git commit` で名前やメールアドレスの設定を求められた場合は、Gitに表示された設定コマンドを実行してから、`git add .` と `git commit ...` をもう一度実行してください。

## 手順4：GitHub Pagesを有効にする

1. GitHubの `BBguns` リポジトリで **Settings** を開きます。
2. 左側の **Pages** を選びます。
3. **Build and deployment** の **Source** で **GitHub Actions** を選びます。
4. 上部の **Actions** タブを開き、`Deploy static site to GitHub Pages` の処理が終わるまで待ちます。最初の公開には数分かかることがあります。
5. 処理に緑色のチェックが付いたら、**Settings → Pages** に戻り、表示された公開URLを開きます。

公開URLは通常、次の形です。`YOUR-USERNAME` は自分のGitHubユーザー名です。

```text
https://YOUR-USERNAME.github.io/BBguns/
```

サイトが表示されない場合は、Actionsの処理が完了しているか、リポジトリ名とURLの綴りが合っているか確認してください。

## 手順5：iPhoneで開いてホーム画面に追加する

1. iPhoneで **Safari** を開き、GitHub Pagesの公開URLにアクセスします。
2. 「カメラを起動」をタップし、Safariのカメラ使用確認で **許可** を選びます。
3. 的を映し、自動検出または画面タップで位置を設定します。斜めから撮影すると的は楕円形に見えるため、アプリは楕円形状と角度を追跡して枠と得点判定を補正します。カメラ映像の下にあるズームスライダーは、端末のカメラズームに対応していればそれを使い、未対応の場合は画面を拡大します（画質は向上しません）。ズーム時は的を画面中央に合わせてください。遠くの小さい的は「検出設定」で「遠距離検出」をONにしてから「的を自動検出」を押してください。認識後はカメラ起動中に0.1秒ごとに位置・大きさ・角度を補正します。揺れや追跡の乱れを検出した間は命中判定を一時停止し、画面が安定してから再開します。誤検出を減らすため、穴候補は暗い芯・小さく丸い形・周囲との局所的な明るさの差を確認し、同じ大きさと位置で4回連続して検出します。「候補を確認してから加点」がON（初期設定）の場合は、履歴の「穴を確認して加点」を押すまで得点に入りません。OFFにすると候補を自動加点します。検出設定の数値を上げるほど候補条件が厳しくなります。得点に記録済みの穴は、的の位置補正後も同じ場所として扱い、再加点しません。トレーニング開始時に合図音が鳴り、終了時に合計点を読み上げます。命中点の読み上げは「検出設定」でON/OFFできます。
4. アイコンから起動したい場合は、Safariの **共有** ボタンをタップして **ホーム画面に追加** を選びます。

カメラを使うときは、HTTPSの公開URLをSafariで開いてください。PC内の `index.html` をiPhoneに送っただけでは、カメラを使えません。

## アプリを更新する

ファイルを変更したあと、BBgunsフォルダでPowerShellを開き、次のコマンドを実行します。

```powershell
git add .
git commit -m "Update target scoring app"
git push
```

GitHub Actionsの処理が完了すると、公開サイトにも変更が反映されます。ホーム画面に追加済みのアプリで古い画面が表示される場合は、いったんSafariで公開URLを開き直してください。

## 補足

- iPhoneではSafariを使ってください。カメラ利用にはHTTPSとカメラ許可が必要です。
- アプリ画面はオフライン用にキャッシュされますが、最初のアクセスやカメラ利用時はインターネット接続が必要です。
- GitHub Pagesの公開URLを知っている人はアプリを閲覧できます。
