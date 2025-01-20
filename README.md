# swizzle-set

```hsakell
import Data.SwizzleSet qualified as SwzS
import Data.SwizzleSet.TH

type TupInt10 = (Int, Int, Int, Int, Int, Int, Int, Int, Int, Int)

nums :: TupInt10
nums = (0, 1, 2, 3, 4, 5, 6, 7, 8, 9)


foo :: TupInt10
foo = SwzS.ywv (100, 200, 300) nums -- (0, 100, 2, 200, 300, 5, 6, 7, 8)

foo2, foo3, foo4 :: TupInt10
foo2 = SwzS.z 123 nums -- (0, 1, 123, 3, 4, 5, 6, 7, 8, 9)
foo3 = SwzS.u 321 nums -- (0, 1, 2, 3, 4, 321, 6, 7, 8, 9)
foo4 = SwzS.q 333 nums -- (0, 1, 2, 3, 4, 5, 6, 7, 8, 333)

swizzleSet "" "zvusq"

bar :: TupInt10
bar = zvusq (100, 200, 300, 400, 500) nums -- (0, 1, 100, 3, 200, 300, 6, 400, 8, 500)
```

```haskell
import GHC.Generics
import Data.Tuple
import Data.SwizzleSet qualified as SwzS
import Data.SwizzleSet.Class

newtype Red = Red Double deriving Show
newtype Green = Green Double deriving Show
newtype Blue = Blue Double deriving Show
newtype Alpha = Alpha Double deriving Show

data Argb = Argb Alpha Red Green Blue deriving (Show, Generic)

instance SwizzleSet1 Argb where type X Argb = Alpha
instance SwizzleSet2 Argb where type Y Argb = Red
instance SwizzleSet3 Argb where type Z Argb = Green
instance SwizzleSet4 Argb where type W Argb = Blue

sample :: Argb
sample = Argb (Alpha 0.5) (Red 0.9) (Green 0.3) (Blue 0.2)

lessRed :: Argb
lessRed = SwzS.y (Red 0.4) sample -- Argb (Alpha 0.5) (Red 0.4) (Green 0.3) (Blue 0.2)

lessAlphaGreen :: Argb
lessAlphaGreen = SwzS.xz (Alpha 0.2, Green 0.1) sample
	-- Argb (Alpha 0.2) (Red 0.4) (Green 0.1) (Bluye 0.2)
```

```haskell
import Data.SwizzleSet.TH

swizzleSet "set" "ywuq"

sample :: (Int, Int, Int, Int, Int, Int, Int, Int, Int, Int)
sample = setYwuq (100, 200, 300, 400) (0, 1, 2, 3, 4, 5, 6, 7, 8, 9)
```
