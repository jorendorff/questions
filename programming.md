## Programming

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
