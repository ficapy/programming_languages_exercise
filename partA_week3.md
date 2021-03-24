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

