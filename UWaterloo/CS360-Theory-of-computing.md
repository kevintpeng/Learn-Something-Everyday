# Theory of Computing
### Summary
- **Cantor's** theorem: the power set of the natural numbers P(N) is uncountable (since N is an infinite set)
- any single language is countable because you can map the natural numbers to the infinite set of all possible strings (again, there is a bijection that can be defined)
- on Deterministic Finite Automata and Non-deterministic Finite Automata equivalence, a language `A` is regular iff there exists an NFA that accepts all strings in `A`
- there are uncountably many languages over sigma, but countably many regular languages
- **pumping lemma** (for regular languages) tells us that there is always a loop in a DFA for input string whose length is greater or equal to then number of states
- context-free languages always have a pushdown automata and a context free grammar that represent them, which with the addition of a stack makes the behaviour of each state depend on what's on the top of the stack
- **pumping lemma for context-free grammars** works on parse trees, and says that after the parse tree has a certain height, then there must be some path from the root to leaf that has more nodes than states, and thus we get a loop
- Deterministic stack machines are an interesting alternative and equivalent computing model to the turing machine, extending pushdown automata with a second stack gives them the ability to perform universal computation
  - we can encode anything as strings including DSMs themselves, so we can describe problems as sets of DSMs that solve those problems under given constraints
- **semi-decidable** languages are a subset of undecidable languages that have machines that describe them that are guaranteed to terminate when reading a string that must be accepted
  - there are non-semidecidable languages like DIAG, the language of all DSMs that do not accept themselves, which paradoxically cannot accept or reject itself
- for languages described by turing machines and deterministic stack machines, there is no fundamental syntactic structure to determine whether the language is infinite or not (whether a loop exists)
- **NP** is the set of languages/problems that have a polynomial-time verifier
- **NP complete** languages are the hardest in NP, since it is NP hard (anything in NP reduces to it) while still being in NP
- **time-hierarchy theorem** gives us that if f(x) = o(g(x)), then DTIME(f) is a proper subset of DTIME(g)
  - **DTIME(f)** is the set of languages that have a DSM whose running time satisfies O(f) deterministically (and NTIME for nondeterministically)

### 1 [Background](https://cs.uwaterloo.ca/~watrous/CS360/Lectures/01.pdf)
- Computational problems themselves can be viewed as mathematical objects
- a set `A` is countable iff there is an injective function from A -> Natural numbers
- if set `A` is infinite, it is countable if there's a bijection (total)
- recall the power set is the set of all subsets of a set
- one proof pattern is breaking down a set into finite lists of sets for the purpose of enumeration
- **Cantor's** theorem: the power set of the natural numbers P(N) is uncountable (since N is an infinite set)
- strings are finite sequences of symbols from an alphabet
- languages are sets of strings

### 2 [DFAs and Countability](https://cs.uwaterloo.ca/~watrous/CS360.Spring2019/Lectures/02.pdf)
- any single language is countable because you can map the natural numbers to the infinite set of all possible strings (again, there is a bijection that can be defined)
- the set of all languages over any alphabet is uncountable by Cantor's theorem
- recall from CS241 that a DFA is a state diagram representation
- we use the function L as a filter for a language
- a language `A` is **regular** if there is a DFA, `M`, s.t. `L(M) = A`

### 3 [NFA](https://cs.uwaterloo.ca/~watrous/CS360/Lectures/03.pdf)
- we can think about non-determinism as verification
  - given a string, NFA determines whether its possibly accepted
- there can be multiple possible transitions for each input to the transition function, and epsilon is a possible input
- NFA accepts a string if there exists some
- epsilon-closure is an extension of NFAs over a set of states, e(R) that is the set of states reachable from states in R using just epsilon transitions
- using this, we can delta * for NFAs (taking a string as input)
- a language `A` is regular iff there exists an NFA that accepts all strings in `A`

