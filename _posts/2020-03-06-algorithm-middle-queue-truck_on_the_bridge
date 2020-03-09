---
layout: posts
comments: true
title: 【アルゴリズム_中級】橋を渡るトラック
tags: [algorithm, java, queue, middle level]
category: java

---

* **アルゴリズム分類**

  Stack&Queue

* **問題説明**

  幾つかのトラックが一車線の橋を渡ろうとします。全てのトラックが橋を渡るまでの時間を算出する必要があります。トラックは1秒に「1」ずつ移動し、橋の長さは「bridge_length」、耐えられる最大重量は「weight」です（トラックが橋に完全に上がっていない場合、このトラックの重量は考慮しません）。

  たとえば、長さが「2」で最大重量が「10」の橋があります。車重が[7、4、5、6]のトラックが橋を渡るまでかかる時間は以下の通りです。

  | 経過時間 | 橋を渡ったトラック | 橋を渡っているトラック | 待機トラック |
  | -------- | ------------------ | ---------------------- | ------------ |
  | 0        | []                 | []                     | [7,4,5,6]    |
  | 1~2      | []                 | [7]                    | [4,5,6]      |
  | 3        | [7]                | [4]                    | [5,6]        |
  | 4        | [7]                | [4,5]                  | [6]          |
  | 5        | [7,4]              | [5]                    | [6]          |
  | 6~7      | [7,4,5]            | [6]                    | []           |
  | 8        | [7,4,5,6]          | []                     | []           |

  全てのトラックは橋を渡るまでは8秒が掛かります。

  solution関数の引数として橋の長さ「bridge_length」、橋が耐えられる最大重量「weight」、トラック別車重「trunck_weights」が渡されます。このとき、全てのトラックが橋を渡るまでかかる時間をreturnするsolution関数を作成して下さい。

* **制約条件**

  ・bridge_lengthは「1」以上「10,000」以下です。  

  ・weightは「1」以上「10,000」以下です。  

  ・trunck_weightsのサイズは「1」以上「10,000」以下です。  

  ・全てのトラックの車重は「1」以上「weight」以下です。  

  ・トラックの順番変更はできないです。

* **入出力例**

  | bridge_length | weight | truck_weights                   | return |
  | ------------- | ------ | ------------------------------- | ------ |
  | 2             | 10     | [7,4,5,6]                       | 8      |
  | 100           | 100    | [10]                            | 101    |
  | 100           | 100    | [10,10,10,10,10,10,10,10,10,10] | 110    |

