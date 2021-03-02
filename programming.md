## Programming

### Exercises

*   **In any dictionary of the English language, consider the graph that
    connects each main entry to each word used in its definitions. What
    are the strongly connected components in this graph?**

*   **Implement a copy-on-write database.**


### Databases

*   **What are the advantage of columnar data storage?**


### ELF

*   **Suppose `program` links with `mylib1.so` and `other.so`, which
    links against `mylib2.so`. Assume both `mylib` versions export some
    of the same symbols. When we run `program`, what happens? Do we get
    accidental interposition?**

    This is a question about the behavior of the dynamic linker,
    `ld-linux.so`.

    The only documentation I know of for this whole process is "How To
    Write Shared Libraries" by Ulrich Drepper,
    <https://software.intel.com/sites/default/files/m/a/1/e/dsohowto.pdf>.

    In this case:

    All four are loaded: `program`, `mylib1.so`, `other.so`, `mylib2.so`
    in that order. Then symbol references are resolved; I believe for
    each symbol reference, `ld-linux.so` probes the three libraries in
    order until it finds a definition.

    This means that, yes, we do get accidental interposition. Don't do
    this.

*   **Can I use ELF relocations to do arithmetic at link time?**

    No, it isn't nearly that fancy.

    But also, jimb writes: "You are thinking about ELF as a language
    that the linker interprets, and assuming it must be specified, but I
    think it is just barely even documented everything a Linux linker is
    expected to find and process"

    DWARF, on the other hand...


### Very specific questions about particular programming languages

*   **In DTrace's D language, is division banned within predicates?**

    I guess you probably have to parenthesize...


### Loop invariants

https://alastairreid.github.io/RelatedWork/papers/tuerk:vstte:2010/

*   **What are some examples of how this applies to loops?**

    Here's some code that passes the Dafny verifier:

    ```dafny
    function expt(b: nat, e: nat): (c: nat)
      requires b != 0 || e != 0
    {
      if e == 0 then 1
      else if b == 0 then 0
      else b * expt(b, e - 1)
    }

    method pow(b: nat, e: nat) returns (z: nat)
      requires b != 0 && e != 0
      ensures z == expt(b, e)
    {
      var x := b;
      var y := e;
      z := 1;
      while y > 0
        invariant x != 0
        invariant z * expt(x, y) == expt(b, e)
      {
        while y % 2 == 0
          invariant x != 0 && y > 0
          invariant z * expt(x, y) == expt(b, e)
          decreases y - 1
        {
          doubling_lemma(x, y);
          x := x * x;
          y := y / 2;
        }
        z := z * x;
        y := y - 1;
      }
    }

    // x² = x * x.
    lemma x_squared_lemma(x: nat)
      ensures expt(x, 2) == x * x
    {
      assert expt(x, 1) == x;
      assert expt(x, 2) == x * expt(x, 1);
    }

    // If y is even, then x^y == (x * x) ^ (y/2).
    lemma doubling_lemma(x: nat, y: nat)
      requires x != 0 && y != 0 && y % 2 == 0
      ensures expt(x * x, y / 2) == expt(x, y)
    {
      if y == 2 {
        x_squared_lemma(x);
        assert expt(x * x, 1) == x * x == expt(x, 2);
      } else {
        doubling_lemma(x, y - 2);
      }
    }
    ```


### Algebraic effects

https://www.microsoft.com/en-us/research/wp-content/uploads/2016/08/algeff-tr-2016-v3.pdf

*   **What is the syntax here?**

    `effect name { ...primitive_operations... }` defines an effect.

    The primitive operations are a suite of things a function can do under this effect
    in addition to mere computation.

    Each primitive operation has typed parameters and a return type,
    like a function.

    `handle (action) { ...cases... }` is `dynamic-wind` for setting up
    effect handlers.  It calls a function, *action*, with no arguments,
    and for the dynamic extent of the call, some effect handlers are
    installed.

    Each of the `cases` matches `pattern -> expression` where *expression*
    may contain the special expression `resume(expr)`.

    It is syntactic sugar for `(handler { ...cases... })(action)`

    `handler (param0, ...) { ...cases... }` evaluates to a function that
    takes one argument, an action function, and calls it with some effect handlers.
    It first evaluates the optional `params`, then

    In a handler, each case matches `pattern -> expression`, where *expression*
    may contain the special expression `resume(args, resume_expr)`.

#### Transferring knowledge from Koka to Unison

*   **Implement exceptions using effects.**

*   **Implement a single-variable state system using effects.**

*   **Implement a multiple-variable mutable data store using effects.**

*   **Is it possible to implement something like the ST monad using effects?**

*   **Implement `yield` and generators using effects.**

*   **Implement `amb` using effects.**

*   **Implement `async`/`await` using effects.**

*   **Implement a combinatorial parser library using effects.**


### Unison

<https://www.youtube.com/watch?v=gCWtkvDQ2ZI>