### 4 [Regular Expressions](https://cs.uwaterloo.ca/~watrous/CS360/Lectures/04.pdf)
3 regular operations:
- union of language A and B: w is in either
- concatenation AB: wx is every combination of w in A then x in B
- kleene star A * : {empty} u A u AA u AAA ...

If A and B are regular languages, any regular operations on them produce regular languages
- prove by defining an NFA that uses DFAs for A and B to accept the resultant language
- proving kleene star acts as a closure does not follow from union and concatenation, because it is an infinite number of operations applied so it needs its own proof

### 5 [Proving languages to be nonregular](https://cs.uwaterloo.ca/~watrous/CS360/Lectures/05.pdf)
- **pumping lemma** for regular languages allows us to observe the pigeon hole principle for DFAs: that more inputs than states means that some state was visited more than once.
  - also, for any string length greater than or equal to n for n states, there must be a loop
  - formally, for a regular language there is always some point at which all strings longer than or equal to `n` can be written containing a loop in the first n characters
  
How to use the pumping lemma:
1. fix n as any number, we describe the rest of the proof based on any fixed selection of n
1. define a string towards contradiction, that satisfies the requirements of the pumping lemma
1. determine what form the loop must take
1. because the pumping lemma must hold for all number of loops, show that there is a particular number of loops where xy<sup>i</sup>z is not contained in the language which contradicts the third condition of the pumping lemma.

- one example of a nonregular language is closing parentheses
- one where the number of loops affects the state is not truly a loop
- one where certain lengths are accepted, but are not cyclic values (exponential, prime)

### 6 [More regular languages](https://cs.uwaterloo.ca/~watrous/CS360/Lectures/06.pdf)
- reverse is another closure property of regular languages (proven by constructing a reversed NFA, or by reversing a regular expression)
- symmetric difference of languages is another clousre property
  - if A is regular, B is nonregular, A ▲ B is nonregular
- prefix is a closure property, proven by constructing a DFA where the new accepting states are the set of all states q where there's some string v s.t. δ * (q,v) in the original accepting states
- suffix is a closure property, proven by allowing an epsilon transition to any reachable state
- substring is a closure property, because it is a prefix of a suffix
- example proof showing that the language where a single symbol can be skipped can be proven using two copies of the DFA with modifications to the first copy, where there exists epsilon transitions from the first to the second DFA wherever there exists transitions within the first.

### [7 Context-free grammars and languages](https://cs.uwaterloo.ca/~watrous/CS360/Lectures/07.pdf)
- CFG `G = (V, Σ, R, S)`, where V is variables, Σ is the alphabet, R is rules, S is start variable
  - rules are of the form `A → w`, A in V, w in (V union Σ) * e.g. S -> 0 S 1, S -> epsilon
- every CFG generates a language L(G)
- formally, the **yields relation** defined by CFG G is a relation x maps to y, where some variable in V from x is replaced by some new set of symbols, following some rule in R.
- if x in L(G), we can find some **derrivation** of it from the CFG, a sequence of strings where the first is S and the last is x

Prove that all w in A is generated by CFG G:
1. Let w be a string in A, and set n = |w|. Prove that w is in L(G) by strong induction
1. base case: n = 0 so w is empty, and show that there is some derivation
1. induction step: assume n >= 1, and hypothesize that x in L(G) for every x in A with |x| < n
1. use properties of language A and the assumption that any strings less are generated by G to find a derivation of w in terms of other generated strings smaller than w

### [8 Parse Trees, Ambiguity, Chomsky Normal Form](https://cs.uwaterloo.ca/~watrous/CS360/Lectures/08.pdf)
- **left-most derivation**: the left most variable always gets replaced. All strings generated by a CFG have a left-most derivation because each step can only ever correspond to a single variable, never multi-variable so we choose can choose the left by convention
- ambiguity between parse trees can arise even in left-most derivations, and unambiguous CFGs are a requirement for some applications
- it is not always possible to generate unambiguous CFGs, specifically for **inherently ambiguous languages**
  - one such example includes an OR in its definition, and when you satisfy both conditions, it's ambiguous which one can be used to satisfy it
