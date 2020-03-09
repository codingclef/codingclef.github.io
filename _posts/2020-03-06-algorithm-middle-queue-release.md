---
layout: posts
comments: true
title: 【アルゴリズム_中級】リリース作業
tags: [algorithm, java, queue, middle level]
category: java
---

* **アルゴリズム分類**

  Stack&Queue

* **問題説明**

  ある会社では機能改善作業を行っています。各機能は進捗が100%になったらリリースできます。

  また、各機能の開発速度が異なるため、優先度が低い機能が高い機能より先に開発完了になることもあるし、この時は優先度が高い機能がリリースされるときに一緒にリリースすることになります。

  優先度順で作業進捗が入ってある定数配列「progresses」と各作業の開発速度が入ってる定数配列「speeds」が与えられたら各リリース時にいくつの機能が同時にリリースされるかをreturnする関数を作成して下さい。

* **制約条件**
  
  ・作業数（progresses、speeds配列のサイズ）は100以下です。  
  ・作業進捗は100未満の自然数です。  
  ・作業速度は100以下の自然数です。  
  ・リリースは1日1回でき、その日の最後にするとします。例えば、進捗率が95%の作業の開発速度が一日4％ならリリースは2日後に行われます。
  
* **入出力例**

  | progresses | speeds   | return |
  | ---------- | -------- | ------ |
  | [93,30,55] | [1,30,5] | [2,1]  |

  1番目の機能は現在進捗率が93%であり、1日1%ずつ作業ができるので、7日間作業後リリースされます。  
  2番目の機能は現在進捗率が30%であり、1日30%ずつ作業ができるので、3日間作業後リリースされます。しかし、1番目の機能がまだ開発中であるため、1番目機能と同時に7日後にリリースされます。  
  すなわち、7日目に2つ（1番目、2番目）、9日目に1つ（3番目）がリリースされます。

* **自分の解答**

  問題分類と内容がStack、QueueなのでStack、Queueで解きたいと思ったが、まだStack、Queueと親しくなく...
  以下の3段階に考えて解いた。  
  　① 各機能の完成まで日数を算出して  
  　② 優先順を考慮し各機能がリリース可能な日付を算出  
  　③ 最後に一日リリースされる機能の数を算出
  
  ```java
  import java.util.ArrayList;
  import java.util.LinkedHashMap;
  import java.util.List;
  import java.util.Map;
  import java.util.Map.Entry;
  
  public class Solution {
  
    /**
     * 一日リリース機能の数を算出
     *
     * @param progresses リリース順の現在進捗
     * @param speeds 各作業のスピード
     * @return 各リリース日にリリースされる機能の数
     */
      public int[] solution(int[] progresses, int[] speeds) {
          // 各機能の完成まで日数算出
          List<Integer> requiredDays = getRequiredDays(progresses, speeds);
          // リリース可能日の算出
          List<Integer> releaseDays = getReleaseDays(requiredDays);
          // 日ごとのリリース件数の算出
          int[] releaseCounts= getReleaseCount(releaseDays);
          // 結果リターン
          return releaseCounts;
      }
      
    /**
     * 各機能の完成まで日数算出
     *
     * @param progresses リリース順の現在進捗
     * @param speeds 各作業のスピード
     * @return 各機能の完成まで日数
     */
      public List<Integer> getRequiredDays (int[] progresses, int[] speeds) {
          
          List<Integer> requiredDays = new ArrayList<Integer>();
  
          for (int i = 0; i < (progresses.length); i++) {
              // 各機能の完成まで掛かる日
              requiredDays.add(getRequiredDays(progresses[i], speeds[i]));
          }
  
          return requiredDays;
      }
      
    /**
     * 機能の完成まで日数算出
     *
     * @param progress 進捗
     * @param speeds 作業速度
     * @return 完成までの日数
     */
      public int getRequiredDays (int progress, int speed) {
  
          int day = (100 - progress) / speed;
  
          if ((100 - progress) % speed > 0) {
              day+=1;
          }
  
          return day;
      }
  
    /**
     * 各機能のリリース可能日算出
     *
     * @param requiredDays 各機能の完成まで日数
     * @return 各機能のリリース可能日
     */
      public List<Integer> getReleaseDays (List<Integer> requiredDays) {
  
          List<Integer> releaseDays = new ArrayList<Integer>();
  
          // 初リリース日
          releaseDays.add(requiredDays.get(0));
  
          for (int i = 0; i < (requiredDays.size() - 1); i++) {
              if (releaseDays.get(i) >= requiredDays.get(i+1)) {
                  // 優先度が低い機能が先に完成する場合
                  releaseDays.add(releaseDays.get(i));
              } else {
                  // 優先度が高い機能が先に完成する場合
                  releaseDays.add(requiredDays.get(i+1));
              }
          }
          
          return releaseDays;
      }
  
    /**
     * 日ごとのリリース件数算出
     *
     * @param releaseDays 各機能のリリース可能日
     * @return 日ごとのリリース件数
     */
      public int[] getReleaseCount (List<Integer> releaseDays) {
          // 順番を守るためにLinkedHashMap
          Map<Integer, Integer> map = new LinkedHashMap<Integer, Integer>();
  
          for (int i = 0; i < releaseDays.size(); i++) {
              // 日ごとにリリースされる件数をカウントする
              map.put(releaseDays.get(i), map.getOrDefault(releaseDays.get(i), 0) + 1);
          }
      // LinkedHashMap → 配列
  		int[] releaseCount = new int[map.size()];
          
  		int index = 0;
          
  		for (Entry<Integer, Integer> entry : map.entrySet()) {
  			releaseCount[index] = entry.getValue();
  			index++;
  		}
          
          return releaseCount;
      }
  }
  ```
  
