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



