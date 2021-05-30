#### 二分探索って何？
合格発表の時に自分の受験番号があるかどうか、どのようにして探しましたか？
（受験番号がとても小さかった人以外）1番最初から番号を一つ一つ順に見ていく人はいないと思います。まず、パネルの最初の番号をみて自分の番号台に入るパネルはどれか探し、次にパネルの中で左なのか真ん中なのか右なのか見てみて...えっと...あった！とかするでしょう。

あるいは、辞書で単語を調べていて、その単語が「O」で始まるとします。「A」から順番に単語を検索できますが、真ん中あたりから始めるほうが合理的です。
これは探索問題で、これらのケースにおいて、問題の解決に **二分探索（binary search）** というアルゴリズムを使うことができます。

二分探索は本当にすごいアルゴリズムです。自分はクラス分けの表が配られたときは二分探索を使って自分の名前を探します。自分の生活に応用できたりするので、ぜひ、習得してください！
#### 数あてゲーム
二分探索の例として、Bさんは1から128までの間にある数字を1つ思い浮かべるとします。
AさんはBさんが思い浮かべた数字をできるだけ少ない回数で言い当てたいです。Aさんが数字を答えるたびに、Bさんはその数字が小さすぎる（too low）、大きすぎる（too high）、または正解であることを伝えます。
1、2、3、4...といった具合に推測を始めると、次のようになり、かなり効率が悪いです。

- A 「1!」
- B 「Too low」
- A 「2!」
- B 「Too low」
- A 「3」
- B 「Too low」
- A 「4...」
- B 「Too low」
- A 「ツカレタ」

これは単純探索（simple search）です。数字を推測するたびに、数字を1つだけ消していきます。Bさんが思い浮かべた数字が127 だった場合は、正解にたどり着くまでに127回も推測することになってしまいます。

もっとよい方法があります。64から始めるのです。

- A 「64!」
- B 「Too low」

64は小さすぎますが、数字の半分が除外されます。この時点で1から64までの数字はすべて小さすぎることがわかります。次に、96と推測します。

- A 「96!」
- B 「Too high」

96は大きすぎますが、この場合も、残りの数字の半分が除外されます。二分探索では、毎回中央の数字を推測し、残りの数字の半分を除外します。次に、（64と96の中間である）80と推測します。
- A 「80!」
- B 「Too low」
- A 「88!」
- B 「Too high」
- A 「84!」
- B 「Too low」
- A 「86!」
- B 「Too high」
- A 「85!!」
- B 「Yes！！！！！！！！！！！！！！！！！！！！！！！！！」

Bさんがどの数字を思い浮かべたとしても、数字を1つ推測するたびに半分の数字が除外されるため、最大7回で言い当てることができます。
#### 計算量
なぜ最大で7回なのでしょうか？
それは128を2で割り続けると7回で1になるからです。別の言い方をすると1に2を掛け続けると7回で128以上になるからです。一般的には、N個の要素からなるリストでは、最悪の場合、二分探索では(\(log_2 N\))ステップが必要になります。

\(log\)を知らないかもしれません。少なくとも中2までは学校で習いませんが、\(log_2 A\)は「2を何回掛けたらAになるか」を表わしています。
- \(log_2 8  == 3 (2^3 == 8)\)
- \(log_3 9  == 2 (3^2 == 9)\)

数あてゲームの計算量は\(O(logN)\)になります。(計算量は\(O()\)で囲みます。)
#### logの速さ
果たして\(log\)はどれくらい速いのでしょうか。\(N\) と \(logN\)を比べてみましょう。
|    \(N\)    |  10  |  1000  |  1000000  |  1000000000000000000  |
|    ----    | ---- |  ----  |    ----    |         ----         |
|  \(logN\)  |   4   |   10   |    20    |           60           |
計算量 \(O(logN)\) は**とてつもなく高速**であることが分かります。\(log\)最強！
ということは、二分探索めっちゃ速い！！

