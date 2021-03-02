### Cryptography

-   One-way accumulators (1993):
    https://link.springer.com/content/pdf/10.1007%2F3-540-48285-7_24.pdf
-   Textbook on cryptography and network security:
    https://www.google.com/books/edition/Cryptography_and_Network_Security/SWbn3lBe2FcC?hl=en&gbpv=1
-   Elliptic curve cryptography (comes highly recommended from graydon):
    http://point-at-infinity.org/ecc/phrecc.txt (17 screens)


#### Physical uncloneable functions

-   Physical uncloneable functions: http://www.ieee-security.org/TC/SP2013/papers/4977a286.pdf
-   Public physical uncloneable functions: https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=6856138
-   PUFs for authentication: https://idus.us.es/bitstream/handle/11441/56512/Using%20Physical.pdf?sequence=1
-   PUFs for authentication: https://d1wqtxts1xzle7.cloudfront.net/30684881/Security_2c_20Privacy_20and_20Trust_20in_20Modern_20Data_20Management_20%28Data-Centric_20Systems_20and_20Applications%29-milanovic.pdf?1361917189=&response-content-disposition=inline%3B+filename%3DTrust_management.pdf&Expires=1613483002&Signature=GkL4WtvXpyAEamJslcvH~j2Cg3TziK~AW26yCh3d3zdhE3EYN8ghIg7WTLDPVIb6qsEcL30y9sSAGAHg0lX7leH17rKnQe4qQ08XQiGC~09iP2FCeXOHIAO2NnRGuTUuUfoLS9tPzI0PaHNbABFJKfTfgk-TEC-pGJdf1iBiU02khkAZccBulp9quGOU2jMsJGQsbku3Vq3FGOo6pRAMh-fZjm3K~W1OKvQp51Zcj30bHZO36kD~JjwVBt4yzt~NwI9~Rjc~VeEJ6IyF6MMkldO61AGLjm2rsNCHs~PVm7DVM4OK-yOLmDaN1sVW7wrVEV8mzrk07dShXxmmVdXMvw__&Key-Pair-Id=APKAJLOHF5GGSLRBV4ZA#page=143


### Programming languages

-   Algebraic effects:
    https://www.microsoft.com/en-us/research/wp-content/uploads/2016/08/algeff-tr-2016-v2.pdf

-   "Datalog and Recursive Query Processing", Green, Huang, Loo and
    Zhou. Foundations and Trends in Databases, 2012.
    http://blogs.evergreen.edu/sosw/files/2014/04/Green-Vol5-DBS-017.pdf
    Page 34.

### Programs to read

-   Stockfish: https://github.com/official-stockfish/Stockfish/blob/master/src/movepick.h


### Distributed systems

http://muratbuffalo.blogspot.com/2021/02/foundational-distributed-systems-papers.html

-   "A Brief Tour of FLP Impossibility", blog post
    https://www.the-paper-trail.org/post/2008-08-13-a-brief-tour-of-flp-impossibility/

    -   "Impossibility of Distributed Consensus with One Faulty Process", 1985. Fischer, Lynch, Paterson.
	    https://groups.csail.mit.edu/tds/papers/Lynch/jacm85.pdf

-   Distributed Sagas
    <https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/saga/saga>

    I honestly don't know how impressed I should be.

    -   Video: https://www.youtube.com/watch?v=0UTOLRTwOX0

-   "Feral Concurrency Control: An Empirical Investigation of Modern
    Application Integrity" (Bailis, Peter et al. 2015???. Rails.)

-   "Spanner: Google's globally-distributed database" (2012)

-   "Challenges to Adopting Stronger Consistency at Scale" (Facebook. 2015.)

-   "Sagas" (Garcia-Molina, Hector and Kenneth Salem. 1987. Long-lived transactions in a database.)


### Databases

