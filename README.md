Download Link: https://assignmentchef.com/product/solved-cs320-homework-8-corel
<br>



The goal of Homework #9 is to implement the interpreter of COREL, which is the CORELanguage with types. Implement its type checker and the interpreter:

&lt;COREL&gt; ::= &lt;num&gt;

| true

| false

| {+ &lt;COREL&gt; &lt;COREL&gt;}

| {- &lt;COREL&gt; &lt;COREL&gt;}

| {= &lt;COREL&gt; &lt;COREL&gt;}

| {with {&lt;id&gt; : &lt;Type&gt; &lt;COREL&gt;} &lt;COREL&gt;}

| &lt;id&gt;

| {fun {&lt;id&gt; : &lt;Type&gt;} &lt;COREL&gt;}

| {&lt;COREL&gt; &lt;COREL&gt;}

| {if &lt;COREL&gt; &lt;COREL&gt; &lt;COREL&gt;}

| {recfun {&lt;id&gt; : &lt;Type&gt; &lt;id&gt; : &lt;Type&gt;} &lt;COREL&gt;}

| {withtype {&lt;id&gt; {&lt;id&gt; &lt;Type&gt;}+} &lt;COREL&gt;}

| {cases &lt;id&gt; &lt;COREL&gt; {&lt;id&gt; {&lt;id&gt;} &lt;COREL&gt;}+}

&lt;Type&gt; ::= num

| bool

| {&lt;Type&gt; -&gt; &lt;Type&gt;}

| &lt;id&gt;

It provides number literals with three binary operations; addition, subtraction, and equality. The equality operation is only for numbers thus it should throw an error when either value of left or right hand side is not number. The with expressions, identifiers, fun expressions, and function applications have semantics that you learned in the lecture:

test(run(“42”), “42”) test(run(“true”), “true”) test(run(“{+ 1 2}”), “3”) test(run(“{- 2 1}”), “1”) test(run(“{= 1 0}”), “false”) testExc(run(“{= true 0}”), “no type”) test(run(“{with {x : num 1} x}”), “1”) test(run(“{{fun {x : num} {+ x 1}} 2}”), “3”)

The value of the first expression of if-then-else expression should be evaluated into the boolean value and if it is true, evaluate the second expression, otherwise, evaluate the third one. The recfun form denotes recursive function:

<table width="673">

 <tbody>

  <tr>

   <td width="673">test(run(“””{{recfun {f: {num -&gt; num} x: num}{if {= x 0} 0 {+ {f {- x 1}} x}}}10}”””), “55”) testExc(run(“{if 1 2 3}”), “no type”)</td>

  </tr>

 </tbody>

</table>

1

In this homework, the withtype and cases are more general version than in the lecture. It accepts one or more constructors in withtype thus cases also have one or more cases. With such generality, you should care about the mismatch between withtype and cases. If cases does not exactly cover the all the cases of the corresponding withtype, typeCheck should throw an error with a message containing “not all cases”:

<table width="673">

 <tbody>

  <tr>

   <td width="673">test(run(“””{withtype{fruit {apple num}{banana num}}{cases fruit {apple 1}{apple {x} x}{banana {y} y}}}”””), “1”)testExc(run(“””{withtype{fruit {apple num}{banana num}}{cases fruit {apple 1}{apple {x} x}}}”””), “not all cases”)</td>

  </tr>

 </tbody>

</table>

2