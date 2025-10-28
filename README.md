# Imandra proofs of the "top 100" theorems :-)

This is a project by [Grant Passmore](https://www.cl.cam.ac.uk/~gp351) to prove the [Top 100 Theorems](https://www.cs.ru.nl/~freek/100/) in [Imandra](https://marketplace.visualstudio.com/items?itemName=imandra.imandrax).

# Status

Currently, we have proven **15/100**:

1\. [Irrationality of √2](#thm-1)  
3\. [Denumerability of the Rationals](#thm-3)  
4\. [Pythagorean Theorem](#thm-4)  
11\. [Infinitude of Primes](#thm-11)  
42\. [Sum of the Reciprocals of the Triangular Numbers](#thm-42)  
44\. [Binomial Theorem](#thm-44)  
52\. [Number of Subsets of a Set](#thm-52)  
58\. [Formula for Number of Combinations](#thm-58)  
60\. [Bezout's Theorem](#thm-60)  
65\. [Isosceles Triangle Theorem](#thm-65)  
69\. [Greatest Common Divisor Algorithm](#thm-69)  
74\. [Principle of Mathematical Induction](#thm-74)  
80\. [The Fundamental Theorem of Arithmetic](#thm-80)  
85\. [Divisibility by 3 Rule](#thm-85)  
91\. [The Triangle Inequality](#thm-91)  

More coming soon!

# Statements of the Theorems in Imandra


<a id="thm-1"></a>
## 1. Irrationality of √2

[Source: src/sqrt2_irrational.iml](src/sqrt2_irrational.iml)

*Statement (informal):* There does not exist $p\in\mathbb{Q}$ s.t. $p^2=2$.

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem sqrt2_is_irrational (r:Rational.t) =
  Rational.is_reduced r
  ==> Rational.square r <> Rational.of_int 2
```
</details>

[Back to list](#status)


<a id="thm-3"></a>
## 3. Denumerability of the Rationals

[Source: src/q_denum.iml](src/q_denum.iml)

*Statement (informal):* There is a surjection from $\mathbb{N}$ to $\mathbb{Q}$.

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem decode_onto (q:Rational.t) =
  decode (encode q) = q

theorem rationals_denumerable_surj (q : Rational.t) =
  Bijection.surj
    decode
    encode
    (fun (n:int) -> n >= 0)       (* dom on N *)
    (fun (_:Rational.t) -> true)  (* all rationals allowed *)
    q
```
</details>

[Back to list](#status)


<a id="thm-4"></a>
## 4. Pythagorean Theorem

[Source: src/pythagoras.iml](src/pythagoras.iml)

*Statement (informal):* 

If the triangle $\triangle ABC$ is right-angled at vertex $C$, then
$$|AB|^2 = |AC|^2 + |BC|^2.$$

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem pythagoras (a : point) (b : point) (c : point) =
  right_at c a b ==> dist2 a b = Real.(dist2 a c + dist2 b c)
```
</details>

[Back to list](#status)


<a id="thm-11"></a>
## 11. Infinitude of Primes

[Source: src/inf_primes.iml](src/inf_primes.iml)

*Statement (informal):* There are infinitely many prime numbers.

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem infinitude_of_primes n =
  let bigger_prime = euclid (abs n) in
  is_prime bigger_prime && bigger_prime > n
```
</details>

[Back to list](#status)


<a id="thm-42"></a>
## 42. Sum of the Reciprocals of the Triangular Numbers

[Source: src/triangular.iml](src/triangular.iml)

*Statement (informal):*  

The sum of the reciprocals of the triangular numbers converges to $2$.

More formally, for all $n \ge 0$, the partial sum of the reciprocals of the first $n$ triangular numbers satisfies

$$
\sum_{k=1}^{n} \frac{1}{T_k} = 2 - \frac{2}{n+1},
$$

where $T_k = \dfrac{k(k+1)}{2}$.


<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem sum_recip_tri_closed_form n =
  n >= 0
  ==>
  sum_recip_tri n = 2.0 -. (2.0 /. (Real.of_int (n+1)))
```
</details>

[Back to list](#status)


<a id="thm-44"></a>
## 44. Binomial Theorem

[Source: src/binomial.iml](src/binomial.iml)

*Statement (informal):* 

For all $n \in \mathbb{N}$,

$$
(x + y)^n = \sum_{k=0}^{n} \binom{n}{k} x^{n-k} y^{k}.
$$

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem binomial_theorem n x y =
  n >= 0 && x >= 0 && y >= 0 
  ==> pow (x + y) n = binom_eval n x y
```
</details>

[Back to list](#status)


<a id="thm-52"></a>
## 52. Number of Subsets of a Set

[Source: src/num_subsets.iml](src/num_subsets.iml)

*Statement (informal):* 
If $S$ is finite, then $|\mathcal{P}(S)|=2^{|S|}$.

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem powerset_len xs =
  List.length (powerset xs) = pow 2 (List.length xs)
```
</details>

[Back to list](#status)


<a id="thm-58"></a>
## 58. Formula for Number of Combinations

[Source: src/combinations.iml](src/combinations.iml)

*Statement (informal):* 

For integers $0\le k\le n$, $\displaystyle \binom{n}{k}=\frac{n!}{k!(n-k)!}$.

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem choose_closed_form n k =
  0 <= k && k <= n
   ==> 
  Binomial.choose n k = fact n / (fact k * fact (n - k))
```
</details>

[Back to list](#status)


<a id="thm-60"></a>
## 60. Bezout's Theorem

[Source: src/gcd.iml](src/gcd.iml)

*Statement (informal):* 

For all $a,b\in\mathbb{Z}$ there exist $x,y\in\mathbb{Z}$ such that $ax+by=\gcd(a,b)$.

In Imandra, we construct the Bezout coefficients with the function `bezout_sub`.

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
let rec bezout_sub (a:int) (b:int) : int * int =
  if a < 0 || b < 0 then (0,0)
  else if a = 0 then (0,1)
  else if b = 0 then (1,0)
  else if a >= b then
    let (u',v') = bezout_sub (a - b) b in
    (u', v' - u')
  else
    let (u',v') = bezout_sub a (b - a) in
    (u' - v', v')

theorem bezout a b =
  let (u,v) = bezout_sub a b in
  u*a + v*b = gcd a b
```
</details>

[Back to list](#status)


<a id="thm-65"></a>
## 65. Isosceles Triangle Theorem

[Source: src/isosceles.iml](src/isosceles.iml)

*Statement (informal):*  

In triangle $ABC$, if the two sides meeting at vertex $C$ are equal in length, i.e., $|AC| = |BC|$, then the base angles at $A$ and $B$ are equal:  

$$
\angle CAB = \angle ABC.
$$

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem isosceles_triangle (a:point) (b:point) (c:point) =
  neqp a b && neqp a c
  && dist2 a c = dist2 b c
  ==>
  angle_eq (sub c a) (sub b a) (sub a b) (sub c b)
```
</details>

[Back to list](#status)


<a id="thm-69"></a>
## 69. Greatest Common Divisor Algorithm

[Source: src/gcd.iml](src/gcd.iml)

*Statement (informal):*  

For natural numbers $a,b$, the Euclidean GCD algorithm defined by repeated subtraction always terminates and yields a positive integer $g$ such that $g$ divides both $a$ and $b$, and every common divisor of $a$ and $b$ divides $g$.

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
let rec gcd (a:int) (b:int) : int =
  if a < 0 || b < 0 then 0
  else if a = 0 then b
  else if b = 0 then a
  else if a >= b then gcd (a - b) b
  else gcd a (b - a)

theorem gcd_remainder_step a b =
  a >= 0 && b > 0 ==> gcd a b = gcd b (a mod b)

theorem gcd_dvd_both_pos a b =
  gcd a b > 0 ==> a mod (gcd a b) = 0 && b mod (gcd a b) = 0

theorem gcd_absorbs_common_divisor d a b =
  d > 0 && a mod d = 0 && b mod d = 0 ==> (gcd a b) mod d = 0
```
</details>

[Back to list](#status)


<a id="thm-74"></a>
## 74. Principle of Mathematical Induction

[Source: src/math_induct.iml](src/math_induct.iml)

*Statement (informal):*  

If $P(0)$ holds and $\forall n,(P(n)\Rightarrow P(n+1))$, then $\forall n\in\mathbb{N},P(n)$.

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
axiom base prop =
  prop 0

axiom inductive prop n =
  n >= 0 && prop n ==> prop (n+1)

theorem induction prop n =
  n >= 0 ==> prop n
```
</details>

[Back to list](#status)


<a id="thm-80"></a>
## 80. The Fundamental Theorem of Arithmetic

[Source: src/fta.iml](src/fta.iml)

*Statement (informal):*  

Every integer $n>1$ can be expressed as a unique product of primes.

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem prime_factorization_exists n =
  n >= 2 
  ==> all_prime (prime_factors n) && prod_list (prime_factors n) = n

theorem prime_factorization_unique xs ys =
  all_prime xs && all_prime ys && prod_list xs = prod_list ys
  ==> permutation xs ys
```
</details>

[Back to list](#status)



<a id="thm-85"></a>
## 85. Divisibility by 3 Rule

[Source: src/div_by_3.iml](src/div_by_3.iml)

*Statement (informal):*  

An integer $n$ is divisible by $3$ iff the sum of its base-$10$ digits is divisible by $3$.


<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem sum_digits_preserves_mod3 n =
  n >= 0 ==> n mod 3 = sum_digits n mod 3
```
</details>

[Back to list](#status)


<a id="thm-91"></a>
## 91. The Triangle Inequality

[Source: src/tri_ineq.iml](src/tri_ineq.iml)

*Statement (informal):*  

For all vectors $x, y$ in the plane, the length of their sum is at most the sum of their lengths:

$$
\|x + y\| \le \|x\| + \|y\|.
$$

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem triangle_inequality (x:R2.vec) (y:R2.vec) (nx:real) (ny:real) (nxy:real) =
  let open Real in
  let open R2 in
  nx >= 0.0 && ny >= 0.0 && nxy >= 0.0 &&
  nx *. nx = norm x && ny *. ny = norm y && nxy *. nxy = norm (add x y)
  ==> nxy <= nx +. ny
```
</details>

[Back to list](#status)


# Contact

Grant Passmore (grant@imandra.ai)
