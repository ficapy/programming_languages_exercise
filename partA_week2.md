### [https://www.coursera.org/learn/programming-languages/home/week/2](https://www.coursera.org/learn/programming-languages/home/week/2)

1. Write a function ` alternate: int list -> int`  that takes a list of numbers and adds them with alternating sign. For example `alternate [1,2,3,4] = 1 - 2 + 3 - 4 = -2`

1-2+3-4 可以看作 1 + (-2) + 3 + (-4)

```sml
fun alternate(input :int list) = 
        let
            fun inner(input: int list, plus: bool) =
                    if null input then 0 else 
                        if plus then hd input + inner(tl input, not plus) else 
                            (0 - hd input) + inner(tl input, not plus) (* 或者 else inner(tl input, not plus) - hd input *)
        in
            inner(input, true)
        end

val a = alternate([]);
val a = alternate([1,2]);
val a = alternate([1,2,3]);
val a = alternate([1,2,3,4]);
val a = alternate([1,2,3,4,5]);
```

更简单的写法

1-2+3-4 = 1-(2-3+4) = 1-(2-(3-4))

```sml
fun alternate(nums: int list) = 
        if null nums then 0 else
            hd nums - alternate(tl nums)
```



2. Write a function `min_max : int list -> int * int` int that takes a non-empty list of numbers, and returns a pair `(min, max)`of the minimum and maximum of the numbers in the list.

写查找最小，查找最大，然后组合

```sml
fun find_max(l: int list) = 
        if null (tl l) then hd l else 
            let val remain = find_max(tl l)
            in if hd l > remain then hd l else remain
            end

val a = find_max([~3,~2,~1]);
val a = find_max([~3]);
val a = find_max([~1,1,4]);

fun find_min(l: int list) = 
        if null (tl l) then hd l else 
            let val remain = find_min(tl l)
            in if hd l > remain then remain else hd l
            end

val a = find_min([~3,~2,~1]);
val a = find_min([~3]);
val a = find_min([~1,1,4]);

fun min_max(l: int list) = 
        (find_min(l), find_max(l))

val a = min_max([~1,1,4]);
```

```sml
fun min_max(l:int list) = 
        if null l then (0,0) else
            if null (tl l) then (hd l, hd l) else
                let
                    val temp = hd l
                    val result = min_max(tl l)
                    val min = #1 result
                    val max = #2 result
                in
                    (if min<temp then min else temp, if max > temp then max else temp)
                end

val a  = min_max([1, 5, 2, 7, 9, ~1])
```

```python
def min_max(l):
    if not l:
        return (0, 0)
    if len(l) == 1:
        return (l[0], l[0])
    end = min_max(l[1:])
    min, max = end
    return min if min < l[0] else l[0], max if max > l[0] else l[0]


print(min_max([1, 5, 2, 7, 9, -1]))
```



3. Write a function `cumsum : int list -> int list` that takes a list of numbers and returns a list of the partial sums of those numbers. For example `cumsum [1,4,20] = [1,5,25]`

这个例子表明了如果不添加附加参数，递归只擅长从后往前推，不擅长从前往后推，必须使用额外变量记录，每次从前往后推的时候变化的变量

```sml
fun cumsum(l: int list) = 
        let fun inner(l, acc) = 
                    if null l then [] else (acc + hd l)::inner(tl l, acc + hd l)
        in inner(l, 0)
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
        if n = 0 then [] else i::mul(i, n - 1)

fun add_list(a:int list,b:int list) = 
        (* 把两个列表加在一起 *)
        if null a then b else hd a::add_list(tl a, b)

val a = mul(1, 10);

fun repeat(i: int list, n: int list) = 
        (* 组合 *)
        if null i then [] else add_list(mul(hd i,hd n), repeat(tl i,tl n))

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

```sml
fun addAllOpt(l: int option list) = 
        if null l then NONE else
            let 
                fun addOpt(a:int option,b: int option) =
                        let
                            val ra = if isSome a then valOf a else 0
                            val rb = if isSome b then valOf b else 0
                        in
                            if isSome a orelse isSome b then SOME(ra + rb) else NONE
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
        if null l then false else 
        		hd l orelse any(tl l)

