# エラーの種類

## コンパイルエラー

プログラムの文法ミス。エラー発生時は一切動作しない。

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
//↓に全角スペース
　cout << "Hello, world!" << endl;
}

```

全角スペースによるコンパイルエラーは以下。

```
./Main.cpp:6:1: error: stray ‘\343’ in program
 　cout << "Hello, world!" << endl;
 ^
./Main.cpp:6:1: error: stray ‘\200’ in program
./Main.cpp:6:1: error: stray ‘\200’ in program

```

## 論理エラー

プログラムは一見正しく動作しているが、結果がおかしい時。
発見が難しい。
