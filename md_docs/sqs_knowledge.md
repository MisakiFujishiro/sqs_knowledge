# SQS
<div style="display: flex; justify-content: space-around;margin-top: -20px;">
  <img src="img/sqs.png" style="width: 50%;">
</div>

---



## SQSの型
----


### メッセージ型
<div style="font-size: 0.8em;">
    <p>
        メッセージ一つ1つが単独で処理することができる
    </p>
</div>

<div style="display: flex; justify-content: space-around;margin-top: -20px;">
  <img src="img/sqs-03.jpg" style="width: 70%;">
</div>

----


### P2P
<div style="font-size: 0.8em;">
    <p>
        1つのメッセージに対して、受信者が1つになる
    </p>
</div>

<div style="display: flex; justify-content: space-around;margin-top: -20px;">
  <img src="img/sqs-04.jpg" style="width: 70%;">
</div>


----

### Pull型
<div style="font-size: 0.8em;">
    <p>
        データは一度キューに格納される<br>
        データの取得はconsumerが能動的に行う
    </p>
</div>

<div style="display: flex; justify-content: space-around;margin-top: -20px;">
  <img src="img/sqs-05.jpg" style="width: 70%;">
</div>


----
### まとめ
<div style="font-size: 0.8em;">
    <p>
        独立した一つ一つのメッセージデータを扱い、<br>
        1つのデータは1つのconsumeが取得するP2P型であり、<br>
        consumerが能動的にデータを取得するpull型のサービス
    </p>
</div>

<div style="display: flex; justify-content: space-around;margin-top: -20px;">
  <img src="img/sqs-06.jpg" style="width: 70%;">
</div>




---
## キューの種類
----
### standardキュー
<div style="font-size: 0.8em;">
    <p>
        ベストエフォートの順序で処理されるキュー<br>
        配信についてもat least onceであり、重複する可能性が残る<br>
        その分、スループットは無制限になる
    </p>
</div>

<div style="display: flex; justify-content: space-around;margin-top: -20px;">
  <img src="img/sqs-07.jpg" style="width: 70%;">
</div>

----
### fifoキュー

<div style="font-size: 0.6em;">
    <p>
        配信順序を受信順序と厳密に一致させるキュー<br>
        順序はキュー全体で管理されるのではなく、メッセージグループID内で管理される<br>
        配信についてもメッセージ重複IDを利用した重複排除の仕組みが組み込まれている
    </p>
</div>

<div style="display: flex; justify-content: space-around;margin-top: -20px;">
  <img src="img/sqs-08.jpg" style="width: 70%;">
</div>











---
## 基本機能(standardキュー)
----
### ポーリング
<div style="font-size: 0.6em;">
    <p>
        consumerがSQSに対してメッセージを受信しにいく処理<br>
        ショートポーリング:メッセージがあれば返却、なければないと即時に返信<br>
        ロングポーリング:メッセージがあれば返却、なければ指定時間待機してから返信
    </p>
</div>

<div style="display: flex; justify-content: space-around;margin-top: -20px;">
  <img src="img/sqs-09.jpg" style="width: 70%;">
</div>

----
### メッセージの削除（受信ハンドル）
<div style="font-size: 0.6em;">
    <p>
        SQSでは、処理を完了時にキューのメッセージをconsumerが削除する<br>
        consumerがメッセージを受信した際に、SQSからから受信ハンドルが配信される<br>
        SQSは、メッセージを受信し処理完了時に受信ハンドルを指定して削除を実行する
    </p>
</div>

<div style="display: flex; justify-content: space-around;margin-top: -20px;">
  <img src="img/sqs-10.jpg" style="width: 70%;">
</div>



----
### 可視性タイムアウト
<div style="font-size: 0.55em;">
    <p>
        consumer-Xが受信したメッセージを別consumerが認識しなくなる時間<br>
        この時間はタイムアウトの時間などを鑑みて決定する必要がある<br>
        可視性タイムアウト経過後、別のconsumerに配信されると受信ハンドルが新規発行され<br>
        consumer-Xが受け取った古い受信ハンドルでは削除できなくなる
    </p>
</div>

<div style="display: flex; justify-content: space-around;margin-top: -20px;">
  <img src="img/sqs-11.jpg" style="width: 70%;">
</div>

----
### 遅延キューとメッセージタイマー
<div style="font-size: 0.8em;">
    <p>
        メッセージがキューに入ってから処理されるまでの遅延時間<br>
        遅延キューを利用すればキュー単位で、<br>
        メッセージタイマーを利用すればメッセージ単位で適用される
    </p>
</div>

<div style="display: flex; justify-content: space-around;margin-top: -20px;">
  <img src="img/sqs-12.jpg" style="width: 70%;">
</div>

----
### DLT
<div style="font-size: 0.8em;">
    <p>
        メッセージが配信されて、処理が失敗した場合に再配信される<br>
        事前設定した所定の回数再配信された場合、<br>
        DLQ(Dead Letter Queue)として設定した別キューに配信される
    </p>
</div>

<div style="display: flex; justify-content: space-around;margin-top: -20px;">
  <img src="img/sqs-13.jpg" style="width: 70%;">
</div>




---
## FIFOキュー
----
### メッセージグループID
<div style="font-size: 0.7em;">
    <p>
        メッセージ送信時に指定する必須のID<br>
        メッセージはこのグループIDごとに管理され、ID内で順番が担保される<br>
        kafkaのpartitionに対応するが、事前に数を指定する必要性はない
    </p>
</div>

<div style="display: flex; justify-content: space-around;margin-top: -20px;">
  <img src="img/sqs-14.jpg" style="width: 70%;">
</div>

----
### メッセージ重複ID
<div style="font-size: 0.8em;">
    <p>
        メッセージ送信時に指定するID<br>
        メッセージを配信する際に重複IDを確認し<br>
        5分間の間は同じ重複IDのメッセージを配信しなくなる
    </p>
</div>

<div style="display: flex; justify-content: space-around;margin-top: -20px;">
  <img src="img/sqs-15.jpg" style="width: 70%;">
</div>
