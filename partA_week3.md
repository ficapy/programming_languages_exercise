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

