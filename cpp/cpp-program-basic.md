# c++のプログラムの基本型

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {

}
```

1 行目 2 行目はあまり重要ではない。とりあえず**main 関数から始まる**ということは覚えておく。

C++のコードは`main{}`における`{`の次の行から実行され、それに対応する`}`に辿り着くとプログラムが終了する。

だから基本的に初めは難しいこと考えず`main{}`内部に記述するコードのことだけ考えればいい。

# Hello world を出力するプログラム

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    cout << "Hello, world!" << endl;
}
```

## cout(シーアウト)

`cout`を使うことで文字列を出力できる。
C++プログラム内部で文字列を扱う場合、`""`で囲う必要がある。Python と同じ.
`endl`は改行を表している。

以下のように複数行にわたって文字列を出力させることもできる。

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  cout << "hey";
  cout << "Hello" << endl;
  cout << "ok!" << endl;
}
```

出力

```
heyHello
ok!
```

# 注意点

C++プログラム内に全角文字が入り込むとエラーになるので注意。
