```sml
type student_id = int
type grade = int (* must be in 0 to 100 range *)
type final_grade = { id : student_id, grade : grade option }
datatype pass_fail = pass | fail
```

1. Write a function`pass_or_fail` of type `{grade : int option, id : 'a} -> pass_fail` that takes a `final_grade` (or, as the type indicates, a more general type) and returns `pass` if the grade field contains`SOME i`for an `i*≥75` (else `fail`).

```sml
fun pass_or_fail {grade= x, id= _} =
        case x of
            SOME(i) => if i >= 75 then pass else fail
        | NONE => fail

val t = pass_or_fail {id= 123,grade=SOME(10)};
val t = pass_or_fail {id= 123,grade=SOME(80)};
val t = pass_or_fail {id= 123,grade=NONE};
```

2. Using `pass_or_fail` as a helper function, write a function`has_passed` of type `{grade : int option, id : 'a} -> bool`that returns`true` if and only if the the grade field contains `SOME i` for an `i≥75`.

```sml
fun has_passed(x: final_grade) =
        if pass_or_fail x = pass then true else false 
```

3. Using `has_passed` as a helper function, write a function`number_passed` that takes a list of type `final_grade` (or a more general type) and returns how many list elements have passing (again, ≥75) grades.

```sml
fun number_passed x = 
        case x of
            [] => 0
        |x::xs' => (if has_passed(x) = true then 1 else 0) + number_passed(xs')

val t = number_passed [{id= 123,grade=SOME(10)},{id= 123,grade=SOME(80)}];
```

4. Write a function`number_misgraded` of type `(pass_fail * final_grade) list -> int` that indicates how many list elements are "mislabeled" where mislabeling means a pair`(pass,x)` where`has_passed x` is `false` or `(fail,x)` where `has_passed x` is`true`.

```sml
fun number_misgraded x = 
        case x of
            [] => 0
        | (judge,f)::xs' => if judge = has_passed(f) then 0 else 1 + number_misgraded(xs')

val t = number_misgraded([(true,{id= 123,grade=SOME(10)})])
```



```sml
datatype 'a tree = leaf 
                 | node of { value : 'a, left : 'a tree, right : 'a tree }
datatype flag = leave_me_alone | prune_me
```

5. Write a function `tree_height` that accepts an `'a tree` and evaluates to a height of this tree. The height of a tree is the length of the longest path to a leaf. Thus the height of a leaf is `0`.

```sml
fun tree_height x = 
        case x of
            leaf => 0
        |node{value=_,left=a,right=b} => 1 + Int.max(tree_height a, tree_height b)

    
val a = tree_height(node{value=1,left=node{value=2,left=node{value=3,left=leaf,right=leaf},right=leaf},right=leaf})
```

6. Write a function `sum_tree` that takes an `int tree` and evaluates to the sum of all values in the nodes.

```sml
fun sum_tree x =
        case x of
            leaf => 0
        |node{value=v,left=l,right=r} => v + sum_tree(l) + sum_tree(r)
    
val a = sum_tree(node{value=1,left=node{value=2,left=node{value=3,left=leaf,right=leaf},right=leaf},right=leaf})
```

7. Write a function`gardener` of type `flag tree -> flag tree` such that its structure is identical to the original tree except all nodes of the input containing `prune_me` are (along with all their descendants) replaced with a leaf.

```sml
fun gardener x =
    case t of
        node {value=leave_me_alone, left=l, right=r} => node{value=leave_me_alone, left = gardener l,right= gardener r}
        | _ => leaf
```

8. Re-implement various functions provided in the SML standard libraries for lists and options.  See http://sml-family.org/Basis/list.html and http://sml-family.org/Basis/option.html.  Good examples include `last`, `take`,`drop`, `concat`, `getOpt`, and `join`.

```sml
fun last x = 
        case x of
            [] => raise List.Empty
        | a::[] => a
        | a::as' => last as'

val a = last [1,2,3,4];

fun take(l,i) =
        case i of
            0 => []
        | _ => hd l::take(tl l,i-1)

val a = take([1,2,3],1);

fun drop(l,i) =
        case i of
            0 => l
        | _ => drop(tl l, i-1)

val a = drop([1,2,3],1);

fun concat l =
        case l of 
            [] => []
        | x::xs' => x @ concat(xs');

val a = concat [[1,2],[3,4]];

fun getOpt x =
        case x of 
            (SOME(v), _) => v
        | (_, a) => a

join..... 我愣是没看明白它的函数签名
val join : 'a option option -> 'a option
```