数あてゲームの解き方や\(log\)は[アルゴリズム・AtCoder のための数学【前編：数学的知識編①】](https://qiita.com/e869120/items/b4a0493aac567c6a7240)の**2-3章や3-2章**に書かれています。図も入っていて分かりやすいです。
#### 注意点
二分探索は**ソート(小さい順に)したものの中でしか行えません**。例えば`1, 6, 5, 2, 8`という数列の中で6が何番目にあるかを求めたい時、このまま二分探索を行うことはできません。
なぜなら、二分探索だと真ん中の5を見て、「`6 > 5`より6は真ん中より右にある」となりますが、実際はそうではありません。6は真ん中の5より左にあります。
なので、`1, 6, 5, 2, 8`をソートして`1, 2, 5, 6, 8`とした状態で二分探索をしましょう。
#### C++での二分探索の書き方
> **問題文**
> 正整数 \(N\) と長さ \(N\) の数列 \(A\) が与えられます。
> \(0\) 以上 \(N\) 未満の全ての \(i\) について \(A_i\) が数列の中で何番目に小さいかを求めなさい。
> **制約**
> - \(1 \leqq N \leqq 10^5\)
> - \(1 \leqq A_i \leqq 10^9\)
> - `入力に含まれる値はすべて整数`
> - `Aに含まれる値はすべて異なる`

> **入力例**
> \(5\)
> \(1\) \(6\) \(5\) \(2\) \(8\)
> **出力例**
> \(1\) \(4\) \(3\) \(2\) \(5\)

このような問題があった時、どのように解けばいいのでしょうか。
まず```A```をソートしたものを```sortA```とします。```sortA```の中から```A[i]```を見つけることは、「`sortA[x] >= A[i]`となる最小のxを求める」と言い替えることができます。よって、
- x = left は絶対に`sortA[x] >= A[i]`を満たさない
- x = right は絶対に`sortA[x] >= A[i]`を満たす

この2つが成立するように left と right を設定すれば、left と right の間に`sortA[x]>=A[i]`を満たすような境目があるので、left と right の間を縮めていけばいいことになります。
具体的には、最初に、`left == -1, right == A.size()`と置けばよいです。

left と right の真ん中 mid が`sortA[mid] >= A[i]`ならば right を mid に、そうでないならば left を mid にすることで上の2つの条件を保ちながら検索範囲を半分にすることができます。
気持ちとしてはこんな感じです。
- left は常に条件を満たさない
- right は常に条件を満たす
- right - left = 1 になるまで範囲を狭める (最後は right が条件を満たす最小)

これをめぐる式二分探索と言ったりします。
[この記事](https://qiita.com/drken/items/97e37dd6143e33a64c8c)がとても分かりやすいので、よく分からなかったという人はぜひ読んでみてください。
##### 実装例
C++がワカラナイ！という人は[APG4b](https://kclc-kaisei.github.io/APG4b/1-0/)を見てください。第1章が分かれば十分だと思います。
```c++
#include<bits/stdc++.h>
using namespace std;

int main() {
	//入力
	int N; cin >> N;
	
	vector<int> A(N), sortA(N);
	for (int i = 0; i < N; i++) {
		cin >> A[i]; sortA[i] = A[i];
	}
	sort(sortA.begin(), sortA.end());//AをsortしたものがsortA
	
	for (int i = 0; i < N; i++) { //全てのA[i]を見る
		int left = -1;
		int right = A.size();
		int mid = (left + right) / 2; //leftとrightの中間
		
		while (right - left > 1) { //leftとrightの差が1になるまで続ける
			if (sortA[mid] >= A[i]) { right = mid; }
			else { left = mid; }
			mid = (left + right) / 2;
		}
		
		if (i != 0) { cout << ' '; }
		cout << right + 1;
	}
	cout << endl;
	return 0;
}
```
これの計算量は\(O(NlogN)\)になります。
#### いつ使うの？
**最小値の最大化**などは使えることが多いです。一発で最大値を求めるのが難しいとき、二分探索を使うことで、（二分探索の中で）ある値\(X\)がOKか？という判定問題に変えることができます。慣れるためにも、二分探索の問題をたくさん解くことが大事になってきます。
二分探索の使い方は[この記事](https://betrue12.hateblo.jp/entry/2019/05/11/013403)に詳しく書かれています。
#### lower_boundとupper_bound
配列 a があり、a の l 番目から r - 1 番目までの要素が小さい順にソートされていたとします。
そのとき、lower_bound(a + l, a + r, x) - a というプログラムは、
- `a[l]` から `a[r - 1]` までの中で初めて x 以上となるようなインデックスを返します。
- 例えば、a = {2, 3, 4, 6, 9} の場合、lower_bound(a, a + 5, 7) - a = 4 となります。7以上の最小の数9は（最初を0番目として）4番目だからです。

*lower_bound(a + l, a + r, x) というプログラムは、
- `a[l]` から `a[r - 1]` までの中で初めて x 以上となるような数そのものを返します。
- 例えば、a = {2, 3, 4, 6, 9} の場合、*lower_bound(a, a + 5, 7) = 9 となります。7以上の最小の数は9だからです。

upper_boundにすると**x 以上**の部分が**xより大きい**になります。

また、**「key以上の要素の最小値そのもの」がない場合**は5になったりa.end()になったりと**面倒なことになる**ので気をつけましょう。
[この記事](https://qiita.com/e869120/items/518297c6816adb67f9a5#3-16-lower_bound)に詳しく書かれています。
#### 練習問題
[JOI 2009 本選 2 - ピザ](https://atcoder.jp/contests/joi2009ho/tasks/joi2009ho_b)
[ABC077-C](https://atcoder.jp/contests/abc077/tasks/arc084_a)
[競プロ典型 90 問-001](https://atcoder.jp/contests/typical90/tasks/typical90_a)最小値の最大化です。
[ABC034-D](https://atcoder.jp/contests/abc034/tasks/abc034_d) ムズカシイです。**平均の最大化** は二分探索が使えることが多いです。
[atcoder-tags](https://atcoder-tags.herokuapp.com/tags/Searching/Binary-Search)　ここに二分探索の問題がたくさん載っています。
#### 参考文献
https://codezine.jp/article/detail/9900?p=2
https://qiita.com/drken/items/97e37dd6143e33a64c8c
https://qiita.com/e869120/items/eb50fdaece12be418faa