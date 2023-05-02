Download Link: https://assignmentchef.com/product/solved-comp302-assignment4-polymorphic-trees
<br>
[Question 1:In this question we work with polymorphic trees

type ’a tree = Empty | Node of ’a tree * ’a * ’a tree;;

Write a function mapTree that takes a function and a tree and applies the function to every node of the tree. Here is the type

val mapTree : (’a -&gt; ’b) * ’a tree -&gt; ’b tree = &lt;fun&gt;

Here is an example

# let t1 = Node(Empty, 0, (Node(Empty, 1, Empty)));; let t2 = Node(Node(Empty,5,Empty),6,Empty);; let t3 = Node(Node(t2,2,Node(Empty,3,Empty)),4,t1);;

# t3;;

<ul>

 <li>: int tree =</li>

</ul>

Node

(Node (Node (Node (Empty, 5, Empty), 6, Empty), 2, Node (Empty, 3, Empty)),

4, Node (Empty, 0, Node (Empty, 1, Empty)))

# mapTree((fun n -&gt; n + 1), t3);;

<ul>

 <li>: int tree =</li>

</ul>

Node

(Node (Node (Node (Empty, 6, Empty), 7, Empty), 3, Node (Empty, 4, Empty)),

5, Node (Empty, 1, Node (Empty, 2, Empty)))

[Question 2:  Suppose that <em>f </em>: <strong>R </strong>→ <strong>R </strong>is a <em>continuous </em>function from the real numbers to the real numbers. Suppose further that there is an interval [<em>a,b</em>] in which the function is known to have <em>exactly one root</em><a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a>. We will assume that<a href="#_ftn2" name="_ftnref2"><sup>[2]</sup></a> <em>f</em>(<em>a</em>) <em>&lt; </em>0 and <em>f</em>(<em>b</em>) <em>&gt; </em>0 and the root <em>r </em>is somewhere in between. There is an algorithm that is reminiscent of binary search that finds (a good approximation of) the root. The <em>half-interval method </em>for finding the root of a real-valued function works as follows. The idea is to search for the root by first guessing that the root is the midpoint of the interval. If the midpoint is the root we are done. If not, we check whether the value of <em>f </em>at the midpoint is positive or negative. If it is positive then the root must be between <em>a </em>and the midpoint; if it is negative, the root must be between the midpoint and <em>b</em>. In either case we recursively call the search algorithm. Code this algorithm in OCaml. The following shows the names and types that we expect.

# let rec halfint((f: float -&gt; float),

(posValue:float),(negValue:float),(epsilon:float)) = ….

val halfint : (float -&gt; float) * float * float * float -&gt; float = &lt;fun&gt;

The parameter epsilon is used to determine the accuracy with which you are making comparisons. Recall that you cannot <em>reliably </em>test floating-point numbers for equality. You may need to use Float.abs which is the floating point version of the absolute value function.

[Question 3:  This is a classic example of a higher-order function in action. Newton’s method can be used to generate approximate solutions to the equation <em>f</em>(<em>x</em>) = 0 where <em>f </em>is a differentiable real-valued function of the real variable <em>x</em>. The idea can be summarized as follows: Suppose that <em>x</em><sub>0 </sub>is some point which we suspect is near a solution. We can form the linear approximation <em>l </em>at <em>x</em><sub>0 </sub>and solve the linear equation for a new approximate solution. Let <em>x</em><sub>1 </sub>be the solution to the linear approximation <em>l</em>(<em>x</em>) = <em>f</em>(<em>x</em><sub>0</sub>) + <em>f</em><sup>0</sup>(<em>x</em><sub>0</sub>)(<em>x </em>− <em>x</em><sub>0</sub>) = 0. In other words,

If our first guess <em>x</em><sub>0 </sub>was a good one, then the approximate solution <em>x</em><sub>1 </sub>should be an even better approximation to the solution of <em>f</em>(<em>x</em>) = 0. Once we have <em>x</em><sub>1</sub>, we can repeat the process to obtain <em>x</em><sub>2</sub>, etc. In fact, we can repeat this process indefinitely: if, after <em>n </em>steps, we have an approximate solution <em>x<sub>n</sub></em>, then the next step is

<em>.</em>

This will produce approximate solutions to any degree of accuracy provided we have started with a good guess <em>x</em><sub>0</sub>. If we have reached a value <em>x<sub>n </sub></em>such that |<em>f</em>(<em>x<sub>n</sub></em>)| <em>&lt; ε</em>, where <em>ε </em>is some real number representing the tolerance, we will stop.

Implement a function called newton with the type shown below. The code outline is for your guidance.

# let rec newton(f, (guess:float), (epsilon:float), (dx:float)) = let close((x:float), (y:float), (epsilon:float)) = abs_float(x-.y) &lt; epsilon in let improve((guess:float),f,(dx:float)) = …. in if close((f guess), 0.0, epsilon) then

guess else

….

val newton : (float -&gt; float) * float * float * float -&gt; float = &lt;fun&gt;

which when given a function <em>f</em>, a guess <em>x</em><sub>0</sub>, a tolerance <em>ε </em>and an interval <em>dx</em>, will compute a value <em>x</em><sup>0 </sup>such that |<em>f</em>(<em>x</em><sup>0</sup>)| <em>&lt; ε</em>. You can test this on built-in real-valued functions like sin or other functions in the mathematics library. Please note that this method does not always work: one needs stronger conditions on <em>f </em>to guarantee convergence of the approximation scheme. Never test it on <em>tan</em>! Here are two examples:

let make_cubic((a:float),(b:float),(c:float)) = fun x -&gt; (x*.x*.x +. a *. x*.x +. b*.x +. c)

let test1 = newton(make_cubic(2.0,-3.0,1.0),0.0,0.0001,0.0001)

(* Should get -3.079599812; don’t worry if your last couple of digits are off. *) let test2 = newton(sin,5.0,0.0001,0.0001) (* Should get 9.42477…. *)

[Question 4:  In class we say how to define a higher-order function that computes <em>definite </em>integrals of the form

Z <em>a</em>

<em>f</em>(<em>x</em>)d<em>x.</em>

<em>b</em>

To remind you

let integral((f: float -&gt; float),(lo:float),(hi:float),(dx:float)) =

let delta (x:float) = x +. dx in dx *. iterSum(f,(lo +. (dx/.2.0)), hi, delta);;

where iterSum was also defined in class. In this question I want you to define a function that computes the <em>indefinite integral</em>:

This means that it returns a function not a number. Here is the type that I expect

# let indIntegral(f,(dx:float)) = … val indIntegral : (float -&gt; float) * float -&gt; float -&gt; float = &lt;fun&gt;

Use any of the functions defined in class. We will preload iterSum and integral so you won’t have to recode them. <strong>You do not have to do test cases for the last question.</strong>

<a href="#_ftnref1" name="_ftn1">[1]</a> The root of a function <em>f </em>is a value <em>r </em>such that <em>f</em>(<em>r</em>) = 0.

<a href="#_ftnref2" name="_ftn2">[2]</a> When I am writing mathematical statements I do not bother to write 0<em>.</em>0; when I am writing code I am careful to distinguish between 0 and 0<em>.</em>0.