-   Colyer on the ARIES paper
    "ARIES: A Transaction Recovery Method Supporting Fine-Granularity Locking and Partial Rollbacks Using Write-Ahead Logging"
	https://blog.acolyer.org/2016/01/08/aries/

-   CORFU: A Distributed Shared Log
    https://www.cs.utexas.edu/users/lorenzo/corsi/cs380d/papers/a10-balakrishnan.pdf

-   Kafka

-   LMDB (Lightning memory-mapped database)
    - The LMDB File Format https://blog.separateconcerns.com/2016-04-03-lmdb-format.html

-   the Google BigTable paper

-   Log-structured merge trees https://www.cs.umb.edu/~poneil/lsmtree.pdf
    http://www.benstopford.com/2015/02/14/log-structured-merge-trees/

-   NoFTL: http://www.vldb.org/pvldb/vol6/p1278-petrov.pdf

Pavlo talks about three kinds of database:

- in-place updates (VoltDB) - traditional table b-trees with write-ahead log

- copy-on-write (LMDB) - shadow copy of table on updates, no write-ahead log

- log-structured (RocksDB) - all writes appended to log, no table heap


### Virtualization and containers

-   Kubernetes: https://kubernetes.io/docs/tutorials/kubernetes-basics/


### Compilers

-   Video: MLIR tutorial: https://www.youtube.com/watch?v=Y4SvqTtOIDk


### Operating systems

-   Linux storage:
    https://upload.wikimedia.org/wikipedia/commons/f/fb/The_Linux_Storage_Stack_Diagram.svg


### Hardware

-   DDR4 SDRAM - Initialization, Training, and Calibration
    https://www.systemverilog.io/ddr4-initialization-and-calibration



### Grab bag

-   https://ideolalia.com/essays/two-concepts-of-legibility.html
-   https://www.ribbonfarm.com/2012/05/09/welcome-to-the-future-nauseous/
-   Openstack: https://www.openstack.org/software/
-   Interview with bmc: https://jamesmunns.com/podcast/006-bryan/

-   MCA/MCE in Xen: https://lists.xenproject.org/archives/html/xen-devel/2009-02/pdf47CGiwzq4V.pdf
-   What is this? https://twitter.com/kc8apf/status/1361891861871292420
-   rust-vmm project

-   Databases on New Hardware
    https://www.youtube.com/watch?v=YgCqS6AQG_E&list=PLSE8ODhjZXjasmrEd2_Yi1deeE360zv5O&index=25
    (watched to 48:00)

-   Intel flash translation layer:
    http://idke.ruc.edu.cn/people/dazhou/Papers/Intel-FTL.pdf

-   Decompilation for security analysis: https://www.usenix.org/system/files/conference/usenixsecurity13/sec13-paper_schwartz.pdf (17 pages)

-   Continuations: implementations: https://kavon.farvard.in/papers/pldi20-stacks.pdf

-   https://en.wikipedia.org/wiki/Multi-armed_bandit#Contextual_bandit

-   SSA: "Single-Pass Generation of Static Single-Assignment Form for
    Structured Languages". Brandis and Mössenböck, _ACM Transactions on
    Programming Languages and Systems_, 1994. (15 pages)
    http://www.cs.trinity.edu/~mlewis/CSCI3294-F01/Papers/p1684-brandis.pdf

- [x] What is the difference between a duck?
    https://www.google.com/books/edition/Scientific_Approaches_to_Consciousness/QehHAwAAQBAJ?hl=en&gbpv=1&dq=between+a+duck&pg=PA445&printsec=frontcover

    I read 4 pages of this 6-page chapter, all that's avaliable on Google Books. It was awful.

-   Certificate Transparency https://tools.ietf.org/html/rfc6962

-   Video: History of DNS. From the "Systems we love" thing bmc ran once.
    https://www.youtube.com/watch?v=ieYriPDSC7Q&list=PLpIELpJvZsvcZAyx4NruEccCVlNZGKODs&index=15
