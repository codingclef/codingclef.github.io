---
layout: posts
comments: true
title: 【アルゴリズム_初級】電話番号リスト
tags: [algorithm, java, hash, low level]
category: java
---

* **アルゴリズム分類**

  Hash

* **問題説明**

  電話番号リストに記載している電話番号の内、ある番号は他の番号の接頭語の場合があるか確認しようとします。

  電話番号が以下の場合、消防署の電話番号は田中の電話番号の接頭語です。

  　・消防署：119

  　・松村　：97 674 223

  　・田中　：11 9552 4421

  電話番号の持つ配列phone_bookがsolutionの引数に与えられたとき、ある番号が他の番号の接頭語の場合があればfalseを、なければtrueをリターンする関数solutionを作成して下さい。

* **制約条件**

  ・phone_bookのサイズは1以上1,000,000以下です。

  ・各電話番号の長さは1以上20以下です。

* **入出力例**

  | phone_book                    | return |
  | ----------------------------- | ------ |
  | ["119","97674223","1195524421"] | false  |
  | ["123","456","789"] | true  |
  | ["12","123","1235","567","88"] | false  |

* **自分の解答**

  問題の分類がHashなので、二重for分を使わずにマップなどで解答する問題かと思ったが、以下のコードで、効率性テストも通った。Hashを使った解答があるか探してみたが見当たらなかった。

```java
class Solution {
	public boolean solution(String[] phone_book) {
        // startsWith()にて接頭語確認
		for (int i = 0; i < phone_book.length - 1; i++) {
			for (int j = i + 1; j < phone_book.length; j++) {
				if (phone_book[j].startsWith(phone_book[i])) return false;
				if (phone_book[i].startsWith(phone_book[j])) return false;
			}
		}
		return true;
    }
}
```

* **他の人の解答**

  Hashを使用している解答ではないが、炎上している解答を持ってきた。

  まず、Stringソートの特徴（辞書整列）を利用して最も近い値のみ比較を行うため、二重for分よりは効率いいという意見もあるが、ソートが必要になるため、ソートしているのに効率いいのか？という意見もある。

  確かに効率性テストの結果が良くなかったので、ソートより二重for分の方が処理速度は速そうだ。

```java
import java.util.Arrays;

class Solution {
    public boolean solution(String[] phone_book) {
        // ソートする
        Arrays.sort(phone_book);
        boolean result = true;
        for (int i = 0; i < phone_book.length - 1; i++) {
            if (phone_book[i + 1].startsWith(phone_book[i])) {
                result = false;
                break;
            }
        }
        return result;
    }
}
```
