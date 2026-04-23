# Imandra proofs of the "top 100" theorems :-)

This is a project by [Grant Passmore](https://www.cl.cam.ac.uk/~gp351) to prove the [Top 100 Theorems](https://www.cs.ru.nl/~freek/100/) in [Imandra](https://marketplace.visualstudio.com/items?itemName=imandra.imandrax).

# Status

Currently, we have proven **30/100**:

1\. [Irrationality of √2](#thm-1)  
3\. [Denumerability of the Rationals](#thm-3)  
4\. [Pythagorean Theorem](#thm-4)  
10\. [Euler's Generalization of Fermat's Little Theorem](#thm-10)  
11\. [Infinitude of Primes](#thm-11)  
30\. [The Ballot Problem](#thm-30)  
34\. [Divergence of the Harmonic Series](#thm-34)  
38\. [Arithmetic Mean/Geometric Mean](#thm-38)  
42\. [Sum of the Reciprocals of the Triangular Numbers](#thm-42)  
44\. [Binomial Theorem](#thm-44)  
51\. [Wilson's Theorem](#thm-51)  
52\. [Number of Subsets of a Set](#thm-52)  
54\. [Königsberg Bridges Problem](#thm-54)  
58\. [Formula for Number of Combinations](#thm-58)  
60\. [Bezout's Theorem](#thm-60)  
65\. [Isosceles Triangle Theorem](#thm-65)  
66\. [Sum of a Geometric Series](#thm-66)  
68\. [Sum of an Arithmetic Series](#thm-68)  
69\. [Greatest Common Divisor Algorithm](#thm-69)  
73\. [Ascending or Descending Sequences (Erdos-Szekeres)](#thm-73)  
74\. [Principle of Mathematical Induction](#thm-74)  
77\. [Sum of Kth Powers (Faulhaber's Formula)](#thm-77)  
78\. [Cauchy-Schwarz Inequality](#thm-78)  
80\. [The Fundamental Theorem of Arithmetic](#thm-80)  
85\. [Divisibility by 3 Rule](#thm-85)  
88\. [Derangements Formula](#thm-88)  
89\. [Factor and Remainder Theorem](#thm-89)  
91\. [The Triangle Inequality](#thm-91)  
93\. [The Birthday Problem](#thm-93)  
96\. [Principle of Inclusion/Exclusion](#thm-96)  

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


<a id="thm-10"></a>
## 10. Euler's Generalization of Fermat's Little Theorem

[Source: src/euler.iml](src/euler.iml)

*Statement (informal):*
If $\gcd(a, n) = 1$ and $n \ge 2$, then $a^{\varphi(n)} \equiv 1 \pmod{n}$, where $\varphi(n)$ is Euler's totient function.

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem euler a n =
  n >= 2 && 1 <= a && a < n && gcd a n = 1
  ==> pow a (phi n) mod n = 1
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


<a id="thm-30"></a>
## 30. The Ballot Problem

[Source: src/ballot.iml](src/ballot.iml)

*Statement (informal):*

In an election where candidate A receives $a$ votes and candidate B receives $b$ votes with $a > b$, the probability that A is strictly ahead of B throughout the count is

$$\frac{a - b}{a + b}.$$

Equivalently, in integer form: among all $n!$ orderings of the $n = a + b$ voters (where $e$ is the set of A-supporters), the number of favorable orderings (in which A maintains a strict lead at every non-empty prefix) is

$$(a - b)\cdot (n - 1)!.$$

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem ballot_theorem (p : int list) e =
  Sets.dlistp p
  ==> num_favs (perms p) e * List.length p
      = (if a_count p e > b_count p e
         then a_count p e - b_count p e
         else 0)
        * Combinations.fact (List.length p)
```
</details>

[Back to list](#status)


<a id="thm-34"></a>
## 34. Divergence of the Harmonic Series

[Source: src/harmonic.iml](src/harmonic.iml)

*Statement (informal):*  
The harmonic series diverges: its partial sums grow without bound.

More precisely, let

$$
H(n) = 1 + \frac{1}{2} + \frac{1}{3} + \cdots + \frac{1}{n}.
$$  

Then, $\forall m \ge 0$, if $n \ge 2^{2m}$ then $H(n) > m$.

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem harmonic_series_diverges m n =
  m >= 0 && n >= exp2 (2*m)
  ==> 
  harmonic_sum n >. Real.of_int m
```
</details>

[Back to list](#status)



<a id="thm-38"></a>
## 38. Arithmetic Mean/Geometric Mean

[Source: src/am_gm.iml](src/am_gm.iml)

*Statement (informal):*
For non-negative reals $a_1, \ldots, a_n$, the arithmetic mean is at least the geometric mean:

$$\frac{a_1 + a_2 + \cdots + a_n}{n} \ge \left(a_1 \cdot a_2 \cdots a_n\right)^{1/n}.$$

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem am_gm_power xs =
  let n = List.length xs in
  n >= 1 && all_nonneg xs ==>
  real_pow (sum xs) n >=. real_pow (Real.of_int n) n *. prod xs

theorem am_gm xs g =
  xs <> []
  && all_nonneg xs
  && g >=. 0.0
  && real_pow g (List.length xs) = prod xs
  ==> Real.of_int (List.length xs) *. g <=. sum xs
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


<a id="thm-51"></a>
## 51. Wilson's Theorem

[Source: src/wilson.iml](src/wilson.iml)

*Statement (informal):*
If $p$ is prime, then $(p-1)! \equiv -1 \pmod{p}$.

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem wilson p =
  is_prime p ==> Combinations.fact (p - 1) mod p = p - 1
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


<a id="thm-54"></a>
## 54. Königsberg Bridges Problem

[Source: src/konigsberg.iml](src/konigsberg.iml)

*Statement (informal):*  
The **Königsberg Bridges Problem** asks whether it is possible to take a walk through the city of Königsberg that crosses each of its seven bridges exactly once and returns to its starting point.  

Euler showed that such a walk is impossible.  In modern graph-theoretic terms, the corresponding graph has more than two vertices of odd degree, and thus has no Eulerian path.

We prove two versions:  
- a **concrete version**, representing the specific Königsberg map and a purported (impossible) path of length 7 directly, which Imandra proves automatically, and  
- a **general version**, using the abstract notion of *Eulerian polarity* and reasoning over permutations of edges, and then instantiating this to the configuration of Königsberg.

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem konigsberg_concrete start b1 b2 b3 b4 b5 b6 b7 =
  not (eulerian start b1 b2 b3 b4 b5 b6 b7)

theorem konigsberg r p =
  List.mem r regions && valid_path p r
  ==> not (permutation p bridges)
```


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

$$\angle CAB = \angle ABC.$$

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


<a id="thm-66"></a>
## 66. Sum of a Geometric Series

[Source: src/geometric_series.iml](src/geometric_series.iml)

*Statement (informal):*  
For a geometric sequence with first term $a$, last term $l$, and common ratio $r \neq 1$,

$$\sum_{k=0}^{n-1} a r^k = \frac{a - r \cdot l}{1 - r}.$$

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem sum_geometric_series seq ratio =
  geometric_sequence seq ratio && ratio <> 1.0 ==>
  sum seq = Real.((first seq - ratio * last seq) / (1.0 - ratio))
```
</details>

[Back to list](#status)


<a id="thm-68"></a>
## 68. Sum of an Arithmetic Series

[Source: src/arithmetic_series.iml](src/arithmetic_series.iml)

*Statement (informal):*  
For an arithmetic sequence with first term $a$, common difference $d$, and $n+1$ terms,

$$\sum_{k=0}^{n} (a + kd) = \frac{(n+1)(2a + nd)}{2}.$$

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem sum_arithmetic_series a d n =
  n >= 0 ==>
  arithmetic_sum a d n 
   = Real.((of_int n + 1.0) * (2.0 * a + (of_int n * d)) / 2.0)
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


<a id="thm-73"></a>
## 73. Ascending or Descending Sequences (Erdos-Szekeres)

[Source: src/ascending.iml](src/ascending.iml)

*Statement (informal):*

If $l$ is a list of distinct reals of length $> rs$, then $l$ has an ascending subsequence of length $> r$ or a descending subsequence of length $> s$.

Equivalently, any sequence of more than $(r{-}1)(s{-}1)$ distinct reals contains an ascending subsequence of length $\ge r$ or a descending subsequence of length $\ge s$.

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem erdos_szekeres l r s =
  dlistp l
  && List.length l > r * s
  && r >= 0 && s >= 0
  ==> List.length (longest_ascending_subseq l) > r
      || List.length (longest_descending_subseq l) > s
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


<a id="thm-77"></a>
## 77. Sum of Kth Powers (Faulhaber's Formula)

[Source: src/sum_kth_powers.iml](src/sum_kth_powers.iml)

*Statement (informal):*

For all $n \ge 0$ and all $m \ge 0$,

$$
(m+1)\,\sum_{k=1}^{n} k^m \;=\; B_{m+1}(n+1) - B_{m+1}(1),
$$

where $B_j(x)$ is the $j$-th Bernoulli polynomial (with the convention $B_1 = -\tfrac{1}{2}$).

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem faulhaber n m =
  n >= 0 && m >= 0
  ==> Real.of_int (m + 1) *. sum_pow_r n m
       = bernpoly (m + 1) (Real.of_int (n + 1))
         -. bernpoly (m + 1) 1.0
```
</details>

[Back to list](#status)


<a id="thm-78"></a>
## 78. Cauchy-Schwarz Inequality

[Source: src/cauchy_schwarz.iml](src/cauchy_schwarz.iml)

*Statement (informal):*  

For all vectors $u, v$ in an inner product space,

$$|\langle u, v \rangle| \le \|u\| \cdot \|v\|.$$

Equivalently, $\langle u, v \rangle^2 \le \langle u, u \rangle \cdot \langle v, v \rangle$.

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem cauchy_schwarz_1 u v =
  same_length u v ==>
  Real.(dot u v * dot u v) <=. Real.(dot u u * dot v v)

theorem cauchy_schwarz_2 u v eu ev =
  same_length u v && is_sqrt eu (norm_sq u) && is_sqrt ev (norm_sq v) ==>
  abs_real (dot u v) <=. Real.(eu * ev)
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


<a id="thm-88"></a>
## 88. Derangements Formula

[Source: src/derangements.iml](src/derangements.iml)

*Statement (informal):*

The number of derangements (permutations with no fixed points) of $\{0, \ldots, n-1\}$ is

$$D(n) = n! \sum_{k=0}^{n} \frac{(-1)^k}{k!}.$$

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem derangements_formula n =
  n >= 0 ==> List.length (derangements n) = subfact n
```
</details>

[Back to list](#status)


<a id="thm-89"></a>
## 89. Factor and Remainder Theorem

[Source: src/factor_remainder.iml](src/factor_remainder.iml)

*Statement (informal):*

For any polynomial $p(x)$ and value $a$, the remainder when $p(x)$ is divided by $(x - a)$ is $p(a)$.  That is,

$$p(x) = (x - a) \cdot q(x) + p(a)$$

for some polynomial $q$.  As a corollary, $(x - a)$ divides $p(x)$ iff $p(a) = 0$.

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem remainder_theorem p a x =
  poly_eval p x = Real.((x - a) * poly_eval (synth_div p a) x + poly_eval p a)

theorem factor_remainder p a x =
  poly_eval p a = 0.0
  = (poly_eval p x = Real.((x - a) * poly_eval (synth_div p a) x))
```
</details>

[Back to list](#status)


<a id="thm-91"></a>
## 91. The Triangle Inequality

[Source: src/tri_ineq.iml](src/tri_ineq.iml)

*Statement (informal):*  

For all vectors $x, y$ in the plane, the length of their sum is at most the sum of their lengths:

$$\|x + y\| \le \|x\| + \|y\|.$$

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem triangle_inequality (x:R2.vec) (y:R2.vec) (nx:real) (ny:real) (nxy:real) =
  let open Real in
  let open R2 in
  nx >= 0.0 && ny >= 0.0 && nxy >= 0.0 &&
  nx * nx = norm x && ny * ny = norm y && nxy * nxy = norm (add x y)
  ==> nxy <= nx + ny
```
</details>

[Back to list](#status)


<a id="thm-93"></a>
## 93. The Birthday Problem

[Source: src/birthday.iml](src/birthday.iml)

*Statement (informal):*

The smallest number of people for which the probability of a shared birthday exceeds $\frac{1}{2}$ is $23$.

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
theorem birthday_problem k =
  (0 <= k && k <= 22 ==> collision_prob k <=. 0.5)
  && (k = 23 ==> collision_prob k >. 0.5)
```
</details>

[Back to list](#status)


<a id="thm-96"></a>
## 96. Principle of Inclusion/Exclusion

[Source: src/inclusion_exclusion.iml](src/inclusion_exclusion.iml)

*Statement (informal):*

If $l = [A_1, \ldots, A_n]$ is a non-empty list of distinct finite subsets of some universe $u$ (each $A_i$ represented as a dlist), then

$$|A_1 \cup \cdots \cup A_n| = \sum_i |A_i| - \sum_{i<j} |A_i \cap A_j| + \sum_{i<j<k} |A_i \cap A_j \cap A_k| - \cdots.$$

<details open>
<summary><strong>Imandra statement</strong></summary>

```ocaml
(* The alternating sum, starting from k-subsets *)
let rec inc_exc_aux (l : 'a list list) (k : int) : int =
  if k < 1 || k > List.length l then 0
  else Sets.(sum_len_int (subsets_of_order k l)
       - inc_exc_aux l (k + 1))

(* The inclusion-exclusion formula *)
let inc_exc (l : 'a list list) : int =
  inc_exc_aux l 1

theorem inclusion_exclusion_principle u l =
  Sets.(dlistp u && dlistp l && l <> [] && sublistp l (subsets u)
        ==> inc_exc l = List.length (union_list l))
```
</details>

[Back to list](#status)


# Contact

Grant Passmore (grant@imandra.ai)
