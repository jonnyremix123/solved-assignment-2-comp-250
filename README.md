Download Link: https://assignmentchef.com/product/solved-assignment-2-comp-250
<br>



<h1>Introduction</h1>

In your usage of Java language so far you have come across various programming constructs such as variables, operators, control flow statements etc. Similar constructs are present in many other programming languages. In this assignment, you will deal specifically with expressions and their evaluation. You will parse expressions and evaluate them using a stack.

An <a href="https://en.wikipedia.org/wiki/Expression_(computer_science)"><strong>expression</strong></a> is a programming language construct which evaluates to a single value. It is composed of different elements such as constants, variables, operators, method calls etc, put together according to the <em>syntax </em>of the language. For example (2 + 3) in Java evaluates to a single value 5 and is composed of operator + and constants 2 and 3. A similar expression in a different programming language may follow different syntax. For example in <a href="http://lisp-lang.org/">Lisp</a> the expression is written as <a href="https://en.wikipedia.org/wiki/Polish_notation">(+ 2 3)</a> .

<h1>Assignment</h1>

Your task in this assignment is to evaluate arithmetic expressions and calculate their values. The statements are written in a programming language whose syntax is described below. You will use Java to write a <a href="https://en.wikipedia.org/wiki/Parsing">parser</a> and an evaluator for expressions written in this language. For simplicity you will only deal with input expressions that are composed of positive integers and the operators that are listed below.

<table width="494">

 <tbody>

  <tr>

   <td width="60">+</td>

   <td width="264">addition</td>

   <td width="72">++</td>

   <td width="97">increment</td>

  </tr>

  <tr>

   <td width="60">–</td>

   <td width="264">subtraction</td>

   <td width="72">– –</td>

   <td width="97">decrement</td>

  </tr>

  <tr>

   <td width="60">*</td>

   <td width="264">multiplication</td>

   <td width="72">[ ]</td>

   <td width="97">absolute value</td>

  </tr>

  <tr>

   <td width="60">/</td>

   <td width="264">integer division</td>

   <td width="72"> </td>

   <td width="97"> </td>

  </tr>

 </tbody>

</table>

You may assume that values of expressions are of type int in Java and are within the range specified by that type. Since the subtraction operator is included in the list below, the value of an expression can be either positive or negative.

<a href="https://en.wikipedia.org/wiki/Increment_and_decrement_operators">Increment or decrement</a> operations on constants are not allowed in Java. But our language does allow it. In particular, our language has the pre-increment operation which increments values of an integers or an expression, and similarly it has the pre-decrement operation which decrements values. For example, the value of “(++5)” is “6” while expression “(– –(9))” evaluates to 8.

Note that operators +, –, *, / are <a href="https://en.wikipedia.org/wiki/Binary_operation">binary operators</a><a href="https://en.wikipedia.org/wiki/Binary_operation">,</a> i.e, they operate on two elements and produce a value, whereas the operators ++, – – and [ ] are <a href="https://en.wikipedia.org/wiki/Unary_operation">unary operators</a><a href="https://en.wikipedia.org/wiki/Unary_operation">,</a> i.e, they operate on a single element and produce a value. Consider an expression “(++(3 + 2))” in our language. The binary operator “+” operates on <em>two </em>int values 2 and 3 and produces 5. The unary operator “++” then operates on the result 5, a <em>single </em>value, and produces 6 which is the value of the expression. Similarly “((++(5 – 2)) + 3)” evaluates to 7.

The absolute value operator is denoted by [ ], instead of the | | which is generally used in arithmetic. We use the square brackets because they have a left and right versions which makes parsing easier. An expression within the [ ] operator evaluates to its absolute value. Note that no such operator exists in Java. See Table 1 for more examples of expressions.

<h2>Formal language</h2>

Your task in this assignment is to write code to parse and evaluate expressions. You can assume that your code will only be tested on valid expressions. But how can we define what a valid expression is? As you will learn if you take <a href="http://www.mcgill.ca/study/2017-2018/courses/COMP-330">COMP 330</a> and later a compilers course such as <a href="http://www.mcgill.ca/study/2017-2018/courses/COMP-520">COMP 520</a><a href="http://www.mcgill.ca/study/2017-2018/courses/COMP-520">,</a> there are <a href="https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form">formal methods</a> for defining languages. Indeed most programming languages are defined using these formal methods.

The syntactic rules for expressions and operators in our expression language can be defined as follows. A <em>digit </em>is defined:

<em>digit </em>= 0 | 1 | 2 | <em>… </em>9

The vertical bar | does not appear in expressions. Rather it is used here to mean <em>or</em>, that is, a digit can be any one of the characters ‘0’, ‘1’,.. ‘9’. Using the definition of a digit, we can then define a <em>positive integer </em>in a <em>recursive </em>manner as follows,

<em>positiveInteger </em>= <em>digit </em>| <em>digit positiveInteger</em>

so a <em>positive integer </em>is a <em>digit </em>or a digit followed by a <em>positive integer</em>. Notice that in this definition we allow positive integers to start with digit ‘0’, but you can redefine a <em>positive integer </em>to exclude this case if we wish. We can also define operators

<em>IncDecOperator </em>= + + | − − <em>binaryOperator </em>= + | − | ∗ | <em>/ </em>In a similar manner we define an <em>expression </em>as follows:

<em>expression </em>= ( <em>positiveInteger binaryOperator positiveInteger </em>) |

( <em>expression binaryOperator positiveInteger </em>) |

( <em>positiveInteger binaryOperator expression </em>) |

( <em>expression binaryOperator expression </em>) |

( <em>IncDecOperator positiveInteger </em>) |

( <em>IncDecOperator expression </em>) |

[ <em>expression </em>] | [ <em>positiveInteger </em>]

An expression is therefore one of the following:

<ul>

 <li>a positive integer or an expression followed by a binary operator followed by a positive integer or an expression, enclosed within round brackets</li>

 <li>an <em>increment or decrement operator </em>followed by a positive integer or by an expression, enclosed within round brackets.</li>

 <li>an expression or a positive integer, enclosed within square brackets</li>

</ul>

Note that a valid expression is always contained within brackets. Thus “(3+1)” is a valid expression but “3+1” is not. This requirement on the brackets ensures that there is a unique order for evaluating the (sub)expressions in the case that an expression has multiple operators. We will return to this issue later in the course when we discuss expression trees.

In your solution, <em>you may assume that each expression has been parenthesized with proper nesting</em>. Therefore, you don’t need to worry about <a href="https://en.wikipedia.org/wiki/Order_of_operations">operator precedence</a> issues nor checking for balanced parenthesis. Table 1 below lists some valid expressions and their respective values

<table width="300">

 <tbody>

  <tr>

   <td width="171">Expression</td>

   <td width="129">Evaluated Value</td>

  </tr>

  <tr>

   <td width="171">(2 / 3)</td>

   <td width="129">0</td>

  </tr>

  <tr>

   <td width="171">((- -(2 + 3)) – 1)</td>

   <td width="129">3</td>

  </tr>

  <tr>

   <td width="171">(++((7 – 2) * 3))</td>

   <td width="129">18</td>

  </tr>

  <tr>

   <td width="171">([((7 + 2) * (3 – 4))])</td>

   <td width="129">9</td>

  </tr>

  <tr>

   <td width="171">((7 – 2) * (++(4 – 3)))</td>

   <td width="129">10</td>

  </tr>

 </tbody>

</table>

Table 1: Example expressions with corresponding values

<h2>Parsing</h2>

Your first task is to parse in input expression string into meaningful units called <em>tokens</em>. Parsing will take an expression as a string and return an array list of the tokens in the expression, such that each token is string. We use an array list because it is more efficient than a linked list in certain situations. Thus, the expression “(27 + 572)” should be parsed into the list of five tokens: “(”, “27”, “+”, “572”, “)”. Note that white spaces in an expression string must be ignored when parsing.

We can then take this list of tokens as input to the eval() method. It is important that for any expressions containing the “++” and “– –” operators be stored as a single token rather than as two. Thus, “(++25)” should be parsed as “(”, “++”, “25”, “)” rather than “(”, “+”, “+”, “25”, “)”. Also note that “25” must be not be parsed as “2”, “5”.

The parsing is done in the Expression constructor. <em>You will need to fill in the missing code where it is indicated.</em>

You should use the Java <a href="https://docs.oracle.com/javase/7/docs/api/java/lang/StringBuilder.html">StringBuilder</a> class to build each token as you parse the expression string. The append() and delete() methods of the StringBuilder class will be helpful in tokenizing the string.

<h2>Evaluation</h2>

The evaluation is done in the eval method. Again, your task is to fill in the missing code where it is indicated. The easiest approach is to use two stacks, one to store values (named valueStack in the starter code) and another (named operatorStack) to store tokens such as operators or brackets.

We emphasize that you may assume that all expressions are valid; that is, they are syntactically correct and will evaluate to a single value. HINT: Considering that all expressions are syntactically correct, there is no need to store the opening bracket of an expression in the stacks.

<h2>Testing</h2>

We provide an additional tester class to test your expression class along with sample testing files (sample in.txt, sample value.txt) which test for simple expressions only. <em>Your code will be evaluated on a much more extensive and challenging set of test cases, and we encourage you to post complicated test cases on the discussion board.</em>

<h2>Grading</h2>

Your grade will be based on the number of test cases passed. No points for code-style or readability, though you should comment your code for your own benefit.

<table width="472">

 <tbody>

  <tr>

   <td width="380">a) Implement parser in the Expression constructor</td>

   <td width="92"><strong>(40 points)</strong></td>

  </tr>

  <tr>

   <td width="380">b) Implement support for operators +, –, *, /</td>

   <td width="92"><strong>(20 points)</strong></td>

  </tr>

  <tr>

   <td width="380">c) Implement support for operators – – and ++</td>

   <td width="92"><strong>(20 points)</strong></td>

  </tr>

  <tr>

   <td width="380">d) Implement support for operator [ ]</td>

   <td width="92"><strong>(20 points)</strong></td>

  </tr>

 </tbody>

</table>

<h1>Other Specifications</h1>

All your code should be written in the Expression.java file. Submit a file A2.zip which contains only the file Expression.java .

Only add code where it is indicated.

Do not add import statements.

You can use whatever package statement you want. The grading software will remove it.

Good luck and have fun!