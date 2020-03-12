---
layout: posts
comments: true
title: 【アルゴリズム】LEVEL①．体操服
tags: [algorithm, java, greedy algorithm]
category: java
---

* **アルゴリズム分類**

  貪欲法（greedy algorithm）

* **問題説明**

  お昼時間に泥棒が入り、一部学生の体操服が盗まれました。幸せにして余分の体操服を持っている学生がいて体操服を貸します。学生たちの番号は体格順になっていて、すぐ前の番号の学生かすぐ後ろの番号の学生しか貸せないです。例えば4番学生は3番か5番にしか貸せないです。体操服がなければ授業参加ができないため、適宜に貸してできるだけ多くの学生たちが授業を受けるようにしないとならないです。

  全体学生の数「ｎ」、体操服を盗まれた学生の番号が入っている配列「lost」、余分の体操服を持っている学生の番号が入っている配列「reserve」が与えられたら、授業を受けられる最大学生数をreturnするsolution関数を作って下さい。

* **制約条件**

  ・全体学生の数は「2」以上「30」以下です。  
  ・体操服を盗まれた学生の数は「1」以上「ｎ」以下であり、重複はないです。  
  ・余分の体操服を持っている学生の数は「1」以上「ｎ」以下であり、重複はないです。  
  ・余分の体操服がある学生のみ、体操服を貸すことができます。  
  ・余分の体操服がある学生が体操服を盗まれることがあります。  
  　この場合、1つのみ盗まれたとして残った体操服が1つなので、貸すことができないです。

* **入出力例**

  | n    | lost   | reserve   | return |
  | ---- | ------ | --------- | ------ |
  | 5    | [2, 4] | [1, 3, 5] | 5      |
  | 5    | [2, 4] | [3]       | 4      |
  | 3    | [3]    | [1]       | 2      |

  例題#1

  　1番学生が2番学生に貸して3番か5番学生が4番学生に貸したら、5人が授業を受けられます。

  例題#2

  　3番学生が2番か4番学生に貸したら、4人が授業を受けられます。

  例題#3

  　1番学生が3番学生に貸せないため、2人が授業を受けられます。

* 出所

  https://hsin.hr/coci/archive/2009_2010/contest6_tasks.pdf

* **自分の解答**

  ```java
  class Solution {
      
      /**
      * @param n 全体学生数
      * @param lost 盗まれた学生数
      * @param reserve 余分がある学生数
      * @return 体育時間に参加できる学生数
      */
      public int solution(int n, int[] lost, int[] reserve) {
          
          int[] students = new int[n + 1];
          
          // 体操服の初期値
          for (int i = 1; i <= n; i++) {
              students[i] = 1;
          }
          
          // 余分がある学生
          for (int reserveStudent : reserve) {
              students[reserveStudent]++;
          }
  
          // 盗まれた学生
          for (int lostStudent : lost) {
              students[lostStudent]--;
          }
          
          for (int i = 1; i <= n; i++) {
              if (students[i] == 0) {
                  // 体操服がない場合
                  if (i - 1 > 0 && students[i - 1] == 2) {
                      // 前の学生に余分がある
                      students[i - 1]--;
                      students[i]++;
                  } else if (i + 1 <= n && students[i + 1] == 2) {
                      // 後ろの学生に余分がある
                      students[i + 1]--;
                      students[i]++;
                  }
              }
          }
          
          // 参加できる学生の数
          int answer = 0;
          
          for (int student : students) {
              if (student > 0) {
                  // 体操服を持っている学生数
                  answer++;
              }
          }
  
          return answer;
      }
  }
  ```

* **他の人の解答**

  HashSetを活用した解答。
  
  ロジック自体はそんなに相違ないがHashSetについて勉強になりそう。
  
  ```java
  import java.util.*;
  
  class Solution {
      public int solution(int n, int[] lost, int[] reserve) {
          int answer = n;
          HashSet<Integer> resList = new HashSet<>();
          HashSet<Integer> losList = new HashSet<>();
  
          for (int i : reserve)
              resList.add(i);
          for (int i : lost) {
              if(resList.contains(i))
                  resList.remove(i);
              else
                  losList.add(i);
          }
          for (int i : lost) {
              if(losList.contains(i)) {
                  if(resList.contains(i-1))
                      resList.remove(i-1);
                  else if(resList.contains(i+1))
                      resList.remove(i+1);
                  else
                      answer--;
              }
          } 
          return answer;
      }
  }
  ```
