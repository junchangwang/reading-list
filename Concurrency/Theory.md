
#### Theory

* **[HFP02]** Timothy L. Harris, Keir Fraser, and Ian A. Pratt. "A Practical Multi-Word Compare-and-Swap Operation". In Lecture Notes in Computer Science'02.
    * This paper presents CAS2, a primitive that atomically compares two distinct memory locations and sets a new value into one of them. The basic idea is to first use a pointer to the descriptor, which holds the information of the CAS2 operation, to occupy the destination location, and then forces all related processes to help perform the CAS2 operation. This paper also provides an implementation of CASN based on the proposed CAS2.
* **[TP14]** Shahar Timnet, Erez Petrank. "A Practical Wait-Free Simulation for Lock-Free Data Structures". In PPoPP'14.

------

#### Linearizability and variations

* **[HW90]** Maurice Herlihy, and Jeannette Wing. "Linearizability: A Correctness Condition for Concurrent Objects". In TPLS'90.
* **[KH14]** Alex Kogan, and Maurice Herlihy. "The Future(s) of Shared Data Structures". In PODC'14.
* **[HHL06]** Steve Heller, Maurice Herlihy, Victor Luchangco, et al. "A lazy concurrent list-based set algorithm". In 2006.

------
#### Memory Consistency Models 

- [Memory order specification in C++](https://en.cppreference.com/w/cpp/atomic/memory_order)
- [GCC built-in atomic operations](https://gcc.gnu.org/onlinedocs/gcc/_005f_005fatomic-Builtins.html)
- **[BOE04]** Hans-J. Boehm. "Threads Cannot Be Implemented As a Library".
- **[MPA05]** Jeremy Manson, William Pugh, and Sarita V. Adve. "The Jave Memory Model". In POPL'05.
- **[BA08]** Hans-J. Boehm and Sarita V. Adve. "Foundations of the C++ Concurrency Memory Model". In PLDI'08.

------
#### Universal construction

* **[HER91]** Maurice Herlihy. "Wait-free synchronization". In TPLS'91.
* **[FK13]** Panagiota Fatourou, and Nikolaos D. Kallimanis. "Highly-Efficient Wait-free Synchronization". In TCS'13.
* **[CRF20]** Andreia Correia, Pedro Ramalhete, and Pascal Felber. "A Wait-Free Universal Construction for Large Objects". In PPoPP'20.

------
#### Deferred processing (read-mostly data structures)

* **[RCU1]** Paul E. McKenney. [Structured Deferral: Synchronization via Procrastination](https://queue.acm.org/detail.cfm?id=2488549)
* **[RCU2]** Prof. Kohler. [Notes on Read-Copy Update](http://read.seas.harvard.edu/~kohler/class/cs261-f11/rcu.html)
* **[MIC04]** Maged M. Michael. "Hazard Pointers: Safe Memory Reclamation for Lock-free Objects". In TPDS'04.
* **[MIC20]** Maged M. Michael. "Hazard Pointer Protection of Structures with Immutable Links [[presentation]](https://www.youtube.com/watch?v=HYCPN6Kp_Ao)". In PODC'20.
  * [Immutable data and react](https://www.youtube.com/watch?v=I7IdS-PbEgI)

------
#### Updates

* **[HIS10]** Danny Hendler, Itai Incze, Nir Shavit, and Moran Tzafrir. "Flat Combining and the Synchronization-Parallelism Tradeoff". In SPAA'10.
* **[BKM14]** Silas Boyd-Wickizer, M. Frans Kaashoek, Robert Morris, and Nickolai Zeldovich. "OpLog: a library for scaling update-heavy data structures". MIT Tech Report'14.

------
#### Memory reclamation

* **[WIC18]** Haosen Wen, Joseph Izraelevitz, Michael L. Scott, et al. "Interval-Based Memory Reclamation". In PPoPP'18. [[slides]](https://www.cs.rochester.edu/u/hwen5/papers/wen-ppopp-2018-slides.pdf).

------
#### Wait-free algorithms in practice

* **[AHS14]** Dan Alishtarh, Keren Censor-Hillel, and Nir Shavit. "Are lock-free concurrent algorithms practically wait-free?" In TOC'14.
* **[DG16]** Tudor David and Rachid Guerraoui. "Concurrent search data structures can be blocking and practically wait-free". In SPAA'16.

------
#### (Persistent) Transactional memory

* **[TM1]** [Prof. Fatahalian's slides in TM](http://cs149.stanford.edu/fall19/lecture/transactionalmem)
* **[HM93]** Maurice Herlihy, and J. Eliot B. Moss. "Transactional Memory: Architectural Support for Lock-Free Data Structures". In ISCA'93.
* **[CFR18]** Andreia Correia, Pascal Felber, and Pedro Ramalhete. "Romulus: Efficient Algorithms for Persistent Transactional Memory". In SPAA'18.
* **[RCF19]** Pedro Ramalhete, Andreia Correia, et al. "OneFile: A Wait-Free Persistent Transactional Memory. In DSN'19.

------
#### Work Stealing

* **[FLR98]** Matteo Frigo, Charles E. Leiserson, and Keith H. Randall. "The Implementation of the Cilk-5 Multithreaded Language". In PLDI'98

------
#### Other interesting algorithms

* **[HLS95]** Maurice Herlihy, Beng-Hong Lim, and Nir Shavit. "Scalable Concurrent Counting". In TCS'95.

------
#### Performance

* **[ABM18]** Maya Arbel-Raviv, Trevor Brown, and Adam Morrison. "Getting to the Root of Concurrent Binary Search Tree Performance". In ATC'18.
  * The authors focus on optimistic binary search trees and perform a detailed performance analysis of 10 state-of-the-art BSTs. This paper reveals that most performance anomalies are caused by cache under-utilization.

* **[GRA15]** Vincent Gramoli. "More than you ever wanted to know about synchronization". In PPoPP'15.
  * This paper presents an extensive comparison of widely-used synchronization techniques including locking, CoW, RCU, lock-free, and transactional memory. The conclusion is three-fold. (1) Lock-free algorithms, which typically utilize CAS, are generally more flexible for multi-core architectures, at the expense of greater complexity. (2) Hardware transactional memory offers more consistent performance than locks. (3) CoW and RCU are more sensitive to contention than other techniques, yet they help to achieve high performance under read-only workloads. [synchrobench](https://github.com/gramoli/synchrobench).

* **[DGT15]** Tudor David, Rachid Guerraoui, and Vasileios Trigonakis. "Asychronized Concurrency: The Secret to Scaling COncurrent Search Data Structures". In ASPLOS'15.
  * The authors performed an exhaustive evaluation of concurrent search data structures including linked lists, hash tables, skip lists, and BST that utilize synchronization techniques including locking, optimistic locking, and lock-free. This paper reveals some interesting findings. For example, An algorithm is lock-based or lock-free does not have a major effect on scalability. The use of hardware TM makes little difference. The number of cache misses per operation is directly correlated to the scalability and performance of an algorithm. [ASCYLIB](https://github.com/LPD-EPFL/ASCYLIB)

------
#### Memory models in programming

- [GCC memory models](https://gcc.gnu.org/wiki/Atomic/GCCMM/AtomicSync)
- [Memory Model Aware Atomic Operations in GCC](https://gcc.gnu.org/onlinedocs/gcc/_005f_005fatomic-Builtins.html)

------
#### Useful tools

- **[Spin]** [A tool that can be used for the formal verification of applications and protocols](http://spinroot.com/spin/whatispin.html)
- **[diy/litmus/herd]** [A verification tool that is aware of memory models](http://diy.inria.fr/)



