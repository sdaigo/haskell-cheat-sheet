## Currying


```
λ > :t max
max :: Ord a => a -> a -> a
```

`max` の型は以下の様にも書ける:

```haskell
max :: (Ord a) => a -> (a -> a)
```

(引数`a` )を取って、(引数`a` を取り`a` を返す関数)を返す

```haskell
λ > let maxWithThree = max 3

λ > :t maxWithThree
maxWithThree :: (Ord a, Num a) => a -> a

λ > maxWithThree 5
5
```

カリー化は関数を本来より少ない数の引数で呼び出したときに **部分適用** された関数を得られる。
部分適用された関数は残りの変数を引数としてとる関数:

```haskell
multiThree :: Int -> Int -> Int -> Int
multiThree x y z = x * y * z
```

`multiThree` の型は `Int -> (Int -> (Int -> Int))` とも書けるので、以下のように呼び出せる:

```haskell
λ > let multiTwoWithNine = multiThree 9

λ > multiTwoWithNine 2 3
54
```

### Section

中置関数も同様に部分適用できる:

```haskell
λ > let divideByTen = (/ 10)
λ > divideByTen 100
10.0

λ > 100 / 10
10.0

λ > (/10 ) 100
10.0

λ > (*2) 100
200

λ > (+3) 200
203

λ > (-3) 200

<interactive>:15:1: error:
    • Non type-variable argument in the constraint: Num (t1 -> t2)
      (Use FlexibleContexts to permit this)
    • When checking the inferred type
        it :: forall t1 t2. (Num t1, Num (t1 -> t2)) => t2

λ > (subtract 3) 200
197
```

NOTE:
* `-` の場合は`(-3)` のようにすると「マイナス3」を意味することになるので、 `subtract` を使う必要がある

## `map`

```haskell
λ > :t map
map :: (a -> b) -> [a] -> [b]
```

```haskell
λ > map (*10) [1,2,3]
[10,20,30]

λ > map fst [(1,2),(3,4),(5,6)]
[1,3,5]
```

## `filter`

```haskell
λ > :t filter
filter :: (a -> Bool) -> [a] -> [a]
```

```haskell
λ > filter even [1..10]
[2,4,6,8,10]

λ > filter (>3) [1..5]
[4,5]

λ > filter (`elem` ['a'..'z']) "AbcDEfGhI"
"bcfh"
```

## lambda

高階関数に渡す関数を作る:

```
\xs -> length xs
```

```haskell
λ > map (\(a, b) -> a + b) [(1,2),(3,4),(5,6),(7,8)]
[3,7,11,15]
```