- a CFG can always be written in **Chomsky Normal Form**, so that only binary parse trees can be derived (allowing unary branches)
  - this was proven by transforming a CFG into CNF step-by-step and showing that they are eqivalent

### [9 Closure Properties for context-free languages](https://cs.uwaterloo.ca/~watrous/CS360/Lectures/09.pdf)
- context-free languages are closed under regular operations
- regular languages are a subset of context-free
- proving closure properties of context-free languages can be done similarly to regular languages, by constructing a CFG based off other CFGs, by manipulating vars and rules using the fact that all CFGs can be written in CNF.

### [10 Proving non-context-free](https://cs.uwaterloo.ca/~watrous/CS360/Lectures/10.pdf)
- pumping lemma for context-free languages: CFGs (in CNF for simplicity producing binary parse trees), then there is some height of parse tree (length of string) that ensures that there is a path that has repeating variables. Repeating variables means some sub-tree could repeat an arbitrary number of times

### [11 Pushdown Automata](https://cs.uwaterloo.ca/~watrous/CS360/Lectures/11.pdf)
- another way to express context-free languages
- similar to NFA, with a stack that is shared between states and can be operated on by the PDA
- P = (Q, sigma, Γ, delta, q0, F) six tuple, where delta can take a char from sigma or a tuple from Stack(Γ) = {↓, ↑} × Γ
- formally, a PDA accepts a string w if there is a sequence of states and a sequence of chars + stack ops that move from q0 to some f in F, where the stack ops produce a **valid stack string**
- symmetric diff of context-free lang A and finite lang B is context free

### 12 Stack Machines
- equivalent computational model to modern programming languages
- like a PDA with multiple stacks 
- it is a 7-tuple with the addition of reject states (necessary for deterministic version later)
- we pass it a populated stack as input, and you simply pop from stack-0
- since the input is a stack, all ops are pushes or pops of a stack
- Deterministic Stack machine (DSM) is a special case of NSM, where every state is either pushing or popping a single stack, and the transitions are based on what symbol is pushed/popped
  - excluded transitions implicitly go to reject state

### [13 Stack Machine computations, languages and functions](https://cs.uwaterloo.ca/~watrous/CS360/Lectures/13.pdf)
- **semi-decidable** is the class of languages that have a DSM that terminates with acceptance for all strings in their language (but may run forever in the case of rejection)
- **decidable languages** that have a DSM that never runs forever
- TODO computable functions

### [14 Turing Machines](https://cs.uwaterloo.ca/~watrous/CS360/Lectures/14.pdf)
- equivalant to DSMs
- tape can move left or right, is infinite and has slots with symbols on them. 
- a DSM can be emulated using a DTM, by building two stacks, one on the left and one on the right of the tape
- a DTM can be emulated using a DSM by having everything left of initial on a stack L, and right on a stack R

### [15 Encodings and decidable languages](https://cs.uwaterloo.ca/~watrous/CS360/Lectures/15.pdf)
- discussed encodings with one example using unambiguous encoding
- notation:〈M〉means the encoding of M, where in this case we want to encode the DFA M
- strategy used for encoding is to first represent the thing to be encoded as a mathematical tuple, then encode each component and string them together
- 〈M〉= 〈〈n〉,〈F〉,〈δ〉〉=  〈〈 |Γ| 〉,〈 F 〉,〈 〈j〉,〈a〉,〈δ(qj, a)〉 〉〉
- we can use the notion of encodings to "recursively" define languages as sets of DFAs or CFGs, specifically for **decidability of formal languages**
- A_DFA = 〈〈D〉,〈w〉〉where D is DFA and w is string in L(D) is decidable
- A_NFA is decidable by converting the NFA to a DFA
- the language of DFAs that express empty languages is decidable
- the language of pairs of CFGs that are equivalent is not decidable
- get comfortable expressing solutions as high level descriptions of DSMs (always reject invalid strings as step 1)
- roughly two classes of decidable languages were presented: ones where we simulate the encoded DFA or equivalent, and ones where we look at the language for the DFA