* **他の人の解答①（コメント by codingClef）**

  この解答もStack、Queueではない。ただのラムダ式。  
  行数を見て驚いたけど、実際に実行してみたらパフォーマンスがすごく悪かった。  
  自分の解答は全ケース1.5ms以内だが、以下は15ms~20msだった。  
  ~~地獄のラムダ解析は後回し...~~
  
  ```java
  import java.util.ArrayList;
  import java.util.Arrays;
  
  class Solution {
      public int[] solution(int[] progresses, int[] speeds) {
          int[] dayOfend = new int[100];
          int day = -1;
          for(int i=0; i<progresses.length; i++) {
              while(progresses[i] + (day*speeds[i]) < 100) {
                  day++;
              }
              dayOfend[day]++;
          }
          return Arrays.stream(dayOfend).filter(i -> i!=0).toArray();
      }
  }
  ```

* **他の人の解答②（コメント by codingClef）**

  これはQueueを使用している。解析してみよう。

  ```java
  import java.util.*;
  
  class Solution {
      public int[] solution(int[] progresses, int[] speeds) {
          Queue<Integer> q = new LinkedList<>();
          List<Integer> answerList = new ArrayList<>();

          for (int i = 0; i < speeds.length; i++) {
              // 完了までの日数算出
              double remain = (100 - progresses[i]) / (double) speeds[i]; // 切り上げのため、double型で演算
              int date = (int) Math.ceil(remain); // 切り上げ
  
              if (!q.isEmpty() && q.peek() < date) {
                  // リリース待ち機能がない かつ 優先度が低に作業が遅く終わる場合
                  answerList.add(q.size()); // 今までリリースを待ってた機能の件数を一日リリース数として格納
                  q.clear(); // Queue初期化
              }

              // Queueに格納
              q.offer(date);
          }
          // 最後にたまる機能の格納
          answerList.add(q.size());
          
          // リストの配列化
          int[] answer = new int[answerList.size()];
  
          for (int i = 0; i < answer.length; i++) {
              answer[i] = answerList.get(i);
          }
  
          return answer;
      }
  }
  ```

