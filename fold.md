いわゆる `reduce`。再帰関数の以下のようなパターン、基底部を `[]` 、`(x : xs)`のパターンを使ってリストを先頭と残りのリストに分解するというパターンを抽象化したもの:

```haskell
reverse' :: [a] -> [a]
reverse' []       = []
reverse' (x : xs) = reverse' xs ++ [x]
```

## `foldl`

left -> right

```haskell
λ > :t foldl
foldl :: Foldable t => (b -> a -> b) -> b -> t a -> b
```

```haskell
-- foldl 2引数を取る関数, Accumrator(初期値), 畳込み対象のリスト
foldl (\acc x -> acc + x) 0 [1,2,3,4,5]

15
```

`\acc x -> acc + x` は `(+)` と同じなので以下のようにも書ける

```haskell
λ > (+) 1 2
3

λ > foldl (+) 0 [1,2,3,4,5]
15
```

```haskell
sum' xs = foldl (+) 0 xs
```

`foldl (+) 0`  リストを取る関数を返すので、以下のように定義できる:
```haskell
sum' :: (Num a) => [a] -> a
sum' = foldl (+) 0
```

## `foldr`
リストを右から順に処理していく:

```haskell
λ > :t foldl
foldl :: Foldable t => (b -> a -> b) -> b -> t a -> b

λ > :t foldr
foldr :: Foldable t => (a -> b -> b) -> b -> t a -> b
```

```haskell
λ > foldr (\x acc -> acc + x) 0 [1,2,3,4,5]
15
```

```haskell
map' :: (a -> b) -> [a] -> [b]
map' f = foldl (\acc x -> f x : acc) []

map'' :: (a -> b) -> [a] -> [b]
map'' f = foldr (\x acc -> f x : acc) []
```