val a = any([true, false]);
val a = any([]);
val a = any([true]);
val a = any([false]);
```

9. Write a function `all:bool list -> bool` that given a list of booleans returns `true` if all of them `true`, otherwise returns  `false`. (If the list is empty it should return `true` because there is no `false`)

```sml
fun all(l: bool list) =
        if null l then true else 
        		hd l andalso all(tl l)

val a = all([true, false]);
val a = all([]);
val a = all([true]);
val a = all([false]);
```

10. Write a function `zip : int list * int list -> int * int list` that given two lists of integers creates consecutive pairs, and stops when one of the lists is empty. For example:`zip ([1,2,3], [4, 6]) = [(1,4), (2,6)]`.

```sml
fun zip(a: int list,b: int list) = 
        if null a orelse null b then [] else 
            (hd a,hd b)::zip(tl a,tl b)

val a = zip([1,2,3], [4,6])
```

11. Write a version`zipRecycle` of `zip`, where when one list is empty it starts recycling from its start until the other list completes. For example:`zipRecycle ([1,2,3], [1, 2, 3, 4, 5, 6, 7]) =[(1,1), (2,2), (3, 3), (1,4), (2,5), (3,6), (1,7)]`

```sml
fun zipRecycle(a: int list,b: int list) = 
        let fun inner(a:int list,b: int list,raw_a:int list, raw_b:int list, adone:bool, bdone:bool) = 
                    if adone andalso bdone then [] else 
                        if null a then (hd raw_a, hd b)::inner(tl raw_a,tl b, raw_a, raw_b, true, bdone orelse (null (tl b))) else 
                            if null b then (hd a, hd raw_b)::inner(tl a,tl raw_b, raw_a, raw_b, adone orelse (null (tl a)), true) else 
                                (hd a, hd b)::inner(tl a,tl b, raw_a, raw_b, adone orelse (null (tl a)), bdone orelse (null (tl b)))
        in
            inner(a,b,a,b,false,false)
        end

val a = zipRecycle([1], [1,2,3]);
val a = zipRecycle([1,2,3], [1,2,3,4,5,6,7]);
```

```python
def zipRecycle(a, b):
    def inner(a, b, raw_a, raw_b, adone, bdone):
        if adone and bdone:
            return []
        else:
            if not a:
                return [(raw_a[0], b[0])] + inner(raw_a[1:], b[1:], raw_a, raw_b, True, bdone or len(b[1:]) == 0)
            else:
                if not b:
                    return [(a[0], raw_b[0])] + inner(a[1:], raw_b[1:], raw_a, raw_b, adone or len(a[1:]) == 0, True)
                else:
                    return [(a[0], b[0])] + inner(a[1:], b[1:], raw_a, raw_b, adone or len(a[1:]) == 0,
                                                  bdone or len(b[1:]) == 0)

    return inner(a, b, a, b, False, False)


print(zipRecycle([1, ], [1, 2, 3, 4, 5, 6, 7]))
print(zipRecycle([1], [1]))
print(zipRecycle([1, 2], [1]))
```

12. Write a version `zipOpt` of `zip` with return type `(int * int) list option`. This version should return `SOME`  of a list when the original lists have the same length, and `NONE` if they do not.

先写出some 和 none的加法出来，再组合

```sml
fun someadd(a:(int*int) option,b:(int*int) list option) = 
        if isSome a andalso isSome b then 
            let val a = valOf a
                val b = valOf b
            in
                SOME(a::b)
            end
        else NONE

val a = someadd(SOME((1,2)), SOME([(3,4)]));
val a = someadd(SOME((1,2)), NONE);

fun zipOpt(a:int list,b:int list) = 
        if null a andalso null b then SOME([]) else 
            if null a andalso not (null b) then NONE else 
                if null b andalso not (null a) then NONE else 
                    someadd(SOME((hd a,hd b)), zipOpt(tl a, tl b))

