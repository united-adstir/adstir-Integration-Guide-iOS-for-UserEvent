# ユーザ行動データ計測機能

## 対象OS

iOS 8以上

## 利用準備

ご利用にはメディアシンボルの設定が必要になります。
担当者にお問い合わせください。

下記を参考にadsitr SDKを導入してください。

https://github.com/united-adstir/AdStir-Integration-Guide-iOS/wiki/%E5%88%9D%E6%9C%9F%E8%A8%AD%E5%AE%9A

## ユーザ行動データ通知方法

以下の手順でadstirのサーバにユーザ行動データを送信いたします。

1. 初期化を行う
1. アプリ内のユーザ行動を計測する

### 1. 初期化を行う

`AdstirUserEvent setMediaSymbol:`を用いて初期化を行います。

```obj-c
[AdstirUserEvent setMediaSymbol:@"MEDIA-xxxx"];
```

### 2. アプリ内のユーザ行動を計測する

`AdstirUserEventBuilder`を用いて`AdstirUserEvent`を作成し、`AdstirUserEventTracker`でadstirのサーバへユーザ行動データを送信します。
adstirが用意しているパラメータは APIリファレンス を参照してください。
独自のパラメータを送信したい場合は、`setParameter()`を用いてkey-value形式で設定してください。valueはint、String、boolean型のみが利用できます。

```obj-c
// イベント作成
AdstirUserEventBuilder *builder = [AdstirUserEventBuilder new];
[builder setEpisode:1]; //現在の話数をセット
[builder setUserName:@"xxxx"]; // アプリ内ユーザ名をセット
[builder setPaymentRate:@"NO"]; // ユーザの課金度合いをセット

// イベント送信
AdstirUserEventTracker *tracker = [AdstirUserEventTracker sharedInstance];
[tracker trackEvent:[builder build:AUEEvent.AUEEventPayment]];
```

#### 課金時のデータを送信する例

\* 初めに初期化を行なってください

```obj-c
// イベント作成
AdstirUserEventBuilder *builder = [AdstirUserEventBuilder new];
[builder setUserName:@"xxxx"]; // アプリ内ユーザ名をセット
[builder setPaymentAmount:1000]; // 1000円分課金
[builder setPaymentRate:@"LOW"]; // ユーザの課金度合いをセット

// イベント送信
AdstirUserEventTracker *tracker = [AdstirUserEventTracker sharedInstance];
[tracker trackEvent:[builder build:AUEEvent.AUEEventPayment]];
```

#### 読書開始に通知する例

```obj-c
// イベント作成
AdstirUserEventBuilder *builder = [AdstirUserEventBuilder new];
[builder setUserName:@"xxxx"]; // アプリ内ユーザ名をセット
[builder setTitle:@"恐怖の〇〇"]; // マンガのタイトル
[builder setCategorys:@[@"ホラー"]]; // マンガのカテゴリ
[builder setEpisode:1]; // 1話目を読書開始

// イベント送信
AdstirUserEventTracker *tracker = [AdstirUserEventTracker sharedInstance];
[tracker trackEvent:[builder build:AUEEvent.AUEEventStartReading]];// 読書開始イベント
```

* テスト時など、adstirのサーバへデータを送信したくない場合はtrackEvent()よりも前に下記を記述します。

```obj-c
[AdstirUserEvent setTestMode:YES];
```

* 端末のIDFAが取得できない場合、SDKはサーバへユーザデータの通知を行いません。

## APIリファレンス

https://united-adstir.github.io/adstir-Integration-Guide-iOS-for-UserEvent/ を参照してください。