### [16 Universal stack machines and non-semidecidable language](https://cs.uwaterloo.ca/~watrous/CS360/Lectures/16.pdf)
- note: mapping multiple stacks to few is one challenge
- universal stack machine U takes an encoding of a DSM
- U semidecides Adsm implies Adsm is a semidecidable language
- DIAG is the language that contains all binary strings 〈M〉 where M is a DSM that rejects the its own encoding 〈M〉  
- start with DIAG and build up to prove not decidable

### [17 Undecidable Languages](https://cs.uwaterloo.ca/~watrous/CS360/Lectures/17.pdf)
Some undecidable languages:
- A_DSM is the language of all DSM-string pairs, where the string is in the language of the DSM
- HALT is the language of DSM-string pairs where the DSM halts on the input string
  - we can use contradiction--assume HALT is decidable and show that if it is, then A_DSM must also be
- E_DSM is the language of all DSMs that produce an empty language, proven using contradiction and A_DSM
- AM is the language that accepts the empty string

Two ways to prove undecidability:

1. contradiction
  - assume toward contradiction that it is decidable. Then construct a DSM that decides a language that we know already to be undecidable
2. reduction: A reduces to B lets us show that the decidability of B implies the decidability of A
  - reductions can be used to show decidability, semidecidability, undecidability
  - contrapositive result for A reduces to B allows us to phrase it as A undecidable imples B undecidable
  - `A` reduces to `B` iff `not A` reduces to `not B`
  - we first give a piecewise mapping `f`, argue that it is computable
  - then show that it is a reduction (all inputs from the language A mapped through f are in B, and all inputs not in A mapped through f are also not in B). Since it is a reduction, and A is undecidable, then we know B is also undecidable

### [18 Closure Properties in Decidability](https://cs.uwaterloo.ca/~watrous/CS360/Lectures/18.pdf)
TODO read this lecture more closely
- for decidable languages, union, concatenation, intersection, negation and kleen star are closure properties
  - not closed under prefix
- for semidecidable languages, union, concat, intersection, kleen star, prefix, suffix, substring are closure properties
  - not closed under negation
- for any infinite semidecidable language, there exists a subset that is an infinite decidable languages
- semidecidable languages are precisely the set of languages which correspond to the range of computable functions

### [19 Time-bounded Computations](https://cs.uwaterloo.ca/~watrous/CS360/Lectures/19.pdf)
- **time constructable** is used to define functions that are--informally--not theoretically malliciously constructed, in other words practically useful
- **DTIME(f)** is the set of languages that have a DSM whose running time satisfies f deterministically
- **time hierarchy theorem** is useful for showing that some class of problems is a strict subset of another
- P is the class of all problems computable in polynomial time
- EXP is exponential
- CFG-simulation related computations are polynomial time

### [20 NP, Polynomial-Time Mapping Reductions](https://cs.uwaterloo.ca/~watrous/CS360/Lectures/20.pdf)
- **polynomially bounded time-constructable functions** is the fancy way of saying a useful function that has polynomial runtime
- A language is in NP if there is some polynomial time verifier
- another approach is to think of NP as polynomial-time nondeterministic computation, NTIME(f)
- P is a subset of NP subset of EXP
  - if A in P, then A itself is a verifier so it's also in NP
- A **polynomial-time reduces** to BB if there is some polynomial-time computable function s.t. `w in A iff f(w) in B`
- B is **NP-hard** if A polynomial-time reduces to B for every language A in NP (harder than or in NP)
- B is **NP-complete** if B is NP-hard and in NP (the hardest NP problems, that are still in NP)
- NP != DTIME(2^n)