Problems 9-16 use this type definition for natural numbers:

```sml
datatype nat = ZERO | SUCC of nat
```

A "natural" number is either zero, or the "successor" of a another integer.    So for example the number 1 is just `SUCC ZERO`, the number 2 is `SUCC (SUCC ZERO)`, and so on.

9. Write`is_positive : nat -> bool`, which given a "natural number" returns whether that number is positive (i.e. not zero).

```sml
fun is_positive x = 
        case x of
            ZERO => false
        | _ => true

val a = is_positive(SUCC ZERO);
val a = is_positive(ZERO);
```

10.  Write`pred : nat -> nat`, which given a "natural number" returns its predecessor. Since 0 does not have a predecessor in the natural numbers, throw an exception `Negative` (will need to define it first).

```sml
exception Negative
fun pred n =
    case n of
    ZERO => raise Negative
    | SUCC p' => p'
```

11. Write `nat_to_int : nat -> int`, which given a "natural number" returns the corresponding `int`. For example, `nat_to_int (SUCC (SUCC ZERO)) = 2`.  (Do not use this function for problems 13-16 -- it makes them too easy.)

```sml
fun nat_to_int x =
        case x of
            ZERO => 0
        | SUCC p' => 1 + nat_to_int p'

val a= nat_to_int (SUCC (SUCC (SUCC ZERO)))
```

12.  Write `int_to_nat : int -> nat` which given an integer returns a "natural number" representation for it, or throws a `Negative` exception if the integer was negative.  (Again, do not use this function in the next few problems.)

```sml
fun int_to_nat x =
        if x <0 then raise Negative else
            if x = 0 then ZERO else SUCC (int_to_nat(x-1))

val a = int_to_nat 10;
val b = nat_to_int a;
```

13. Write `add : nat * nat -> nat` to perform addition.

```sml
fun add x =
        case x of
            (a,ZERO) => a
        |(b, SUCC b') => SUCC(add(b, b'))

val a = add((SUCC ZERO),(SUCC ZERO));
```

14. Write `sub : nat * nat -> nat` to perform subtraction.  (Hint: Use`pred`.)

```sml
fun sub x =
        case x of
            (a,ZERO) => a
        |(ZERO, _) => raise Negative
        |(SUCC p',_) => p'

val a = sub((SUCC (SUCC ZERO)),(SUCC ZERO));
```

15. Write`mult : nat * nat -> nat` to perform multiplication. (Hint: Use `add`.)

```sml
fun mult x =
        case x of
            (a,ZERO) => ZERO
        |(p,SUCC p') => add(p, mult(p,p'))

val a = mult((SUCC (SUCC ZERO)),(SUCC (SUCC (SUCC ZERO))))
```

16. Write `less_than : nat * nat -> bool` to return `true` when the first argument is less than the second.

```sml
fun less_than(first,last) =
        case (first,last) of
            (_,ZERO) => false
        |(ZERO,_) => true
        |(SUCC a', SUCC b') => less_than(a', b')
val a = less_than((SUCC (SUCC ZERO)),(SUCC ZERO));
```



The remaining problems use this datatype, which represents sets of integers:

```sml
datatype intSet = 
  Elems of int list (*list of integers, possibly with duplicates to be ignored*)
| Range of { from : int, to : int }  (* integers from one number to another *)
| Union of intSet * intSet (* union of the two sets *)
| Intersection of intSet * intSet (* intersection of the two sets *)
                
```

17. Write `isEmpty : intSet -> bool` that determines if the set is empty or not.



18. Write `contains: intSet * int -> bool`that returns whether the set contains a certain element or not.

```sml
fun contains(intset, input) =
        case intset of
            Elems ls=> let
                    fun listcontain(l)=
                            case l of
                                [] => false
                            |p::p' => p = input orelse listcontain(p')
                in
                    listcontain(ls)
                end
        | Range{from=a,to=b} => input >= a andalso  input <= b
        | Union(a,b) => contains(a, input) orelse contains(b, input)
        | Intersection(a,b) => contains(a, input) andalso contains(b, input)

val a = contains(Elems([1,2,3]), 3);
```

19. Write`toList : intSet -> int list` that returns a list with the set's elements, without duplicates.

```sml
难度有点高，暂未理解
```

