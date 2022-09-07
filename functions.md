# リスト内包表記

```haskell
-- [Output | Generator]
[x * 2 | x <- [1..100]]
```

```haskell
-- [Output | Generator | Predicate(filter)]
[x * 2 | x <- [1..10], x > 5]
```

# Pattern match
関数の本体を場合分け。引数の構造で場合分けする:

```haskell
echo :: Int -> String
echo 0 = "Zero"
echo 1 = "One"
echo 2 = "Two"
echo 3 = "Three"
echo 4 = "Four"
echo x = "Too Big"
```

```
λ > echo 1
"One"

λ > echo 5
"Too Big"
```

関数を再帰的に定義する:

```haskell
factorial :: Int -> Int
factorial 0 = 1
factorial x = x * factorial (x - 1)
```

```haskell
head' :: [a] -> a
head' []      = error "No head for empty list"
head' (x : _) = x
```

NOTE: 
* すべてに合致するパターンを最後に入れる

## Guard
関数を定義する際に、引数の値が満たす性質で場合分けする:

```haskell
threshold x
  | x > 1.0 = "outlier"
  | x >= 0.5 = "success"
  | otherwise = "failed"
```

## `where`
Guard の後に置いて、Guard 部分から参照可能な変数や関数を定義できる:

```haskell
calcBmi w h
  | bmi <= 18.5 = "underweight"
  | bmi <= 25.0 = "normal"
  | bmi <= 30.0 = "fat"
  | otherwise = "whale"
  where bmi = w / h ^ 2
```

```haskell
initials :: String -> String -> String
initials firstname lastname = [f] ++ "." ++ [l] ++ "."
  where (f:_) = firstname
        (l:_) = lastname
```


NOTE: 
*  異なる引数のパターンを持つ同名の関数から `where` で定義した変数・関数は参照できない

## `let .. in`

`let` 式はどこでも変数を束縛でき、ガード感では共有されない:

```
let [bindings]
    [bindings]
    ...
in  [expression] <------- [bindings] は in の中でだけ参照できる
```

```haskell
let a = 1
    b = 2
in a + b
```

リスト内包表記で使う:
```haskell
[bmi | (w, h) <- [(60, 1.7)], let bmi = w / h ^ 2]

-- With predicate 
[bmi | (w, h) <- [(60, 1.7)], let bmi = w / h ^ 2, bmi > 25.0]
```
