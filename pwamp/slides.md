
### PWAMPまとめ<br>（勝手な解釈）

##### 2018/01/24

---

### PWAとは？
Progressive Web Apps
「Webサイトを最適化すればネイティブアプリに匹敵するよ」という考え方、取り組みのこと

### AMPとは？
Accelerated Mobile Pages
MobileのWebページを高速化するための手法

### PWAMPとは？
PWAとAMPの組み合わせ

---

### PWA

<p id='custom_footer'>まずはPWAから見ていきましょう.</p>

>>>

<!-- .slide: data-background="rgb(243, 235, 224)" -->
#### PWA(Progressive Web Apps)
Progressive Enhancementを前提として下記を取り入れる
* そもそものパフォーマンス向上
* 最新のWeb APIの導入


<p id='custom_footer'>Progressive Enhancementを前提としています. <br>なので、Progressive Enhancementとは何かをまずは学びましょう.</p>

>>>

<!-- .slide: data-background="rgb(243, 235, 224)" -->
#### [Progressive Enhancement](https://website-usability.info/2010/06/entry_100606.html)(1)

* より幅広く、多くの人が情報にアクセスできるようにすることを目指したWebサイトの開発理念（パソコンだけでなく、携帯電話やスマートフォン、支援技術など　や　障害、加齢、怪我、など）<br>
* あらゆるユーザーに対して基本的な体験ができるようにして、リッチなJSやCSS等は、サポートしているブラウザではその価値を体感できるが、サポートされていないブラウザでは邪魔をしないようにする

>>>

<!-- .slide: data-background="rgb(243, 235, 224)" -->
#### [Progressive Enhancement](https://website-usability.info/2010/06/entry_100606.html)(2)

* HTMLが意味的に適切であること（セマンティックであること）、つまり、視覚的なレイアウトを外した状態でも意味が伝わることが大切.<br>
* JavaScriptは控えめなJavaScript（Unobtrusive JavaScript）を推奨（JavaScriptをサポートしていないブラウザでは「何事もなかったかのように」存在感を消す）.

>>>

