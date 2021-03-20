### [https://www.coursera.org/learn/programming-languages/home/week/2](https://www.coursera.org/learn/programming-languages/home/week/2)

1. Write a function ` alternate: int list -> int`  that takes a list of numbers and adds them with alternating sign. For example `alternate [1,2,3,4] = 1 - 2 + 3 - 4 = -2`

1-2+3-4 可以看作 1 + (-2) + 3 + (-4)

```sml
fun alternate(input :int list) = 
        let
            fun inner(input: int list, plus: bool) =
                    if null input
                    then 0
                    else if plus
                        then hd input + inner(tl input, not plus)
                        else (0 - hd input) + inner(tl input, not plus) (* 或者 else inner(tl input, not plus) - hd input *)
        in
            inner(input, true)
        end

val a = alternate([]);
val a = alternate([1,2]);
val a = alternate([1,2,3]);
val a = alternate([1,2,3,4]);
val a = alternate([1,2,3,4,5]);
```

2. Write a function `min_max : int list -> int * int` int that takes a non-empty list of numbers, and returns a pair `(min, max)`of the minimum and maximum of the numbers in the list.

写查找最小，查找最大，然后组合

```sml
fun find_max(l: int list) = 
        if null (tl l)
        then hd l
        else let
                val remain = find_max(tl l)
            in
                if hd l > remain
                then hd l
                else remain
            end

val a = find_max([~3,~2,~1]);
val a = find_max([~3]);
val a = find_max([~1,1,4]);

fun find_min(l: int list) = 
        if null (tl l)
        then hd l
        else let
                val remain = find_min(tl l)
            in
                if hd l > remain
                then remain
                else hd l
            end

val a = find_min([~3,~2,~1]);
val a = find_min([~3]);
val a = find_min([~1,1,4]);

fun min_max(l: int list) = 
        (find_min(l), find_max(l))

val a = min_max([~1,1,4]);
```

3. Write a function `cumsum : int list -> int list` that takes a list of numbers and returns a list of the partial sums of those numbers. For example `cumsum [1,4,20] = [1,5,25]`

```sml
fun cumsum(l: int list) = 
        let
            fun inner(l, acc) = 
                    if null l
                    then []
                    else (acc + hd l)::inner(tl l, acc + hd l)
        in
            inner(l, 0)
        end

val a = cumsum([1,4,20]);
```

4. Write a function `greeting : string option -> string` that given a string  option `option SOME` name returns the string `"Hello three, ...!"` where the dots would be replaced by *name*. Note that the name is given as an option, so if it is `NONE` then replace the dots with `"you"`

```sml
fun greeting(name: string option) = 
        if isSome name
        then "Hello there, "  ^ valOf name
        else "Hello there, you"

val a = greeting(NONE);
val a = greeting(SOME("Ficapy"));
```

5. Write a function `repeat: int list * int list -> int list`  that given a list of integers and another list of nonnegative integers, repeats the integers in the first list according to the numbers indicated by the second list. For example `repeat ([1,2,3], [4,0,3]) = [1,1,1,1,3,3,3]`

```sml
fun mul(i:int,n:int) = 
    (* 处理最简单的单个情况 *)
        if n = 0
        then []
        else i::mul(i, n - 1)

fun add_list(a:int list,b:int list) = 
    (* 把两个列表加在一起 *)
        if null a
        then b
        else hd a::add_list(tl a, b)

val a = mul(1, 10);

fun repeat(i: int list, n: int list) = 
    (* 组合 *)
        if null i
        then []
        else add_list(mul(hd i,hd n), repeat(tl i,tl n))

val a = repeat([1,2,3], [4,0,3]);
```

6. Write a function `addOpt : int option * int option -> int option` that given two "optional" integers, adds them if they are both present (returning `SOME` of their sum), or returns `NONE` if at least one of the two arguments is `NONE`

```sml
fun addOpt(a:int option,b: int option) =
        if isSome a andalso isSome b
        then SOME(valOf a + valOf b)
        else NONE

val a = addOpt(SOME(1),NONE)
val a = addOpt(NONE,SOME(1))
val a = addOpt(SOME(1),SOME(1))
```

7. Write a function `addALLOpt : int option list -> int option` that given a list of "optional" integers, adds those integers that are there (i.e. adds all the `SOME i`) . For example: `addAllOpt ([SOME 1, NONE, SOME 3])= SOME 4.` If the list does not contain any `SOME` is in it, i.e. they are all `NONE` or the list is empty, the function should return `NONE`.

```
fun addAllOpt(l: int option list) = 
        if null l
        then NONE
        else
            let 
                fun addOpt(a:int option,b: int option) =
                        let
                            val ra = if isSome a then valOf a else 0
                            val rb = if isSome b then valOf b else 0
                        in
                            if isSome a orelse isSome b
                            then SOME(ra + rb)
                            else NONE
                        end

            in
                addOpt(hd l, addAllOpt(tl l))
            end

val a = addAllOpt([SOME(1), NONE, SOME(3)])
val b = addAllOpt([NONE, NONE, NONE])
val c = addAllOpt([])
```

8. Write a function `any : bool list -> bool` that given a list of booleans returns `true` if there is at least one of them that is `true`, otherwise returns \verb|false|false. (If the list is empty it should return`false` because there is no`true`.)

```sml
fun any(l: bool list) =
        if null l
        then false
        else if null (tl l)
            then hd l
            else hd l orelse any(tl l)

val a = any([true, false]);
val a = any([]);
val a = any([true]);
val a = any([false]);
```

9. Write a function `all:bool list -> bool` that given a list of booleans returns `true` if all of them `true`, otherwise returns  `false`. (If the list is empty it should return `true` because there is no `false`)

```sml
fun all(l: bool list) =
        if null l
        then true
        else if null (tl l)
            then hd l
            else hd l andalso all(tl l)

val a = all([true, false]);
val a = all([]);
val a = all([true]);
val a = all([false]);
```

10. Write a function `zip : int list * int list -> int * int list` that given two lists of integers creates consecutive pairs, and stops when one of the lists is empty. For example:`zip ([1,2,3], [4, 6]) = [(1,4), (2,6)]`.

```sml
fun zip(a: int list,b: int list) = 
        if null a orelse null b
        then []
        else (hd a,hd b)::zip(tl a,tl b)

val a = zip([1,2,3], [4,6])
```

