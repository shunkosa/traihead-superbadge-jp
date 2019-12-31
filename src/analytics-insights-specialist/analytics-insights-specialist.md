# Einstein Analytics and Discovery Insights Specialist
* TrailheadのSuperbadge、[Einstein Analytics and Discovery Insights Specialist](hhttps://trailhead.salesforce.com/content/learn/superbadges/superbadge_analytics_insights_specialist)の日本語訳(**非公式**)です。
* 各カスタマイズ要素のラベル部分には補足として日本語を括弧内に記載している場合がありますが、正解チェックは英語のラベルを元に行われるため、実際のチャレンジには日本語表記を含めず、英語表記のみを使用して行って下さい。また、チャレンジ前にユーザと組織の言語・ロケールを英語に切り替えておくことを推奨します。

---
## このスーパーバッジを取得するためにすること
1. 解約率を計算する
2. 加入期間別の解約率を表示する
3. 顧客の獲得コストを計算する
4. 契約期間と顧客満足度スコアを比較する
5. サービスの解約を減らすためのソリューションを提供する

## このスーパーバッジでテストする概念
1. ビジネス要件をダッシュボードのレンズに落とし込む
2. ダッシュボードのJSONファイルを設定する
3. SAQLを使用してダッシュボードやレンズのデータセットにアクセス、分析、フォーマットする
4. 選択バインドと結果バインドを設定する
5. 関連する行のセットにわたってデータセットの計算を実行する
6. Einstein Discoveryのストーリーを作成する
7. Einstein Discoveryの予測結果を確認し改善する
8. SalesforceのオブジェクトにEinstein Discoveryのおすすめを追加する

所要時間 : 推定8時間 - 12時間

## 事前準備とメモ
* ペンや鉛筆を用意して、要件を読み進める際にメモを取ってください。
* 解約率を測定およびレポートするための多くの計算および指標があります。以降のシナリオとChallengeにおいて、解約率は、現在の四半期の解約数を現在の四半期のサービス加入者数で割ったものとして計算されます。
* Challengeを検証する際に使用されるため、ステップや項目、射影の名前については、シナリオで指定されている命名規則に慎重に従ってください。
  * Challengeで使用されるダッシュボードにステップを作成したり、ウィジェットを追加したりする際は、検証に必要となるため、ステップの名前をChallengeで指定された名前で更新してください。
  * 変数と射影の名前にはキャメルケースのスペルを使用します。つまり、``lastName``のように設定します。<sup>[1](#footnote1)</sup>
  * 項目名にはタイトルケースを使用します。つまり、``Last Name``のように設定します。
  * スペースを含むステップや項目、データセットのAPI名はアンダースコア(_)を使用します。つまり、``Last_Name``のように設定します。
* このChallengeに対して[Einstein Analytics Developer Edition (DE)](https://developer.salesforce.com/promotions/orgs/analytics-de)組織を作成します。この環境はEinstein Analyticsが有効になっており、Challengeで使用されるサンプルデータが含まれています。 (注 : このChallengeではDTC Default Appは使用しません。)
* [未管理パッケージ](https://login.salesforce.com/packaging/installPackage.apexp?p0=04tf4000003qbmx)をインストールして、サービス加入者(Subscriber)オブジェクトを実装します。この未管理パッケージはカスタムオブジェクトのタブを作成しないため、[手順](https://help.salesforce.com/articleView?id=creating_custom_object_tabs.htm&type=5&language=ja)に従って、組織にタブを作成しておくことを推奨します。もし管理・未管理パッケージ、またはAppExchangeアプリケーションをインストールする際に問題が発生した場合は、この[記事](https://force.desk.com/customer/en/portal/articles/2710899-installing-a-package-or-app-to-complete-a-trailhead-challenge?b_id=13478)の手順に従ってください。
* Challengeを完了するためには、Beattie Subs.csv、Beattie OEM Survey.csv、Beattie Dashboard.json のファイルをアップロードする必要があります。
* [data-insights-specialist.zip](https://developer.salesforce.com/files/data-insight-specialist-data.zip)をダウンロードして解凍してください。
* CSVファイルには、米国フォーマットの日付項目が含まれています。組織のロケールが米国以外の場合は、次の[手順](https://help.salesforce.com/articleView?id=000004999&type=1&language=ja)に従って変更するか、組織のロケールに基づいてSubscription Date (サービスの加入日) およびChurn Date (サービスの解約日) 項目の形式を変更することを検討してください。

#### リファレンス
* SAQLの構文については、[Analytics SAQL リファレンス](https://developer.salesforce.com/docs/atlas.ja-jp.218.0.bi_dev_guide_saql.meta/bi_dev_guide_saql/bi_saql_intro.htm)を参照してください。
* Einstein Analytics Learning Adventureアプリケーションには、Challengeに役立つかもしれない例が含まれています。新しく作成したAnalytics Developer Edition組織にはこのアプリケーションが備わっています。Analytics Studioで、**作成 | アプリケーション | テンプレートからアプリケーションを作成 | Learning Adventure** をクリックします。
* [Let's Play Salesforce](https://www.youtube.com/channel/UCkNDwCEl-BbAsaGSQ7I6Xtg/playlists)のYoutubeチャンネルにも役立つ動画があります。

#### Subscriber オブジェクトにレコードをインポートする
1. Salesforceデータインポートウィザードを使用します。設定で、クイック検索ボックスに``データインポートウィザード``と入力し、**データインポートウィザード** を選択します。
2. **ウィザードを起動する** をクリックします。
3. **カスタムオブジェクト** をクリックし、**Subscribers** を選択します。
4. **新規レコードを追加** を選択します。ドロップダウンメニューの選択肢では、[--なし--] を選択します。
5. **CSV** をクリックします。**ファイルを選択** を選択して、解凍したBeattie Subs.csv を選択します。**次へ** をクリックしてください。<sup>[2](#footnote2)</sup>
6. **次へ** をクリックしてから **インポートの開始** をクリックします。
7. **OK** をクリックします。

#### Beattie Subs.csv と Beattie OEM Survey.csv ファイルをそれぞれアップロードする
1. Analytics Studioで、Challengeのために、Beattieという名前で空白のアプリケーションを作成します。(注 : 作成ボタンが表示されていない場合は、「Einstein Analytics Plusの管理者権限を自分のユーザ ID に割り当てる」セクションに進んでください。)
2. **作成** ボタンをクリックし、**データセット** を選択します。
3. 次のページで**CSV ファイル** をクリックします.
4. **ファイルを選択するか、ここにファイルをドラッグしてください** をクリックし、解凍したCSVファイルを選択します。
5. **次へ** をクリックします。
5. アプリケーションがBeattieになっていることを確認します。
6. **次へ** をクリックします。
7. 項目属性の編集 はそのままにします。
8. **ファイルをアップロード** ボタンをクリックします。
9. **閉じる** をクリックします。

#### Beattie Dashboard.json ファイルをアップロードする
1. JSONファイルをテキストエディタで開き、コピーします。
2. Analytics Studioのホームページで、**作成 | ダッシュボード | 空白のダッシュボードを作成** の順にクリックします。
3. JSONエディタを開きます。Ctrl+E (MacはCmd+E)キーを押下します。
4. 既存のダッシュボードのJSONを、全選択し削除します。
5. JSONエディタの1行目をクリックし、コピーしたJSONを貼り付けます。
6. **完了** ボタンを押下します。
7. ダッシュボードをBeattieアプリケーションに保存します。
8. **X** をクリックしてタブを閉じ、Analytics Studioのホームページに戻ります。

#### Einstein Analytics Plusの管理者権限を自分のユーザIDに割り当てる
1. 設定で、クイック検索ボックスに``権限セット``と入力し、**権限セット** を選択します。  
2. **Einstein Analytics Plus 管理者(Admin)** をクリックします。  
3. **割り当ての管理** ボタンをクリックしてから、**割り当ての追加** ボタンをクリックします。  
4. ユーザのリストの中から自分の名前を見つけて、その横にチェックマークを付けます。  
5. **割り当て** ボタンをクリックしてから **完了** をクリックします。

## シナリオ
ArnasとOlivia Beattieは、40年前に始めた小規模で低出力のラジオ局が、全国の電気通信会社であるBeattie Media and Broadcasting (Beattie Media)に進化するとは夢にも思っていませんでした。何年もの間、彼らの離れた町では、娯楽と情報を得るのに遠くの駅に頼っていました。電波は弱く、コンテンツはその地域固有のものではありませんでした。ArnasとOliviaはこれらの問題を地元のラジオ局を立ち上げることで対処し、その後すぐにテレビ局を始めました。

ArnasとOliviaは、テレビ局やラジオ局の窮地を乗り越えて事業を拡大し、Beattie Mediaは主要な市場に進出することができました。彼らはまた、全国でシンジケートされる新しいトークラジオフォーマットを導入しました。彼らは自分たちの収入をさらなる拡大のための資金に充て、現在では電話、家庭用およびビジネス用のインターネット、ストリーミングサービスなどサービスを多様化しています。

Beattie Mediaの成長は目覚しいものでしたが、ArnasとOliviaは、自分たちがまだ大きな池の小さな魚であることを認識しています。何年もの間、彼らは独立性を維持し、売却の申し出に抵抗してきました。彼らは買収されていないので、彼らは常により大きなメディア・コングロマリットによって追い出されうることを知っています。大企業と競争するために、彼らはBeattie Mediaを上場企業に変える過程で助言を得るための投資銀行を雇いました。ArnasとOliviaは、株式を公開することで、資本基盤を強化し、名声を高めることを期待しています。しかし、新規株式公開(IPO)の資格を得るには、Beattie Mediaは銀行が定めたいくつかの要件を満たす必要があります。

幸いなことに、Beattie Mediaは以下のように、既にそれらの多くを満たしています。
* 堅実な収益を伴う強い成長の歴史
* 経験豊富な経営陣
* 低い負債比率

しかし、彼らに足りてないのは予測可能な収入です。企業が収益を確実に予測するのに問題がある場合、市場はそれを好みません。これは長期的な成功に不可欠な要素です。

ArnasとOliviaは電話、インターネット、ストリーミングサービスをビジネスに追加したので、顧客の減少や解約に問題を抱えていました。インターネットが優れたイコライザーであるため、消費者は今までにない選択肢を得ています。Beattie Mediaは休むことなく働いて、顧客ロイヤリティを獲得または再獲得するためのプロモーションを行い、お買い得な情報を提供していますが、それらのアプローチは体系的または戦略的ではありませんでした。

銀行はBeattie Mediaが大きな可能性を秘めていると考えており、そしてより安定した収益を達成するための計画を考え出すためにArnasとOliviaに助言しています。

ArnasとOliviaは、Salesforceの長年の顧客であり、Einstein Analyticsから得られる可視性と洞察について耳にしています。彼らのオンサイトのSalesforce管理者は、組織にEinstein Analyticsをセットアップし、Subscriberカスタムオブジェクトをデータセットとしてインポートしました。Beattie MediaのBIチームの一員として、Einstein Analyticsのトレーニングに送りこまれたあなたは、今戻ってきて、顧客の解約を管理しやすくするエグゼクティブダッシュボードを構築することをArnasとOliviaから依頼されています。Analyticsのスキルを活用することで、ArnasとOliviaが新しい洞察を発見し、顧客を積極的に維持していくためのソリューションを作成しましょう。

Beattie Mediaはあなたを頼りにしています。さあ始めましょう！

## 標準オブジェクト
標準オブジェクトは今回のハンズオンChallengeには使用されません。

## カスタムオブジェクト
* **Subscriber** (顧客/サービス加入者) - Beattie Mediaの電話、インターネット、ストリーミング放送の加入者

|項目|定義|
|-|-|
|Account Manager|担当者のID|
|Rep Name|担当者の氏名|
|Account Number|顧客管理番号|
|Name|顧客の氏名|
|Address|顧客の住所|
|Subscription Date|サービスの加入日|
|Senior Citizen|お年寄りかどうか (1, 0)|
|Partner|OEMパートナに関連付けられているかどうか (Yes, No)|
|Dependents|扶養家族がいるかどうか (Yes, No)|
|Tenure|サービス加入期間 (月)|
|Phone Service|電話サービスへの加入状況 (Yes, No)|
|Multiple Lines|複数の電話回線を契約しているかどうか (Yes, No, No phone service)|
|Internet Service|インターネットサービスの加入状況 (DSL, Fiber optic, No)|
|Modem Age|モデムの使用年数 (New, Less than 1 year, 1 - 2 years, > 4 years)|
|Online Security|オンラインセキュリティのオプションに加入しているかどうか<br>(Yes, No, No Internet service)|
|Online Backup|オンラインバックアップのオプションに加入しているかどうか<br>(Yes, No, No Internet service)|
|Device Protection|端末保護サービスのオプションに加入しているかどうか<br>(Yes, No, No Internet service)|
|Tech Support|テクニカルサポート契約があるかどうか (Yes, No, No Internet service)|
|Streaming TV|TVのストリーミングに加入しているかどうか (Yes, No, No Internet service)|
|Streaming Movies|映画のストリーミングに加入しているかどうか (Yes, No, No Internet service)|
|Contract|契約期間 (Month-to-month, One year, Two year)|
|Paperless Billing|オンライン請求かどうか (Yes, No)|
|Payment Method|支払方法 (Electronic check, Mailed check, <br>Bank transfer (automatic), Credit card (automatic))|
|Monthly Charges|月のサービス使用料|
|Total Charges|サービス使用料の累計|
|Churn|サービス解約済みかどうか (Yes or No)|
|Churn Date|サービスの解約日|
|Latitude|顧客の所在地 (経度)|
|Longitude|顧客の所在地 (緯度)
|Postal Code|顧客の自宅住所 (郵便番号)|
|Region|顧客の自宅住所 (米国の州)|

## 外部ファイル
* **Beattie Subs** - Beattie Mediaの電話、インターネット、ストリーミングの顧客のCSVファイル (Subscriberオブジェクトのメタデータを参照してください)
* **Beattie OEM Survey** - Beattie Mediaの、OEMパートナシップを通して獲得した顧客基盤のCSVファイル

|項目|定義|
|-|-|
|Account Manager|担当者のID|
|Rep Name|担当者の氏名|
|Account Number|顧客番号|
|Subscription Date|サービスの加入日|
|Region|顧客の自宅住所 (米国の州)|
|OEM|OEMパートナの名前|
|OEM Type|OEMの業種 (Cable Provider, Media Player, <br>PC Manufacturer, Phone Manufacturer, Television Manufacturer)|
|CSAT|顧客満足度スコア (1–10)|

## ビジネス要件の概要
* 解約率を計算する
* 加入期間別の解約率を表示する
* 顧客の獲得コストを計算する
* 加入期間と顧客満足度スコアを比較する
* サービスの解約を減らすためのソリューションを提供する

## ビジネス要件の詳細
### 解約率を計算する
正確に解約率(Churn Rate) を計算することは、Beattie Mediaの株式公開計画には不可欠です。自分のお気に入りの番組(例えば*Keeping Up with the Real Housewives In Paradise* や*The Walking Handmaids*<sup>[3](#footnote3)</sup>) やお気に入りのスポーツイベントがあったときだけに申し込む顧客もいるため、ストリーミングサービスの解約数は高い状態です。

収入と利益を守るために、ArnasとOliviaは、四半期ごとのBeattie Mediaの解約率について理解を深めるためのグラフを求めています。

1. Beattie Media ダッシュボードを開き、**編集** モードに入ります。
2. ``CHALLENGE 1`` というラベルのダッシュボードセクションを見つけて、グラフウィジェットで置き換えます。
3. **Beattie Subs** データセットを使用します。
4. **Churn Rate** という名前のステップを追加します。
5. 四半期中にサービスを解約した顧客の割合を示す Churn Rate 項目を作成します。Beattie Mediaでの定義は以下の通りです。

![](churn_rate.png)

* 解約率を計算するには、四半期ごとの顧客の活動(加入と解約)ごとにデータを整理する必要があります。たとえば、``ActivityDate_Year``や``ActivityDate_Quarter``などの射影を使用して、サービスの加入日と解約日でデータをグループ化することができます。
* Beattie Subs ファイルには、1種類の活動のみを含む四半期があります。たとえば、サービスの加入がなく、解約だけがある場合です。データストリームをgroupやcogroup、またはunionするときには、この点に留意してください。
* このChallengeであなたが作成しようとしている計算は大変ですが、ArnasとOliviaはあなたを信頼しています！上記の解約率の表を使用して、このChallengeに対する計算式を考案してください。

1. 作成したソリューションは折れ線グラフで表示してください。
2. 解約率を表示している最初の四半期にグラフマーカーを追加してください。作成したソリューションを正しく検証するために、グラフマーカーはハードコーディングしてください。
3. ダッシュボードを、``Beattie Media Executive Dashboard`` という名前で**Beattie** アプリケーションに保存してください。
4. 作成したソリューションは次の例のようになるはずです。

![](challenge_1.png)

### 加入期間別の解約率を表示する
新しい顧客を幸せにするためのBeattie Mediaの精力的な働きもまた、初めの頃からサービスに加入していたロイヤリティの高い顧客に対して犠牲を払っていました。ArnasとOliviaは、長年サービスに加入している顧客が無視されているように感じロイヤリティを失い始めていると聞いて落胆しています。これらのかつて安定していた顧客はまた、より良いお買い得情報を求めてサービスを解約しています。ArnasとOliviaは、加入期間内の解約をモニタリングしたいと考えています。たとえば、2年以上契約が続いていてキャンセルされた顧客の数を測定します。彼らは、加入期間別の解約率を示すグラフを求めており、すべての顧客に対して、より戦略的になり、より的を絞るようにしたいと考えています。

1. ``CHALLENGE 2`` というラベルのダッシュボードセクションを見つけて、グラフウィジェットに置き換えます。  
2. ``Churn Tenure`` という名前のステップを追加します。  
3. 解約した顧客 (条件は ``Churn = "Yes"``) の数を顧客の総数で割る計算を追加します。解約した顧客の数が分子になり、総顧客数が分母になります。  
4. Churn Tenure グラフの上に切り替えウィジェットを追加します。  
5. 表示ラベルとして、``Tenure Length`` を追加します  
6. 以下のカスタム定義をウィジェットに追加します。

| Tenure Length (表示名) | Length of Tenure (対応する値の範囲) |
|-|-|
|High Risk|1〜12 か月|
|Medium Risk|13〜24 か月|
|Low Risk|25〜36 か月|

Tenure Length 切り替えウィジェットをChurn Tenure グラフにバインドし、Tenure項目をフィルタリングします。作成したソリューションは以下の例のようになるはずです。

![](challenge_2.png)

### 顧客の獲得コストを計算する
Beattie Mediaは無制限のサービスプランを提供していますが、それは有料のサブスクリプションによるものです。サービスを解約する顧客が彼らの顧客基盤を縮小させています。顧客のプールが小さくなればなるほど、代わりの顧客を獲得するための費用が高くなります。この費用は、1世帯あたり950.00ドル(USD) と推定されています。サービスの解約により、Beattie Mediaはあちこちに行ってしまう顧客を新しい顧客で補いづらくなっています。

ArnasとOliviaから、州別の顧客からの収益および、彼らが失った顧客を埋め合わせるために費やした損失コストを表示するグラフを作成するように依頼されています。加入期間を分類することができたので、彼らはまた、加入期間によって損失コストがファセットで絞り込まれる様子も見たいと思っています。

1. ``CHALLENGE 3`` というラベルのダッシュボードセクションを見つけて、グラフウィジェットに置き換えます。
2. ``Subscriber Revenue`` という名前のステップを追加します。
3. この手順では、``TotalCharges`` 項目を合計して ``Region`` 項目でグループ化する棒グラフを作成します。
4. ``Attrition Cost`` という名前でステップをもう1つ作成します。このステップでは顧客の損失コストを次の式で計算します。[ 解約した顧客の数 (条件は Churn = 'Yes') × 950.00ドル(USD) ]
5. 前の手順での計算には、射影の名前に ``attrCost`` を使用してください。
6. Subscriber Revenue グラフに基準線を追加します。
7. 基準線に損失コストを表示する結果バインドを作成します。また、基準線の値をTenure Lengthのトグルウィジェットを用いて、加入期間の長さでフィルタリングできるようにします。
8. 作成したソリューションは以下の例のようになるはずです。

![](challenge_3.png)

### 契約期間と顧客満足度スコアを比較する
Beattie Mediaの顧客基盤の大部分は、PCとテレビの製造業者を含むOEMメーカーとの提携によるものです。これらの企業は、自社の機器でストリーミングコンテンツにアクセスするために、自社の機器にBeattie Mediaのアプリを携帯することに同意しました。

Beattie Mediaはこれらの顧客に対して、ストリーミングサービスに対する満足度をランク付け(1 = 非常に不満 から 10 = 非常に満足 まで)するように依頼しました。ArnasとOliviaは、CSAT(顧客満足度)スコアが低かったり、加入期間が短かったりする地域やOEMメーカーを見ることに興味があります。

1. ``CHALLENGE 4`` というラベルのダッシュボードセクションを、2つのグラフウィジェットに置き換えます。
2. 1つ目のグラフで、``Beattie Survey`` という名前のステップを追加します。
3. Beattie Subs データセットのTenure (加入期間) 項目を使用して平均のTenureを計算し、Beattie OEM Survey データセットのCSAT項目を使用して平均CSATを計算します。
4. 2つ目のグラフで、``OEM`` という名前のステップを追加します。
5. Beattie OEM Survey データセットのデータをOEM項目別にグループ化し、行 計数(Count of Rows) の基準(Measure) を追加してツリーマップグラフとして表示します。
6. ファセットを使用して、ピラミッドグラフでの選択によりツリーマップグラフが絞り込まれることを確認します。
7. 平均CSATスコアが最も低い**最初**の州のBeattie Survey グラフにグラフマーカーを追加します。Challengeを検証する際に作成したソリューションが正しく評価されるように、マーカーはハードコーディングしてください。

 **クエリを作成するときには、以下の情報を考慮してください**
 
* Beattie OEM パートナのファイルのすべての顧客は、Beattie の顧客のファイル内の最低1つのレコードと紐づきます。<sup>[4](#footnote4)</sup>  
* Beattie MediaのアカウントマネージャはOEMパートナーに割り当てられ、その関係を管理しています。  
* Account Manager項目とRegion項目を使用してデータセットをグループ化します。  
* CSATとTenureの結果を整数に丸めます。  
* 結果をCSATスコアで昇順に並べ替えます。  
* 作成したソリューションはピラミッドグラフとして表示します。

作成したソリューションは以下の例のようになるはずです。

![](challenge_4.png)

### サービスの解約を減らすためのソリューションを提供する
Beattie Mediaが雇った投資銀行は、少なくとも2年の契約がより安定した収益のイメージにつながり、契約更新につながるであろうと提言しています。Beattie Mediaは、既存顧客および潜在顧客がそれを好んでいないことを知っているので、長期契約にこだわっていませんでした。これまでのプロモーションでは、せいぜい数か月の契約継続を獲得することはありましたが、長期契約に対する勝ち手は見出せていません。

あなたはArnasとOliviaに、Einstein Discoveryと、その高速で・利用しやすい・予測的な洞察について話しました。あなたはこれが繰り返し行うプロセスになるだろうという予想を説明しました。ArnasとOliviaはそれに同意し、あなたが考えついたものを見ることを楽しみにしています。また、Einstein Discoveryからのおすすめ情報がSubscriberのサービスのレコードにどのように表示されるかのモックアップを作成する予定であることも2人に伝えました。ArnasとOliviaは、このアイデアを気に入っていて、この情報をカスタマーサクセスエージェントを支援するために手元に置いておくことを望んでいます。

あなたの仕事はデータセットを分析して加入期間を増やすためのおすすめを作成することです。

#### Einstein Discoveryでストーリーを作成する
1. Beattie Subs データセットを使用してストーリーを作成し、顧客の加入期間を改善するためのおすすめを生成します。
2. ストーリーを確認し、調整して、解約の原因を示す関連情報を取得します。  
3. おすすめがが完成したら、これらの[手順](https://help.salesforce.com/articleView?id=bi_edd_wb_native.htm&type=5&language=ja)を実行してSubscriberカスタムオブジェクトにストーリーのおすすめを表示します。注 : 手順には、複数のタブを開く管理パッケージのインストールが含まれます。 インストールが完了したら、余計なタブは必ず閉じてください。  
4. 作成した最終的な予測に Predicted Tenure と名前を付けます。

#### Salesforceのオブジェクトにおすすめを追加する
|項目名|データ型|説明|
|-|-|-|
|Tenure Outcome|数値|Einstein Discovery outcome information.|
|Tenure Explanation|ロングテキストエリア|Einstein Discovery explanation information.|
|Tenure Prescription|ロングテキストエリア|Einstein Discovery prescription information.|
  
1. 結果(Outcome)、説明(Explanation)、処方箋(Prescription) のおすすめを表示するために使用するカスタム項目については、上記の項目名を使用してください。 Salesforceオブジェクトにカスタム項目を追加する手順については、この[リンク](https://help.salesforce.com/articleView?id=adding_fields.htm&type=5&language=ja)を使用してください。  
2. Einstein Discoveryをカスタム項目に接続します。カスタム設定の Einstein Discovery - Write Back Detail で、エントリーの名前としてTenureを使用します。  
3. Subscriberレコードが更新されたときに起動するApexトリガを作成します。上記のリンクにあるヘルプ記事には、コードのテンプレートが用意されています。要求されたパラメータを埋め、トリガーの名前は変更しないようにしてください。  
4. Cancelled Subscribers (解約した顧客) のリストビューから1件レコードを編集し、変更を加えずにレコードを保存します。  
5. 作成したおすすめ項目にストーリーの結果が表示されていることを確認します。結果は以下の例のようになります。

![](challenge_5.png)

## Challenge
### Challenge 1: 解約率を計算する
四半期の解約率を表示する折れ線グラフを作成してください。

### Challenge 2: 加入期間別の解約率を表示する
加入期間別の解約率を表示する評価グラフを作成してください。加入期間の長さでグラフをフィルタできるように設定してください。

### Challenge 3: 顧客の獲得コストを計算する
州別に顧客からの収益を表示し、失った顧客を埋め合わせるために費やした損失コストを表示する棒グラフを作成してください。

### Challenge 4: 契約期間と顧客満足度スコアを比較する
OEM契約を通して獲得した顧客に対して2つのグラフを作成してください。1つ目は、地域別に平均の顧客満足度スコアと加入期間を表示するピラミッドグラフです。2つ目は、OEMパートナ毎に顧客の数を表示するツリーマップグラフです。

### Challenge 5: サービスの解約を減らすためのソリューションを提供する
データセットを分析し、ストーリーを作成し、おすすめをSalesforceのオブジェクトのページレイアウトに追加してください。

## 訳注
* <a name="footnote1">[1]</a> : 原文には記載がありませんが、厳密に言うと<a href="https://ja.wikipedia.org/wiki/%E3%82%AD%E3%83%A3%E3%83%A1%E3%83%AB%E3%82%B1%E3%83%BC%E3%82%B9">Lower Camel Case</a>ということです。
* <a name="footnote2">[2]</a> : 原文には記載がありませんが、文字コードはUTF-8、値の区切り文字にはカンマを指定してください。
* <a name="footnote3">[3]</a> : どちらも架空のテレビ番組ですが、<a href="https://en.wikipedia.org/wiki/The_Real_Housewives_of_Beverly_Hills">The Real Housewives of Beverly Hills</a>、<a href="https://en.wikipedia.org/wiki/The_Walking_Dead_(TV_series)">The Walking Dead</a>と<a href="https://en.wikipedia.org/wiki/The_Handmaid%27s_Tale">The Handmaid's Tale</a> あたりが元ネタだと思います。
* <a name="footnote4">[4]</a> : Account Numberで紐づくということです。前述の通り、Beattie Subs データセットには同じAccount Numberのレコードが複数存在します。