*   **Generally speaking, how do we do all the things source files do
    for us in normal programming languages?**

    -   How do we store comments and whitespace?

    -   How are argument names stored (since the presentation
        specifically points out that the hashing algorithm normalizes
        them away)?

    -   How do we collect related code into bundles, for convenient
        learning and editing, like classes or modules, or files and
        directories?

    -   How do we answer questions like "who wrote this" or "is this the
        official current version of this library"?

    -   How do we mark some tests as the official tests for the library
        you're working on?

    The ability to search is touted, but `.u` files are called "scratch"
    files and are apparently considered stricly temporary. Whatever the
    plan is for comments, they don't go with your code!

*   **Are there lambdas? If so, are they stored separately from the
    containing function?**

*   **How do we do mutual recursion if every function contains a hash of
    its dependencies?**

*   **Is this stuff typed?**

    Yes, there's a type system.

*   **It says here, "No builds". But don't you have to rebuild the
    entire downstream world every time you change anything, exactly like
    Nix? Same for testing.**

*   **The type is stored separately from the AST. Why?**

*   **Is the object store implicitly trusted? For example, can malicious
    data in the store break the type system?**

    "No one else needs to parse or type-check `factorial` either" makes
    it sounds like these received judgments about the code are
    implicitly trusted.

    You're requesting, downloding, and running code from a peer. You
    presumably trust them to *some* extent. But still.

*   **Is there a back edge from the object store to sources?**

*   **At 25:00, this view of dependency conflicts is a lot like
    Nix's. Does anyone agree with this? Isn't the use of old versions
    likely to be accidental and bad? Isn't this dodging the issue?**

*   **At 29:00, this suggests providing an upgrade path for old versions
    of types. Is there actually a system for that? It seems like if you
    find upgrades by querying and searching the universe, you could have
    diamonds in that upgrade graph—competing paths. Also it's not clear
    you want to trust all that code.**

*   **Does having a `Node` value imply you have full control over the
    referenced node? Can you cancel computation?**

*   **But isn't it actually pretty easy to make things transparent?
    Isn't the real challenge giving things the right amount of
    transparency, and letting users say useful stuff, like "run this
    closer to the data"? For example, is `spawn` actually good? How does
    the system know whether or not to run that code elsewhere? Isn't
    guessing that correctly essential to performance?**

*   **Abilities are Unison's effect system. Are abilities also like
    typeclasses/traits/interfaces? Is there something else like that
    instead?**

*   **Do abilities have associated types?**



### Lean

*   **What is the difference between `let` and `have`?
    The following example contains a `let` clause and a `have` clause.
    Changing `let` to `have` works fine, with no apparent effect.
    Changing `have` to `let` breaks the type inference. Why?**

    ```lean
    example (p q r : Prop) : p ∧ (q ∨ r) → (p ∧ q) ∨ (p ∧ r) :=
    begin
      intro h,
      let hp : p := h.left,
      have hqr : q ∨ r := h.right,
      show (p ∧ q) ∨ (p ∧ r),
      cases hqr with hq hr,
        exact or.inl ⟨hp, hq⟩,
      exact or.inr ⟨hp, hr⟩
    end
    ```



### Fuzzing / Property Testing

#### QuickCheck: A Lightweight Tool for Random Testing of Haskell Programs

http://www.eecs.northwestern.edu/~robby/courses/395-495-2009-fall/quick.pdf

*   **What does this mean: "Random testing is especially suitable for
    functional programs because properties can be stated at a fine
    grain."?**

    It means that the parts of programs, even at small scale, are
    effect-free and implicit-context-free. They can be called many
    times, in the same process, no worries; and the input *is* the test
    case.



#### Skeletal Program Enumeration for Rigorous Compiler Testing

Qirun Zhang, Chengnian Sun, Zhendong Su. UC Davis.

https://arxiv.org/pdf/1610.03148.pdf

*   **How does this paper answer the question it poses of "how to
    effectively generate 'good' test programs"?**



### Concurrency and specifications

#### http://drops.dagstuhl.de/opus/volltexte/2018/10088/pdf/LIPIcs-OPODIS-2018-28.pdf

*   **What is an execution trace?**

    It's a collection of individual processes' traces: what they did,
    how they interacted with the object we're talking about. Oh, the
    paper actually kind of answers this on page 3, or at least I was
    able to figure it out.

*   **Why does this paper present the pictures on page 2?**

    I don't know. They are not full execution traces, as they do not see
    into `count`, but that's OK because we're talking about how to
    specify the interface of `count` with its users. So there's a notion
    of execution traces that focuses on a particular "object".

*   **How significant is it that the model assumes a global clock? Isn't
    that unrealistic? How could you record a trace to check against the
    model?**

    The axioms do much to weaken the clock.

*   **What exactly is the transformation from a sequential specification
    to a linearizable concurrent one?**

    I think: a concurrent trace is OK if you can, for each call to the
    object, pick a timestamp *t* within its range (i.e. resolve every
    overlap to a single consistent order), then just merge all the
    traces to a single trace, and that trace is OK under the sequential
    specification.

*   **Are the axioms in this talk about traces, or are they
    "hyperproperties" about specifications generally?**

    They are hyperproperties. Only axiom 1 is a property of individual
    traces.

