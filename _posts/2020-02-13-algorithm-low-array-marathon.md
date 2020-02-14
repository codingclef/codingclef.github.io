---
layout: posts
comments: true
title: 【アルゴリズム_初級】完走できなかったマラソン選手
tags: [algorithm, java, array, low level]
category: JAVA
---

* アルゴリズム分類

  HashMap

* 問題説明

  マラソン大会で、一人の選手以外は全ての選手が完走しました。
  マラソンに参加した選手の名前は入っている入れる「participant」と完走した選手の名前が入っている配列「completion」が与えられたとき、完走できなかった選手の名前がリターンされるようにsolution関数を作成して下さい。

* 制約条件

  マラソンに参加した選手は1人以上、100,000人以下です。
  「completion」のサイズは「participant」のサイズより１短いです。
  選手の名前は1以上20以下のローマ字の小文字です。
  名前が同じ選手がいる場合があります。
  
* 入出力例

  | **participant**                         | **completion**                   | **return** |
  | --------------------------------------- | -------------------------------- | ---------- |
  | [leo, kiki, eden]                       | [eden, kiki]                     | leo        |
  | [marina, josipa, nikola, vinko, filipa] | [josipa, filipa, marina, nikola] | vinko      |
  | [mislav, stanko, mislav, ana]           | [stanko, ana, mislav]            | mislav     |

  例題#1
  
  　leoは選手名簿にはいるが、完走名簿にはいないので、完走できなかった。
  
  例題#2
  
  　vinkoは選手名簿にはいるが、完走名簿にはいないので、完走できなかった。

  例題#3
  
  　mislavは選手名簿には2人がいるが、完走名簿には1人しかいないので、1人は完走できなかった。

* 自分の解答

  HashMapを使う問題だが、配列だけにしてしまった。
  サイズの制約条件があったので、以下の解答でも問題なかったのかな？

```java
class solution {
	public String solution(String[] participant, String[] completion) {
		// 配列のサイズが異なることを利用し、ソート後比較する。
		Arrays.sort(participant);
		Arrays.sort(completion);

		int i;

		for (i = 0; i < completion.length; i++) {
			if (!participant[i].equals(completion[i])) {
				return participant[i];
			}
		}
		return participant[i];
	}
}
```

* 他の人の解答（コメント by codingClef）

```java
class Solution {
	public static String solution(String[] participant, String[] completion) {
		// 完走できなかった選手名
		String answer = null;

		HashMap<String, Integer> hm = new HashMap<>();
        
		// getOrDefualt() HashMapに入力したキーがなければ、指定した値をデフォルトで入れる
		// 例）participant = [mislav, stanko, mislav, ana]
		// roop① mislav,1
		// roop② stanko,1
		// roop③ mislav,2
		// roop④ ana,1
		for (String player : participant) hm.put(player, hm.getOrDefault(player, 0) + 1);

		// 例）completion = [stanko, ana, mislav]
		// roop① stanko, 0
		// roop② ana, 0
		// roop④ mislav, 1
		for (String player : completion) hm.put(player, hm.get(player) - 1);
		
		// hm.keySet()をしてif条件でhm.get(key)をする方法は良くない
		// getするたびにHashMapをsearchするため、キーとバリュを持ってくる際には
		// entrySet()を使用した方が効率的だ
		for (Entry<String, Integer> entry : hm.entrySet()) {
			if (entry.getValue() != 0) {
				answer = entry.getKey();
			}
		}
        
		return answer;
	}
}
```
