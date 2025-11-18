# 文字と文字列

文字列変数内の文字数を`size()`で取得できる。

```cpp
int main() {
    string A;
    A = "Hello";
    cout << A.size() << endl;
}
```

出力は 5 である。`文字列変数.size()`で取得できる。

i 番目の文字を抽出する場合は`at(i)`で取得できる。

```cpp
int main() {
    string A;
    A = "Hello";
    cout << A.at(0) << endl;
}
```

出力は`H`である。

# 文字型 char

文字単体を格納するデータ型は`char`である。

```cpp
char a = 'c'
```

シングルクオートであることに注意。

先ほどの`.at(i)`で取得される文字はこの`char`型で返ってくる。

これを利用して文字列中の特定の文字を書き換えることもできる。

```cpp
int main() {
    string A = "Hello";
    char a = 'M';
    A.at(0) = a;
    cout << A << endl;
}
```

出力は`Mello`になる。

## 文字列の連結

文字列は`+`演算子によって連結が可能。Python と同じ。

## 入力の文字分割

char 型に対して string の入力を入れると、文字分割できる。

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  char a, b;
  cin >> a >> b;

  cout << a << endl;
  cout << b << endl;
}
```

入力

```
OK
```

出力

```
O
K
```