* **自分の解答**

  queueを利用する問題ということはすぐ把握できたが、実装に相当時間が掛かった。  
  最初はTruckオブジェクトを生成せず、マップで実装しようとしたが、マップを無だに参照することが多くマップは諦めた。また、臨時のqueueで残り時間を算出し、元々のqueueに入れる処理が中々実装できず（アイデアがなかった...）時間が掛かった要因になった。  
  queueの扱いは結構慣れたので、実装する方法について考える必要があるかと思った。

  ```java
  import java.util.LinkedList;
  import java.util.Queue;
  
  class Solution4 {
  
      class Truck {
          /** トラック重量 */
          int weight;
          /** 残り時間 */
          int remainTime;
  
          /**
          * コンストラクタ
          *
          * @param weight
          * @param remainTime
          */
          public Truck(int weight, int remainTime) {
              this.weight = weight;
              this.remainTime = remainTime;
          }
  
          /**
          * トラック移動
          */
          public void move() {
              this.remainTime--;
          }
      }
  
      /**
      * @param bridge_length 橋の長さ
      * @param weight 橋の最大重量
      * @param truck_weights トラック重量
      * @return 全てのトラックが橋を渡る時間
      */
      public int solution(int bridge_length, int weight, int[] truck_weights) {
          // 橋を渡っているトラック
          Queue<Truck> onBridges = new LinkedList<>();
          // 準備中のトラック
          Queue<Truck> underBridges = new LinkedList<>();
          
          // 準備中のトラック情報設定
          for (int truck : truck_weights) {
              underBridges.offer(new Truck(truck, bridge_length));
          }
  
          // 経過時間
          int time = 0;
  
          // 橋の上にいるトラック重量の合計
          int onBridgeWeight = 0;
  
          while (true) {
              
              if (!onBridges.isEmpty()) {
                  // 橋上にあるトラックを移動する
                  onBridges = moveTruck(onBridges);
  
                  if (onBridges.peek().remainTime == 0) {
                      // 橋を渡ったらキューから取り出して、橋にあるトラック重量合計から引く
                      onBridgeWeight -= onBridges.poll().weight;
                  }
              }
  
              // 次に待っているトラック
              Truck readyTruck = underBridges.peek();
  
              if (readyTruck != null && onBridgeWeight + readyTruck.weight <= weight) {
                  // 待機トラック出発
                  onBridges.offer(underBridges.poll());
                  // 橋上にあるトラックの重量算出
                  onBridgeWeight += readyTruck.weight;
              }
  
              // 経過時間
              time++;
  
              if (onBridges.isEmpty() && underBridges.isEmpty()) {
                  // 全てのトラックが橋を渡ったら終了
                  break;
              }
          }
  
          return time;
      }
  
      /**
      * 橋上にあるトラックの移動
      *
      * @param onBridges 橋の上にあるトラックのqueue
      * @return トラックの移動後情報
      */
      public Queue<Truck> moveTruck (Queue<Truck> onBridges) {
          // 臨時queue
          Queue<Truck> tempQueue = new LinkedList<>();
          
          // トラックの移動処理
          while (true) {
              // 渡っているトラックを取り出して
              Truck onBridgeTruck = onBridges.poll();
              // 移動する
              onBridgeTruck.move();
              // 移動が終わったトラックを臨時queueに格納
              tempQueue.offer(onBridgeTruck);
              
              if (onBridges.size() == 0) {
                  // 移動するトラックがなければ移動終了
                  break;
              }
          }
  
          // 移動後の情報を元々のqueueに格納
          onBridges = tempQueue;
          
          return onBridges;
      }
  }
  ```

* **他の人の解答**

  特にコメントすることもない似たような解答。  
  違いはトラックの移動を「+」にしたところと「橋上のトラック移動 → 準備トラック移動」ではなく「準備トラック移動 → 橋上のトラック移動」にしたところが異なる。  
  ちなみに私も最初は「準備トラック移動 → 橋上のトラック移動」にしたが、「トラックが橋に完全に上がっていない場合、このトラックの重量は考慮しません」の条件が理解できず、逆順に処理した。

  ```java
  import java.util.*;
  
  class Solution {
      class Truck {
          int weight;
          int move;
  
          public Truck(int weight) {
              this.weight = weight;
              this.move = 1;
          }
  
          public void moving() {
              move++;
          }
      }
  
      public int solution(int bridgeLength, int weight, int[] truckWeights) {
          Queue<Truck> waitQ = new LinkedList<>();
          Queue<Truck> moveQ = new LinkedList<>();
  
          for (int t : truckWeights) {
              waitQ.offer(new Truck(t));
          }
  
          int answer = 0;
          int curWeight = 0;
  
          while (!waitQ.isEmpty() || !moveQ.isEmpty()) {
              answer++;
  
              if (moveQ.isEmpty()) {
                  Truck t = waitQ.poll();
                  curWeight += t.weight;
                  moveQ.offer(t);
                  continue;
              }
  
              for (Truck t : moveQ) {
                  t.moving();
              }
  
              if (moveQ.peek().move > bridgeLength) {
                  Truck t = moveQ.poll();
                  curWeight -= t.weight;
              }
  
              if (!waitQ.isEmpty() && curWeight + waitQ.peek().weight <= weight) {
                  Truck t = waitQ.poll();
                  curWeight += t.weight;
                  moveQ.offer(t);
              }
          }
  
          return answer;
      }
  }
  ```
