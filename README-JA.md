# AMWin-RP

> ⚠️ **注意：このリポジトリは非公式の改変版です**  
このリポジトリは、本家 [`AMWin-RP`](https://github.com/PKBeam/AMWin-RP) に関係のない第三者（darui3018823）によって作成・調整されたものです。  
本アプリケーションによるいかなる損害・不具合についても、当方および本家の開発者は一切の責任を負いかねます。  
本家のオリジナル版は、以下のリンクからご確認いただけます：  
👉 [https://github.com/PKBeam/AMWin-RP](https://github.com/PKBeam/AMWin-RP)

Apple MusicのネイティブWindowsアプリ向けのDiscord Rich Presenceクライアントです。Last.FMのscrobblingにも対応しています。

![GitHub release (latest by date including pre-releases)](https://img.shields.io/github/downloads-pre/PKBeam/AMWin-RP/total)  
![GitHub release (latest by date including pre-releases)](https://img.shields.io/github/downloads-pre/PKBeam/AMWin-RP/latest/total)  

![image](https://user-images.githubusercontent.com/18737124/236110561-e11eabf5-d2c4-4fb3-a743-3152a1aef916.png)

![image](https://user-images.githubusercontent.com/18737124/213862194-e02ec9e7-07ab-481f-9dc5-451b9159c903.png)

---

## インストール

リリースは[こちら](https://github.com/PKBeam/AMWin-RP/releases)で確認できます。

すでに[.NET 7.0 ランタイム](https://dotnet.microsoft.com/ja-jp/download/dotnet/7.0)をインストールしている、またはインストール可能な場合は、NoRuntimeリリースをダウンロードしてください。

そうでない場合は、NoRuntimeとラベル付けされていない別のリリースをダウンロードしてください。こちらには.NET 7.0の実行に必要なコンポーネントが含まれているため、ファイルサイズは大きくなっています。

---

## 使用方法

本アプリは、Apple Musicの[Microsoft Storeバージョン](https://apps.microsoft.com/store/detail/apple-music-preview/9PFHDD62MXS1)のみをサポートしています。  
iTunes、Apple Music via WSA、またはサードパーティのプレイヤーには対応していません。

アプリはバックグラウンドで実行され、システムトレイに最小化されます。終了するには、トレイアイコンを右クリックして「Exit」を選択してください。

リッチプレゼンスを表示するには、Apple Musicアプリが開いており、音楽が再生中である必要があります（一時停止中は表示されません）。

設定から、アプリをWindows起動時に自動的に実行するようにすることも可能です。これはトレイアイコンをダブルクリックすることで変更できます。

---

## Last.FMでのScrobbling

Last.FMへのScrobblingに対応しています。  
ご利用には、Last.FMのAPI KeyとSecret Keyが必要です。以下の手順で取得できます：

1. [https://www.last.fm/api](https://www.last.fm/api) にアクセスし、「Get an API account」をクリックします。
2. 表示される情報と、Last.FMのユーザー名・パスワードをアプリに入力してください。

![AMWin-RP-Scrobbler_Settings](https://user-images.githubusercontent.com/317772/215867741-2999591c-35eb-442a-a349-b8e9046634fb.png)

なお、Last.FMのパスワードは、ローカルのWindowsアカウントにある[Windows資格情報マネージャ](https://support.microsoft.com/ja-jp/windows/%E8%B3%87%E6%A0%BC%E6%83%85%E5%A0%B1%E3%83%9E%E3%83%8D%E3%83%BC%E3%82%B8%E3%83%A3%E3%83%BC%E3%81%AB%E3%82%A2%E3%82%AF%E3%82%BB%E3%82%B9%E3%81%99%E3%82%8B-1b5c916a-6a16-889f-8581-fc16e8165ac0)に保存されます。

この実装ではオフラインでのScrobblingには対応していないため、インターネットに接続していない状態で聴いた曲は記録されません。

---

## どのように動作しているのでしょうか？

技術的な詳細です。

本アプリの最大の課題は、Apple Musicアプリから楽曲情報を取得することです。

これを可能にしているのが、.NETの[UI オートメーション](https://learn.microsoft.com/ja-jp/dotnet/framework/ui-automation/ui-automation-overview)です。これにより、ユーザーのデスクトップ上にある任意のウィンドウのUI要素にアクセスすることができます。

一般的な流れは以下の通りです：

- Apple Musicのウィンドウをデスクトップ上から検出
- 曲名などの情報が含まれる既知のUIコントロールへ移動
- 必要な情報を抽出し、Discord RPCの処理部へ送信

もう一つの課題はカバーアートの取得です。

UI オートメーションでは、ウィンドウに表示されている画像を取得できません。そこで、Apple MusicのウェブサイトにHTTPリクエストを送り、楽曲を検索してカバー画像のURLを取得しています。

これは理想的な方法ではありませんが、ほとんどの場合は目的の画像を得ることができます。

（なお、Discord RPCでは、assets keyの代わりに画像のURLを直接指定できるようになっています）
