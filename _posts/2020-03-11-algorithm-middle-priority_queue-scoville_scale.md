---
layout: posts
comments: true
title: 【アルゴリズム_中級】もっと辛く！
tags: [algorithm, java, priority queue, middle level]
category: java
---

* **アルゴリズム分類**

  優先度付きQueue

* **問題説明**

  辛い物が好きなLeoは全ての食べ物のスコヴィル値を「K」以上に作りたいです。

  そのため、スコヴィル値が最も低い二つのためものを以下の方法で混合して、新しい食べ物を作ります。

  ```
  混合した食べ物のスコヴィル値 = 最も辛くない食べ物のスコヴィル値 + 2番目で辛くない食べ物のスコヴィル値 
  ```

  Leoは全ての食べ物のスコヴィル値が「K」以上になるまで、混ぜ続けます。

  Leoが持っている食べ物のスコヴィル値をもって配列「scoville」と求めるスコヴィル値「K」が与えられたら全ての食べ物のスコヴィル値が「K」以上になるまでの最小の混合回数を作成して下さい。

* **制約条件**

  ・scovilleのサイズは「1」以上「1,000,000」以下です。  
  ・Kは「0」以上「1,000,000,000」以下です。  
  ・scovilleの要素は「0」以上「1,000,000」以下です。  
  ・全ての食べ物のスコヴィル値が「K」以上にできない場合は「-1」をreturnします。

* **入出力例**

  | scoville             | K    | return |
  | -------------------- | ---- | ------ |
  | [1, 2, 3, 9, 10, 12] | 7    | 2      |

  1．スコヴィル値、1と2を混ぜたら以下になります。

  　作られる食べ物のスコヴィル値 = 1 + (2 * 2) = 5  
  　持っているため物のスコヴィル値 = [5, 3, 9, 10, 12]

  2．スコヴィル値、3と5を混ぜたら以下になります・

  　作られる食べ物のスコヴィル値 = 3 + (5 * 2) = 13  
  　持っているため物のスコヴィル値 = [13, 9, 10, 12]

  全ての食べ物のスコヴィル値が7以上になり、2回混合しました。

* **自分の解答**

  ```java
  import java.util.PriorityQueue;
  
  class Solution2 {
  
  	/**
  	 * 全ての食べ物が最低のスコヴィル値を満たすまで混合する回数を算出
  	 *
  	 * @param scoville スコヴィル値配列
  	 * @param K 最低のスコヴィル値
  	 * @return 全てのスコヴィル値を満たすまで混ぜえた回数
  	 */
  	public int solution(int[] scoville, int K) {
  
  		PriorityQueue<Integer> priorityQ = new PriorityQueue<>();
  
  		// 配列 → 優先度付きキュー
  		for (int spicyLevel : scoville) {
  			priorityQ.offer(spicyLevel);
  		}
  
  		// mixするカウント
  		int mixCount = 0;
  
  		// 食べ物が2つ以上残っていて、一番辛くない食べ物が基準を満たすまで繰り返す
  		while (priorityQ.size() > 1 && priorityQ.peek() < K) {
  			// 最も辛くない食べ物
  			int notSpicy1st = priorityQ.poll();
  			// 2番目で辛くない食べ物
  			int notSpicy2nd = priorityQ.poll();
  			// 食べ物混合
  			priorityQ.offer(notSpicy1st + (notSpicy2nd * 2));
  			// 混合カウント増加
  			mixCount++;
  		}
          
  		return priorityQ.peek() < K ? -1 : mixCount;
  	}
  }
  
  ```

* **他の人の解答**

  簡単な問題なので、自分の解答と相当異なる解答はなかった。
