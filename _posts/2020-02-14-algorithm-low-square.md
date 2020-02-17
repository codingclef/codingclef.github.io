---
layout: posts
comments: true
title: 【アルゴリズム_初級】自乗数の判断
tags: [algorithm, java, square, low level]
category: [algorithm,java]
---
* **アルゴリズム分類**

  なし

* **問題説明**

  任意の正の整数ｎに対して、ｎがある正の整数ｘの自乗かどうかを判断しようとします。
  ｎが制の整数ｘの自乗であれば「ｘ+１の自乗」をリターンし、ｎが制の整数ｘの自乗ではなければ、「-1」をリターンする関数を作成して下さい。
  
* **制約条件**

  ｎは1以上、50000000000000以下の正の整数です。

* **入出力例**

  | **n** | **return** |
  | ----- | ---------- |
  | 121   | 144        |
  | 3     | -1         |

  例#1

  121は正の整数11の自乗なので、（11+1）の自乗である144をリターンする。

  例#2

  3は正の整数の自乗ではないので、-1をリターンする。

* **自分の解答**

  ```java
class Solution {
  	public long solution(long n) {
  
  		long answer = 0;
          
  		// Math.sqrt() double値の正しく丸めた正の平方根を返却
  		// 例① n=144 → 12.0 →（int変換）→ 12
  		// 例② n=3 → 1.7320508075688772 →（int変換）→ 1
  		// Math.pow() 番目の引数を、2番目の引数で累乗した値を返却
  		// 例① 12の2乗 = 144 = n
  		// 例② 1の2乗 = 1 ≠ n
  		if (Math.pow((int) Math.sqrt(n), 2) == n) {
  			answer = (long) Math.pow((int) Math.sqrt(n) + 1, 2);
  		} else {
  			answer = -1;
  		}
  
  		return answer;
  	}
}
  ```

* **他の人の解答**

    この人はライブラリを使わず、単純計算で解答した。
    入力値を割り算して、割り算した値と同じ値が出るのかを確認しているが、ｉがｎまで行く必要はない。
    ｎ/ 2 + 1 以降の値は自乗してもｎになることはないためだ。
    もし「ｎ/ 2以降」にすると4の場合、ｎ/ 2が4の 平方根になるので、「ｎ/ 2 + 1」の方が確実である。
  
  ```java
  class Solution {
  	public long solution(long n) {
  		if (n == 1) {
  			return 4;
  		}
  		for (long i = 2; i < n; i++) {
  			if (n / i == i && n % i == 0) {
  				return (i + 1) * (i + 1);
  			}
  		}
  		return -1;
  	}
  }
  ```
  
  