# Modules in programming languages

I want to understand this because it is very important.

## What are modules?

*   **Is there a point?**

*   **Can any aspect of a module design (at the PL design level)
    encourage loose coupling in practice?**



## Packages

*   **Can a single system serve both modules and packages well?**

    The distinction I'm making here is that packages indicate
    maintenance responsibility. A package may contain a bunch of
    modules.

    When dherman was designing JS modules, someone asked about modules
    vs. packages. I couldn't tell what they were even talking about at
    the time, and I thought dherman didn't either. Of course anyone who
    had used npm a bunch would've understood.



## Dynamic languages

*   **Why do JS modules share bindings rather than values?**

*   **What exactly do Python modules share?**

*   **What other designs are there?**


## Static languages

### from "Modules Matter Most"

by Bob Harper, http://macqueenfest.cs.uchicago.edu/slides/harper.pdf

Some of these are questions about the ML module system, and some are
questions about the broader field and its terms and notation. I tried
splitting them into two categories but it seemed counterproductive.

*   (slide 6) **What does "a (partially) interpreted type" mean?

*   (slide 6) **Obviously this is something like Haskell type classes.
    But I thought those were basically invented for Haskell. What about
    Haskell type classes was novel in 1998? How does this design differ,
    and what are the tradeoffs?**

*   (slide 6) **OK, wait a minute, I've seen type classes before;
    they're predicates on types, with a standard decision procedure ~that
    is very simple,~ based on explicitly written `instance`s. What are
    signatures exactly? They do not seem to be predicates on types. Are
    they? Or predicates on modules?**

    They are predicates on structures.

*   (slide 6) **What's the decision procedure? (I.e. How does ML know if
    a given structure implements a given signature?)**

    I think it's like unification.

    At the shallow end of the pool, the elements of a signature are
    declarations of `val`s and `type`s and `structure`s, and ML can just
    check if the structure actually has matching things in it with the
    expected names.

    It gets a bit more complicated, because the elements of a signature
    are not all totally indepedent. They're related, like how in
    `signature DICT` (slide 10) you might have `type 'a t` and `val
    lookup : 'a t -> ...` so that the type of `lookup` involves the type
    `t`, and ML has to check that this relationship holds in the
    signature it's checking.

*   (slide 7) **Why is this slide entitled "Matching"? This seems like
    Go: types implicitly implement interfaces. Is there a difference?**