val a = zipOpt([1,2,3],[4,5,6]);
val a = zipOpt([1,2,3],[4]);
val a = zipOpt([1,2,3],[]);
```

13. Write a function `lookup : (string * int) list * string -> int option` that takes a list of pairs `(s, i)` and also a string `s2` to look up. It then goes through the list of pairs looking for the string`s2` in the first component. If it finds a match with corresponding number `i`, then it returns`SOME i`. If it does not, it returns `NONE`.

```sml
fun lookup(l: (string*int) list, target: string) = 
        if null l
        then NONE
        else if (#1 (hd l)) = target
            then SOME(#2 (hd l))
            else lookup(tl l, target)

val a = lookup([],"123");
val a = lookup([("1",1),("12",2),("123",3)],"123");
```

14. Write a function `splitup : int list -> int list * int list` that given a list of integers creates two lists of integers, one containing the non-negative entries, the other containing the negative entries. Relative order must be preserved: All non-negative entries must appear in the same order in which they were on the original list, and similarly for the negative entries.

```sml
fun splitup(a: int list) = 
        let
            fun add(current:(int list)*(int list),position:int,v:int) = 
                    if position = 0
                    then (v::(#1 current),#2 current)
                    else (#1 current, v::(#2 current))
            
            fun inner(a:int list,non: int list,ne: int list) = 
                    if null a
                    then ([],[])
                    else if hd a > 0
                        then add(inner(tl a, non, ne), 0, hd a )
                        else add(inner(tl a, non, ne), 1, hd a )
        in
            inner(a,[],[])
        end

val a = splitup([~3,~1,1,3]);
```

15. Write a version `splitAt : int list * int -> int list * int list` of the previous function that takes an extra "threshold" parameter, and uses that instead of 0 as the separating point for the two resulting lists.

```sml
fun splitAt(a: int list, target: int) = 
        let
            fun add(current:(int list)*(int list),v:int) = 
                    (v::(#1 current),#2 current)
            
            fun inner(a:int list,p1:int list,index:int) = 
                    if index > target orelse null a
                    then (p1,a)
                    else add(inner(tl a, p1, index + 1), hd a )
        in
            inner(a,[],1)
        end

val a = splitAt([~3,~1,1,3], 0);
val a = splitAt([~3,~1,1,3], 1);
val a = splitAt([~3,~1,1,3], 2);
val a = splitAt([~3,~1,1,3], 3);
val a = splitAt([~3,~1,1,3], 4);
val a = splitAt([~3,~1,1,3], 5);
```

上面的写法理解错题意了

```sml
fun splitAt1(a: int list, target: int) = 
        let
            fun add(current:(int list)*(int list),position:int,v:int) = 
                    if position = 0
                    then (v::(#1 current),#2 current)
                    else (#1 current, v::(#2 current))
            
            fun inner(a:int list,non: int list,ne: int list) = 
                    if null a
                    then ([],[])
                    else if hd a > target
                        then add(inner(tl a, non, ne), 0, hd a )
                        else add(inner(tl a, non, ne), 1, hd a )
        in
            inner(a,[],[])
        end
```

这里也有很明显的允许从后往前处理的特征，因此有更简单的写法

```sml
fun splitAt(l:int list, target:int) = 
        if null l then ([],[]) else
            let
                val result = splitAt(tl l, target)
            in
                if hd l > target 
                then ((hd l)::(#1 result), #2 result)
                else (#1 result, (hd l)::(#2 result))
            end
```

```python
def splitAt(l, target):
    if not l:
        return ([], [])
    result = splitAt(l[1:], target)
    large, small = result
    if l[0] > target:
        return (large + [l[0]], small)
    else:
        return (large, small + [l[0]])

print(splitAt([3, 1, -3, 4,-1], 1))
```

16. Write a function`isSorted : int list -> boolean` that given a list of integers determines whether the list is sorted in increasing order.

```sml
fun isSorted(l: int list) = 
        (* 简单设置它最少有两个元素 *)
        let
            fun inner(l:int list,a:int list,b:int list) = 
                    if null b
                    then true
                    else if hd a > hd b
                        then false
                        else inner(tl l,tl l,tl (tl l))
        in
            inner(l, l, tl l)
        end

val a = isSorted([1,2]);
val a = isSorted([2,1,3]);
```

17. Write a function `isAnySorted : int list -> boolean`, that given a list of integers determines whether the list is sorted in either increasing or decreasing order.

```sml
fun isSorted(l: int list) = 
        (* 简单设置它最少有两个元素 *)
        let
            fun inner(l:int list,a:int list,b:int list, tag: bool) = 
                    if null b
                    then true
                    else if not ((hd a > hd b) = tag)
                        then false
                        else inner(tl l,tl l,tl (tl l), tag)
        in
            inner(l, l, tl l, hd l > hd (tl l))
        end

val a = isSorted([1,2]);
val a = isSorted([3,2,1]);
val a = isSorted([2,1,3]);
```

18. Write a function `sortedMerge : int list * int list -> int list`that takes two lists of integers that are each sorted from smallest to largest, and merges them into one sorted list. For example:`sortedMerge ([1,4,7], [5,8,9]) = [1,4,5,7,8,9]`

```sml
fun sortedMerge(aa:int list*int list) = 
        let
            val a = (#1 aa)
            val b = (#2 aa)
        in
            if null a andalso null b
            then []
            else if null a
                then (hd b)::sortedMerge((a, (tl b)))
                else if null b
                    then (hd a)::sortedMerge(((tl a),b))
                    else if hd a < hd b
                        then (hd a)::sortedMerge(((tl a),b))
                        else (hd b)::sortedMerge((a,(tl b)))
        end

val a = sortedMerge(([1,4,7],[5,8,9]));
```

19. Write a sorting function `qsort : int list -> int list` that works as follows: Takes the first element out, and uses it as the "threshold" for `splitAt`. It then recursively sorts the two lists produced by `splitAt`. Finally it brings the two lists together. (Don't forget that element you took out, it needs to get back in at some point). You could use `sortedMerge` for the "bring together" part, but you do not need to as all the numbers in one list are less than all the numbers in the other.)

```sml
fun splitAt(a: int list, target: int) = 
        let
            fun add(current:(int list)*(int list),position:int,v:int) = 
                    if position = 0
                    then (v::(#1 current),#2 current)
                    else (#1 current, v::(#2 current))
            
            fun inner(a:int list,non: int list,ne: int list) = 
                    if null a
                    then ([],[])
                    else if hd a < target
                        then add(inner(tl a, non, ne), 0, hd a )
                        else add(inner(tl a, non, ne), 1, hd a )
        in
            inner(a,[],[])
        end


fun sortedMerge(aa:int list*int list) = 
        let
            val a = (#1 aa)
            val b = (#2 aa)
        in
            if null a andalso null b
            then []
            else if null a
                then (hd b)::sortedMerge((a, (tl b)))
                else if null b
                    then (hd a)::sortedMerge(((tl a),b))
                    else if hd a < hd b
                        then (hd a)::sortedMerge(((tl a),b))
                        else (hd b)::sortedMerge((a,(tl b)))
        end


fun qsort(a:int list) = 
        if null a
        then []
        else 
            let val pivot = hd a
                val full = splitAt(a, pivot)
                val left = #1 full
                val right = #2 full
                val sort_left = qsort(left)
                val sort_right = qsort(tl right)
            in
                (* 注意，splitAt需要把小的放在前面 *)
                sortedMerge(sort_left, pivot::sort_right)
            end

val a = qsort([]);
val a = qsort([2,1,5,6,9,3]);
val a = qsort([2,3,2,4,4,3])
```

20. Write a function`divide : int list -> int list * int list`that takes a list of integers and produces two lists by alternating elements between the two lists.  For example:`divide ([1,2,3,4,5,6,7]) = ([1,3,5,7], [2,4,6])`.

```sml
(* 这个和上面的SplitAt十分相似 *)
fun divide(a: int list) = 
        let
            fun add(current:(int list)*(int list),position:int,v:int) = 
                    if position = 0
                    then (v::(#1 current),#2 current)
                    else (#1 current, v::(#2 current))
            
            fun inner(a:int list,pre: int list,post: int list, tag:bool) = 
                    if null a
                    then ([],[])
                    else if tag
                        then add(inner(tl a, pre, post, not tag), 0, hd a )
                        else add(inner(tl a, pre, post, not tag), 1, hd a )
        in
            inner(a,[],[],true)
        end

val a = divide([1,2,3,4,5]);
```

21. Write another sorting function `not_so_quick_sort : int list -> int list` that works as follows: Given the initial list of integers, splits it in two lists using divide, then recursively sorts those two lists, then merges them together with `sortedMerge`. 

上面是快排，这里是归并

```sml
fun divide(a: int list) = 
        let
            fun add(current:(int list)*(int list),position:int,v:int) = 
                    if position = 0
                    then (v::(#1 current),#2 current)
                    else (#1 current, v::(#2 current))
            
            fun inner(a:int list,pre: int list,post: int list, tag:bool) = 
                    if null a
                    then ([],[])
                    else if tag
                        then add(inner(tl a, pre, post, not tag), 0, hd a )
                        else add(inner(tl a, pre, post, not tag), 1, hd a )
        in
            inner(a,[],[],true)
        end


fun sortedMerge(aa:int list*int list) = 
        let
            val a = (#1 aa)
            val b = (#2 aa)
        in
            if null a andalso null b
            then []
            else if null a
                then (hd b)::sortedMerge((a, (tl b)))
                else if null b
                    then (hd a)::sortedMerge(((tl a),b))
                    else if hd a < hd b
                        then (hd a)::sortedMerge(((tl a),b))
                        else (hd b)::sortedMerge((a,(tl b)))
        end


fun qsort(a:int list) = 
        (* 如果只有一个元素的时候返回自身 *)
        if null a orelse (not (null a) andalso null (tl a))
        then a
        else  let
                val full = divide(a)
                val left = #1 full
                val right = #2 full
                val sort_left = qsort(left)
                val sort_right = qsort(right)
            in
                sortedMerge(sort_left, sort_right)
            end

val a = qsort([]);
val a = qsort([1]);
val a = qsort([2,1,5,6,9,3]);
val a = qsort([2,3,2,4,4,3])
```

22. Write a function `fullDivide : int * int -> int * int` that given two numbers `k` and `n` it attempts to evenly divide`k` into `n` as many times as possible, and returns a pair `(d, n2)` where`d` is the number of times while `n2` is the resulting `n` after all those divisions. Examples: `fullDivide (2, 40) = (3, 5)` because`2*2*2*5 = 40` and`fullDivide((3,10)) = (0, 10) ` because `3` does not divide `10`.

```sml
fun fullDivide(n:int, l: int) = 
        let
            fun inner(l:int,count:int) = 
                    if (l mod n) <> 0
                    then (count,l)
                    else inner(l div n, count + 1)
        in
            inner(l,0)
        end

val a = fullDivide(2,40);
val a = fullDivide(3,10);
```

23. Using `fullDivide`, write a function `factorize : int -> (int * int) list` that given a number `n` returns a list of pairs `(d, k)` where`d` is a prime number dividing `n` and `k` is the number of times it fits. The pairs should be in increasing order of prime factor, and the process should stop when the divisor considered surpasses the square root of`n`. If you make sure to use the reduced number`n2` given by `fullDivide` for each next step, you should not need to test if the divisors are prime: If a number divides into`n`, it must be prime (if it had prime factors, they would have been earlier prime factors of `n` and thus reduced earlier). Examples: `factorize(20) = [(2,2), (5,1)]`; `factorize(36) = [(2,2), (3,2)]`; `factorize(1) = []`.

