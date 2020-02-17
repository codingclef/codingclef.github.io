---
layout: posts
comments: true
title: 【アルゴリズム_初級】スパイの偽装
tags: [algorithm, java, hash, low level]
category: java
---

* **アルゴリズム分類**

  Hash

* **問題説明**

  スパイは毎日異なる衣装を組合せて偽装します。

  例えば、スパイが保有している衣装が以下であり、今日は丸いメガネ・ロングコート、青いTシャツを着たら、明日はジンズを追加で切るか、丸いメガネの代わりにサングラスをかけないとならないです。

  | 種類     | 名前                   |
  | -------- | ---------------------- |
  | 顔       | 丸いメガネ、サングラス |
  | トップス | 青いTシャツ            |
  | パンツ   | ジンズ                 |
  | アウター | ロングコート           |

  スパイが保有している衣装を持つに次元配列「clothes」が与えられたら、組合せできる数をリターンする関数solutionを作成して下さい。

* **制約条件**

  ・clothesの各行は [衣装名、衣装種類]で組み立てられています。  
  ・スパイが保有している衣装の数は1以上30以下です。  
  ・同じ名前の衣装が存在しません。  
  ・clothesの全ての要素は文字列です。  
  ・文字列の長さは1以上20以下の自然数であり、ローマ字の小文字、または'_'だけ成っています。  
  ・スパイは一日1つ以上衣装を着ます。

* **入出力例**

  | clothes                                                      | return |
  | ------------------------------------------------------------ | ------ |
  | [[yellow_hat, headgear], [blue_sunglasses, eyewear], [green_turban, headgear]] | 5      |
  | [[crow_mask, face], [blue_sunglasses, face], [smoky_makeup, face]] | 3      |

  例題#1

  headgearが「yellow_hat, green_turban」、eyewearが「blue_sunglasses」なので、以下の通り、5つの組み合わせができます。

  ```
  1. yellow_hat
  2. blue_sunglasses
  3. green_turban
  4. yellow_hat + blue_sunglasses
  5. green_turban + blue_sunglasses
  ```

  例題#2

  faceが「crow_mask, blue_sunglasses, smoky_makeup」なので、以下の通り、3つの組み合わせができます。

  ```
  1. crow_mask
  2. blue_sunglasses
  3. smoky_makeup
  ```

* **自分の解答**

  ```java
  class Solution {
      public int solution(String[][] clothes) {
          // 掛け算するため、「1」に初期化
          int answer = 1;
          // 衣装別の数量を持つマップ
          HashMap<String, Integer> clothesMap = new HashMap<String, Integer>();
          // 衣装分類、数量をマップにput
          for (int i = 0; i < clothes.length; i++) {
              clothesMap.put(clothes[i][1], clothesMap.getOrDefault(clothes[i][1], 0) + 1);
          }
          // 衣装の組合せ
          for (String key : clothesMap.keySet()) {
              answer *= clothesMap.get(key) + 1;
          }
          // 何も着ないケースを除外する「-1」
          return answer - 1;
  }
  ```
  
* **他の人の解答①**（コメント by codingClef）

  私の解答と大した違いはない。
  
  getOrDefault()を使わず、解いて実装したこととマップを参照する方法が異なる。

  ```java
  import java.util.HashMap;
  import java.util.Iterator;
  class Solution {
      public int solution(String[][] clothes) {
          // 掛け算するため、「1」に初期化
          int answer = 1;
          // 衣装別の数量を持つマップ
          HashMap<String, Integer> map = new HashMap<>();
          // getOrDefault()と同様
          for(int i=0; i<clothes.length; i++){
              String key = clothes[i][1];
              if(!map.containsKey(key)) {
                  map.put(key, 1);
              } else {
                  map.put(key, map.get(key) + 1);
              }
          }
          // マップバリューの読み取り
          Iterator<Integer> it = map.values().iterator();
          // 衣装の組合せ
          while(it.hasNext()) {
              answer *= it.next().intValue()+1;
          }
          // 何も着ないケースを除外する「-1」
          return answer-1;
      }
  }
  ```
  
* **他の人の解答②**（コメント by codingClef）

  Java8から登場した、stream()を使用した解答。

  動きはある程度理解したが、各種メソッドの正確な定義についてはまだ理解できていない。

  （Generic、ラムダ式の知識が足りない…）

  ```
  stream() 要素を参照する
  collect() 処理後、求めるデータ型に変換する
  groupingBy() グループ化する（キーとバリューのマッピング）
  mapping() バリューの設定
  counting() バリューを蓄積する
  reducing() マッピング内容を元に演算を行う
  ```

  ```java
  import java.util.*;
  import static java.util.stream.Collectors.*;
  
  class Solution {
      public int solution(String[][] clothes) {
          　　　　// clothesの要素を読み取る
          return Arrays.stream(clothes)
              　　// clothes[1]がキー、p[0]をバリューとしてマッピング。
                  .collect(groupingBy(p -> p[1], mapping(p -> p[0], counting())))
             　　 // マッピングしたマップを取得
                  .values()
              　　// マップを読み取る
                  .stream()
           　　   // 組合せ実施後、何も着ないケース除外
                  .collect(reducing(1L, (x, y) -> x * (y + 1))).intValue() - 1;
      }
  }
  ```