*   **Is it really right to argue: "well, no program can have exactly X
    set of traces, therefore X is a bad set of traces and we can just
    rule it out"?**

    ???

*   **Do this paper's arguments and the field of memory model
    specification have implications for each other?**

    I think memory models satisfy the axioms.

*   **Wait, why are we *not* talking about happens-before relations and
    interprocess messages and such here?**

    The specification of a concurrent object, in this view, abstracts
    away from particular specification techniques, of which “...is an
    allowable trace iff there exists a happens-before relation such
    that...” is one.

*   **What are objects in this paper? What are processes?**

    Processes are single-threaded processes. Objects are distributed
    systems. But it's more nuanced than that.

    An object might be a database, together with the client-side
    libraries that can be used to access the database. Processes are
    running programs that use the database.

    Or, an object could be a queue, and the processes are programs that
    produce and consume events.

    Or, an object could be "memory", and the processes are CPUs. In this
    example, note that by "memory" I mean the abstraction, including RAM
    *and* on-CPU caches, *and* all the complicated machinery inside the
    CPU that presents this abstraction to the running software.

    Now you see the nuance, maybe. A process is a single-threaded,
    alternating conversation between the object and whatever program it
    is that's trying to use the object. It's as though the object has
    one customer-service representative on each process that can act
    only when the customer asks it for something. The customer talks
    first.

    A specification is a set of rules that apply to the object's
    behavior. The axioms say that a specification can't constrain the
    behavior of the programs (customers).

*   **The axioms are things that no program can violate?! What...?**
    Yes, the axioms all represent commonsense facts about computing.
    The main contribution of this talk, apparently, is that a layer of
    common sense actually reveals some structure and tells us things
    about standard classes like linearizability.

    The paper expresses everything in the following list more
    concisely. These are my grasping explanations to myself.

    1.  Alternating: Each process observes a call first, then a return,
        then a call...

        This rules out seeing a return first, without having called an
        object (clearly wrong).

        It means if you want to model something reentrant, like signal
        handlers, or generators/coroutines, you have to figure out a way
        to model them; the system isn't able to model them directly.

    2.  Prefix-closed: This means that the specification may not say
        "and at this point, we infallibly continue; the network Can't go
        down" (a clearly wrong thing for a specification to want to say).

    3.  Non-empty: The kind of specification ruled out by this axiom is
        the kind where nothing is allowed—i.e. not only is it illegal to
        call the object, it's also illegal *not* to call the object.

    4.  Receptive: The specification can't constrain the behavior of the
        client program. Any process that doesn't already have a call
        pending may call the object with any value whatever.

        (This is funny because the constraints on callers are kind of a
        natural thing to want to specify: preconditions, and the ability
        to *drop* an object, after which it's invalid to try to access
        it again, are examples. See next question.)

    5.  Total: The spec always permits some return value for every call
        *at every time* (?).

        (This is extremely surprising, as it seems to prohibit specs
        that require blocking until another process does something.  For
        example, a simple blocking queue can't be specified. See next
        question.)

    6.  Commuting invocations: This weakens the global clock a bit.

    7.  Commuting responses: Same.

    8.  Closed under expansion: More along the same lines. Imagine two
        universes that are the same except that a call happens at one
        time in universe A and a bit earlier in universe B. The spec
        must permit all the same subsequent behavior in universe B that
        it permits in universe A (although it may allow more
        behaviors). Earlier is better. On the other hand a return can
        always be delayed.

*   **Can axioms 4 and 5 really be justified?**

    It seems like both axioms prevent the specification of objects we
    would like to specify.

    It seems like you can work around them by adding an "error" return
    value that the object uses to report e.g. precondition failures.
    But if you can easily work around a restriction, why bother having
    it?

*   **How are axioms 4 and 5 related?**

*   **How are axioms 4 and 8 related?**

*   **Could we simplify this?**

*   **The last axiom is asymmetric. Is there a commonsense example that
    illustrates the asymmetry?**


### Miscellaneous

*   **What is the deal with "signaling NaNs"?**

    About half of the NaNspace is signaling NaNs; there's a bit.

    If you're doing NaN-boxing, like `JS::Value`, you'll always check
    before doing math, so the behavior on NaNs doesn't matter.

    To check: SM's canonical NaN Value is quiet (not signaling).

    The spec says that most operations fire an exception when applied to
    a signaling NaN. This means special implementation-defined behavior:
    an interrupt or POSIX signal. The default behavior is for the
    operation to return a quiet NaN (the same, I suppose, as if the
    input signaling NaN or NaNs had been quiet to begin with).

    GNU libc has a very special implementation-specific function that
    causes operations on signaling NaNs to fire a POSIX `SIGFPE` signal.
    It's "not very useful" according to the manual, but you can throw a
    C++ exception (yow). MSVC has something similar.

    All of this behavior should be strongly deprecated, so that adding
    numbers generally has defined behavior.

*   **What's a good textbook about the security of cryptographic
    protocols?**
