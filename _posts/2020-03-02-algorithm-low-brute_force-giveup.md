---
layout: posts
comments: true
title: 【アルゴリズム】LEVEL1．数学を諦めた人
category: [algorithm,java,brute-force,LEVEL1]
---

* **アルゴリズム分類**

  力まかせ探索（Brute-Force）

* **問題説明**

  数学を諦めた3人は模擬試験の全ての問題を当てずっぽうに解こうとします。

  3人は1番問題から最後の問題まで以下のように回答します。

  ・1番目の人：1、2、3、4、5、1、2、3、4、5、…

  ・2番目の人：2、1、2、3、2、4、2、5、2、1、2、3、2、4、2、5、…

  ・3番目の人：3、3、1、1、2、2、4、4、5、5、3、3、1、1、2、2、4、4、5、5、…

  1番問題から最後の問題の解答が順次に入っている配列「answers」が与えられたら

  最も当てた人を配列に格納し、returnする関数を作成して下さい。

* **制約条件**

  ・試験は最大10,000題で構成されています。
  
  ・問題の答えは1、2、3、4、5の内1つです。
  
  ・最も高い点数を取った人が一人ではない場合、昇順にソートしてreturnして下さい。
  
* **入出力例**

  | answers     | return  |
  | ----------- | ------- |
  | [1,2,3,4,5] | [1]     |
  | [1,3,2,4,2] | [1,2,3] |

  例#1

  　・1番目の人は全て正答です。

  　・2番目、3番目の人は全て誤答です。
  
  　最も当てた人は「1番目の人」です。
  
  例#2
  
  　・すべての人が2問題ずつ当てました。

* **自分の解答**

  受験者が3人で限られているから作成できたコード。
  
  ```java
  import java.util.*;
  
  class Solution {
  　　public int[] solution(int[] answers) {
        // 回答パターン
        int[] person1 = {1,2,3,4,5};
        int[] person2 = {2,1,2,3,2,4,2,5};
        int[] person3 = {3,3,1,1,2,2,4,4,5,5};
        
        // 当てた数
        int score1 = 0;
        int score2 = 0;
        int score3 = 0;
        
        // 当たったらカウント
        for (int i = 0; i < answers.length; i++) {
            if (answers[i] == person1[i%person1.length]) score1++;
            if (answers[i] == person2[i%person2.length]) score2++;
            if (answers[i] == person3[i%person3.length]) score3++;
        }
        
        // 最高点の取得
        int maxScore = Math.max(Math.max(score1, score2), score3);
        
        // 最高点が複数あることを考慮し、昇順で格納
        List<Integer> maxList = new ArrayList<Integer>();
  
        if (maxScore == score1) maxList.add(1);
        if (maxScore == score2) maxList.add(2);
        if (maxScore == score3) maxList.add(3);
        
        // リストの配列化
        int[] result = new int[maxList.size()];
  
        for (int i = 0; i < maxList.size(); i++){
            result[i] = maxList.get(i);
        }
        
        // 返却
        return result;
    }
  }
  ```
  
* **他の人の解答（コメント by codingClef）**

  他の人の回答の内、これいいねという回答がなかった。
  
  以下のような回答がstreamライブラリを使用していて、お？と思ったが実際に実行してみると相当時間がかかる。for分で一つ一つ処理した方が早い。

  ```java
  import java.util.ArrayList;
  
  class Solution {
      public int[] solution(int[] answer) {
          // 回答パターン
          int[] a = {1, 2, 3, 4, 5};
          int[] b = {2, 1, 2, 3, 2, 4, 2, 5};
          int[] c = {3, 3, 1, 1, 2, 2, 4, 4, 5, 5};
          
          // 当たったらカウント
          int[] score = new int[3];
          
          for(int i=0; i<answer.length; i++) {
              if(answer[i] == a[i%a.length]) {score[0]++;}
              if(answer[i] == b[i%b.length]) {score[1]++;}
              if(answer[i] == c[i%c.length]) {score[2]++;}
          }
          
          // 最高点の取得
          int maxScore = Math.max(score[0], Math.max(score[1], score[2]));
          
          // 最高点が複数あることを考慮し、昇順で格納
          ArrayList<Integer> list = new ArrayList<>();
          
          if(maxScore == score[0]) {list.add(1);}
          if(maxScore == score[1]) {list.add(2);}
          if(maxScore == score[2]) {list.add(3);}
          
          // リストを配列に変換しながら、返却
          return list.stream().mapToInt(i->i.intValue()).toArray();
      }
  }
  ```