*   (slide 7) **Are instances of signatures inferred in ML? That is, can
    ML figure out that there's an implementation of `ORDERED` for `t =
    Nat`? How?**

*   (slide 7) **The example includes `datatype t = ...` which to me
    seems like a concrete detail. Can the `val` parts of a signature
    also be concrete?**

*   (slide 8) **How does scoping work? Are the "methods" of a signature
    values in the global namespace? How do you implement them for a
    particular type? Do you need to (and can you) "import" the methods
    separately from the signature?**

*   (slide 8) **ML has a rather bottom-up feel to it: scoping is only
    `rec` in places where you explicitly ask for it. How do modules
    interact with that?**

*   (slide 9) **What is "fibration"?**

    It must be the word for being able to define a signature as a
    restriction of another to particular implementation details.

*   (slide 9) **Is this related to Haskell type families?**

*   (slide 9) **Can you use `where` to filter on other implementation
    details, beside types?**

*   (slide 9) **What's an example of this being useful?**

*   (slide 10) **Whoa, how does this work? How is the instance of
    `ORDERED` inferred when the system is inferring an implementation of
    `DICT` for a particular type, say `t = Map`?**

*   (slide 10) **What is meant by "hierarchies"? I.e. what part of this
    example is alleged to be a Σ type?

    Oh. Um, it's referring to how the `DICT` signature has a member
    `Key` that is a structure constrained by another signature,
    `ORDERED`.

*   (slide 10) **And what are "Σ types"? The same thing as sum types in
    other contexts? I think of Rust `enum` and `dyn` types as sum
    types. Signatures do not seem to be quite the same thing; maybe I
    just haven't understood the terminology.**

    I don't think the "are" in "Hierarchies are Σ types" was meant so
    precisely; I think it means you get something *like* sum types but
    *for modules* rather than for values.

*   (slide 11) **Same deal. What is a "Π type"? The same thing as
    product types? I think of tuples as product types.**

*   (slide 11) **This about sum and product types makes signatures seem
    a lot like types. It makes me wonder what would have happened if
    similar syntax had been used.**

*   (slide 11) **It seems functor parameters are... whatever it is
    signatures predicate?**

    Yes, the argument you pass to a functor is a structure, as is the
    return type.

*   (slide 12) **What is coherence here? The Wikipedia [disambiguation
    page for "coherence"](https://en.wikipedia.org/wiki/Coherence)
    offers a few ideas for what it can mean in computer science, but
    those do not seem to be the relevant sense here. Is this related to
    Rust's "coherence rules"?**

*   (slide 12) **What distinction is being made here between *a priori* and
    *a posteriori* imposition of coherence?**

*   (slide 12) **What does "factor out the underlying set" mean?**

*   (slide 13) **Generativity: what are the tradeoffs? Why did OCaml
    choose the other way?**

*   (slide 15) **Here we go. What the heck are we even talking about on
    this slide? What are "kinds"? What are "constructors"? How is `bool`
    anything other than just a type?**

    Note that slide 19 has some examples of this syntax that might help
    explain it.

*   (slide 15) **What does "definitionally equivalent" mean?**

*   (slide 16) **I can't make any sense of this either. What do all
    these alternatives correspond to in terms of actual ML syntax? Like,
    maybe `[[K]]` corresponds to actual `sig ... end` syntax in ML, and `{ σ }`
    refers to some kind of expression using functor-call syntax?**

*   (slide 16) **"...a computation with effects yielding a module"? Now
    you're pulling my leg, right? What kind of computation? What kind of
    effects?**

*   (slide 16) **Wait a minute. Can a signature have `val` elements that
    rely on run-time computations with effects? Modules can contain data
    at run time? The usual examples you get are pretty static! How is
    this different from Haskell instances and Rust trait `impl`s? How is
    it different from classes with fields in languages like C++?**

*   **I basically don't understand anything from here on out. WTF.**

*   (slide 23) **Is this a real feature and what does it do? What's the
    rationale for using `data` as the keyword here? What on earth does
    `data structure` mean here? That has literally nothing to do with
    data structures, yes?**

*   (slide 24) **OK, what are *T* and st here? What is the implicit type
    of α?**

*   (slide 24) **Is... is this meant to be some kind of axiomatic
    description of NAT, where `(T.st)` represents "zero", `(T.st ->
    T.st) represents "successor", and that last mostrosity is some kind
    of description of induction? If so, some axioms are missing,
    right?**


### ML modules, continued

*   **Are "module" and "structure" interchangeable terms?**

*   **How strong is the analogy "value : type :: structure : signature"?
    What statements are true of both? What's different?**

*   **Is there a language kind of like lambda calculus that operates on
    structures? Are functors lambdas?**

*   **How does all this fit in with modules' basic job of separating
    concerns and helping programmers focus on one small piece at a
    time? How are the two jobs related?**

*   **How do you use signatures in practice? Do you have to be inside
    the body of a functor to use them at all? Is there something
    lightweight that you can stick on any function, like Haskell's
    `Ord =>` ?**

*   **Do functors really only take one argument? Why? Can you have a
    curried functor?**

*   **Functors are parametric, right? (I.e. there's no specialization.)
    Does it matter?**

*   **Can signatures have optional features with default implementations
    (like Haskell typeclasses and Rust traits)? If not, is there a
    deep reason why not?**


### from <https://jozefg.bitbucket.io/posts/2015-01-08-modules.html>

*   **What's the difference between weak ascription (`:`,
    a.k.a. transparent annotation) and strong ascription (`:>`,
    a.k.a. opaque annotation)? Why support both?**
