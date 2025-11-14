# 条件式の結果と bool 型

条件式の結果は真の時に 1, 偽の時に 0 になる。

```cpp
cout << (5 < 10) << endl; // 1
```

反対に、条件式の部分を数字にすることも可能。

```cpp
if (1) {
    ///
}
```

しかし数値の 1, 0 と勘違いしやすいため、`true`と`false`のみの bool 型が存在する。

```cpp
int main() {
    int x;
    cin >> x;

    bool a = true;
    bool b = x < 10; // 10未満ならtrue, 以上ならfalseが入る
    bool c = false;
}
```
