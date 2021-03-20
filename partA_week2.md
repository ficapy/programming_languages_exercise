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