<!-- .slide: data-background="rgb(243, 235, 224)" -->
#### そもそものパフォーマンス向上(1)
* [RAIL](https://developers.google.com/web/fundamentals/performance/rail)
* ユーザーを中心に考えるパフォーマンスモデル（response/animation/idle/load）
* [評価指標](https://havelog.ayumusato.com/develop/performance/e744-performance_metrics.html)
  * FP/Speed Index/FCP/FMP/TTI
* 分析
  * Lighthouse/Navigation Timing API/[CALIBRE](https://calibreapp.com/)/[SPEED CURVE](https://speedcurve.com/)/[BUNDLESIZE](https://github.com/siddharthkp/bundlesize)

>>>

<!-- .slide: data-background="rgb(243, 235, 224)" -->
#### [RAIL](https://developers.google.com/web/fundamentals/performance/rail)
* ユーザーを中心に考えるパフォーマンスモデル（response/animation/idle/load）
* ユーザー満足感向上が第一（処理速度向上が目的ではない）
* [遅延＆ユーザーの反応を整理（0〜16ms,0〜100ms,...）](https://developers.google.com/web/fundamentals/performance/rail)
* Chrome DevToolsのTimelineツールでチェック

>>>

<!-- .slide: data-background="rgb(243, 235, 224)" -->
#### [RAIL](https://developers.google.com/web/fundamentals/performance/rail)(response, animation)
* response：ユーザーに対して即座に応答（ユーザー入力は100ms以内に認識）
* [animation：アニメーションやスクロールでは10ms以内にフレームを生成](https://developers.google.com/web/fundamentals/performance/rendering/)
  * 1sに60フレーム、ブラウザの描画も考えると使用できるのは10ms程度（そうしないとブラウザのリフレッシュレートに間に合わないので、画面上でコンテンツが震えて見えるジャンクが発生.）
  * JavaScript/Style/Layout/Paint/Composite
  * ラスタライズ

>>>

<!-- .slide: data-background="rgb(243, 235, 224)" -->
#### [RAIL](https://developers.google.com/web/fundamentals/performance/rail)(idle)
* idle：メインスレッドのアイドル時間を活用
  * 遅延している作業を50ms程度のブロックにグループ化してアイドル中に実行

>>>

<!-- .slide: data-background="rgb(243, 235, 224)" -->
#### [RAIL](https://developers.google.com/web/fundamentals/performance/rail)(load)(1)
* load：ユーザー作業を妨げない（interactive contentsは1000ms以内に提供）
  * サイトは1秒以内に読み込む
  * [クリティカルレンダリングパス(CRP)の最適化](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/)
    * HTML、CSS、JavaScript のバイトの受信から、これらをピクセルとしてレンダリングするために必要な処理までの中間段階で行われている内容を理解して最適化する.
    * DOMツリー、CSSOMツリー、レンダリングツリー、layout(リフロー)、painting(ラスタライジング)
    * 非レンダリングブロック(media type, media query)

>>>

<!-- .slide: data-background="rgb(243, 235, 224)" -->
#### [RAIL](https://developers.google.com/web/fundamentals/performance/rail)(load)(2)
* load：ユーザー作業を妨げない（interactive contentsは1000ms以内に提供）
  * [クリティカルレンダリングパス(CRP)の最適化](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/)
    * JavaScript を非同期(async指定)にして、クリティカル レンダリング パスから不要な JavaScript をすべて取り除く
    * FP(First Paint, 「ナビゲーションの開始（白い画面の状態）から、Web ページの何らかが表示され始めたタイミング」の指標)の改善に有効
  * 重要度の低い読み込みはidle状態まで延期

>>>

<!-- .slide: data-background="rgb(243, 235, 224)" -->
#### [評価指標](https://havelog.ayumusato.com/develop/performance/e744-performance_metrics.html)(1)
* FP(First Paint, 「ナビゲーションの開始（白い画面の状態）から、Web ページの何らかが表示され始めたタイミング」)
* Speed Index(Web ページが表示される過程の進捗 ( Visual Progress ) とその速度を用いた指標、「Web ページのコンテンツがどれくらい速く表示されたかを表すスコア」)
  * Resource Timing API

>>>

<!-- .slide: data-background="rgb(243, 235, 224)" -->
#### [評価指標](https://havelog.ayumusato.com/develop/performance/e744-performance_metrics.html)(2)
* FCP(First Contentful Paint、「ナビゲーションの開始から、Web ページのコンテンツが表示され始めたタイミング」、FPは表示されるのは何でもOKだがFCPはコンテンツとして意味のあるものが表示されたタイミング)
  * Paint Timing API
* FMP(First Meaningful Paint、「Web ページがユーザーにとって、意味がある（役に立つ）表示になったタイミング」)

>>>

<!-- .slide: data-background="rgb(243, 235, 224)" -->
#### [評価指標](https://havelog.ayumusato.com/develop/performance/e744-performance_metrics.html)(3)
* TTI(Time to Interactive、「Web ページのロードが終わって、ユーザーの入力に対して確実に応答できるタイミング」=RAILモデルにおける Idle の要件を満たす状態)
  * Resource Timing API + Long Tasks API
  * 5s以内がトレンド

>>>

<!-- .slide: data-background="rgb(243, 235, 224)" -->
#### そもそものパフォーマンス向上(2)
* テクニック
  * [Resource Hints API](https://qiita.com/ryo_hisano/items/e525616e5026b015aafe)
    * ブラウザに「次に必要となるリソースを教える」ことで、投機的な取得を行う API
  * [App Shellモデル](https://developers.google.com/web/fundamentals/architecture/app-shell?hl=ja)
  * SSR(Server Side Rendering)
  * [PRPL](https://developers.google.com/web/fundamentals/performance/prpl-pattern/?hl=ja)

>>>

<!-- .slide: data-background="rgb(243, 235, 224)" -->
#### 最新のWeb APIの導入(1)
* [A2HS](https://developers.google.com/web/fundamentals/app-install-banners/)
  * アプリのインストールバナー. Manifestファイルを作成するだけで良い.
* [ServiceWorker](https://developers.google.com/web/fundamentals/primers/service-workers/)
  * オフライン体験+バックグラウンド同期+プッシュ通知などのnativeアプリ機能を実現する技術. cacheの利用と各イベントのinterceptが肝.
* [Push Notification](https://developers.google.com/web/fundamentals/codelabs/push-notifications/)
* [Indexed DB (/localStorage/sessionStorage)](https://qiita.com/kosukehorai/items/4553cc356a316c7a9235)

>>>

<!-- .slide: data-background="rgb(243, 235, 224)" -->
#### 最新のWeb APIの導入(2)
* [Credential Management API](https://developers.google.com/web/fundamentals/security/credential-management/?hl=ja)
* [Payment Request API](https://developers.google.com/web/updates/2016/07/payment-request?hl=ja)
* [Geolocation API](https://developers.google.com/web/fundamentals/native-hardware/user-location/?hl=ja)
* [History API](https://liginc.co.jp/web/js/other-js/162559)
<br>etc

>>>

<!-- .slide: data-background="rgb(243, 235, 224)" -->
#### PWAの効果
* そもそものパフォーマンス向上
  * 読み込み、表示が速い.
* 最新のWeb APIの導入
  * native likeな体験.
    * install不要、審査不要、web/nativeで個別開発不要
    * オフライン体験、push notification、バックグラウンド同期...

---

### AMP

>>>

<!-- .slide: data-background="rgb(243, 235, 224)" -->
#### [AMP(Accelerated Mobile Pages)](https://knowledge.sakura.ad.jp/7443/)
GoogleとTwitterが共同してOSSプロジェクトとして立ち上げた手法. [AMP HTMLというフレームワーク](https://www.ampproject.org/ja/)を指す場合もある.

>>>

<!-- .slide: data-background="rgb(243, 235, 224)" -->
#### AMPの構成要素と制約(1)
* AMP HTML：表示高速化のための制約つきHTML
  * 特定のタグ、要素が必須、いくつかのタグ、属性、CSSプロパティは使用不可.
  * img/vide/audio/iframeは専用タグ.
  * CSSは分離不可.
  * SVGに制限あり、カスタムフォントは４つのみ...

>>>

<!-- .slide: data-background="rgb(243, 235, 224)" -->
#### AMPの構成要素と制約(2)
* AMP JS：高速にコンテンツをロードするためのJS
  * AMPに対応したページでは、画像や動画、インラインフレーム（iframe）といったコンテンツロードはすべてAMP JSが管理する仕組み.
  * AMP JS以外のJSは基本的にNG. 動的にサイズが変わるのもNG. iframe内であれば自由に（ただしAMPの恩恵なく）JSを利用することが可能.

>>>

<!-- .slide: data-background="rgb(243, 235, 224)" -->
#### AMPの構成要素と制約(3)
* AMP Cache：コンテンツのキャッシュ配信を行う
* その他
  * 広告配信にも多数対応. 
  * JSON-LD（siteの構造化データを検索エンジンに通知, schema.orgで形式指定）が必須.

>>>

<!-- .slide: data-background="rgb(243, 235, 224)" -->
#### AMPの効果
* AMP HTML：表示高速化のための制約つきHTML
  * ページ表示が速い.
  * データ通信が少ない.
  * SEOへの効果.

---

### PWAMP
= PWA + AMP

>>>

<!-- .slide: data-background="rgb(243, 235, 224)" -->
#### PWAMP
* AMP as PWA：全体的にAMPなので高速だが制約あり（AMPなので）
* AMP to PWA：AMPなのは最初だけ
* AMP in PWA：一部AMPなのでやや制約あり. AMP効果は限定的.
<br>etc