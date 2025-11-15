# for、break、continue

for 文は繰り返しの構文。while よりも短くかける。

```cpp
for (初期化; 条件式; 更新) {
  処理
}
```

たとえば N 回の繰り返し処理は以下のように書く。

```cpp
for (i=0; i < N; i++){
    処理
}
```

break を使うことでループの途中で抜け出すことが可能。また、continue で後の処理を飛ばして次のループへ行ける。

break は大体、
