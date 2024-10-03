
#### General Structure

* **[EFR10, NB-BST]** Faith Ellen, Panagiota Fatourou, Eric Ruppert, and Franck van Breugel. "Non-blocking Binary Search Trees". In PODC'10.
  * This paper describes the first non-blocking BST using single-word CASes. The core idea is to expand each tree node with a flag field, which works as a lock and serializes concurrent updates. To guarantee the property of non-blocking, each update operation stores the necessary information required by other threads to help complete the operation in an object, and saves a reference to the object also in the flag field. The implementation is leaf-oriented and unbalanced.

* **[FPR19, PNB-BST]** Panagiota Fatourou, Elias Papavasileiou, and Eric Ruppert. "Persistent Non-Blocking Binary Search Trees Supporting Wait-Free Range Queries". In SPAA'19.
  * This paper presents an extension of NB-BST that supports wait-free RQs, by leveraging the MVCC technique (the authors called it "persistent" in the paper). Specifically, PNB-BST maintains a global timestamp variable which is read by each operation and incremented by each RQ. PNB-BST prefers RQ operations, such that RQs are wait-free. In contrast, the Find operation is lock-free unless it first invokes an RQ which is much more expensive. Update operations are lock-free, and pro-actively give up their execution if concurrent RQs are in progress.
 
* **[BCC10]** Nathan G. Bronson, Jared Casper, Hassan Chafi, and Kunle Olukotun. "A Practical Concurrent Binary Search Tree". In PPoPP'10.
  * This paper presents a concurrent, relaxed, balance AVL tree, based on optimistic hand-over-hand Locking mechanisms. The authors introduced a series of novel techniques to alleviate the synchronization overhead in coordinating concurrent update and lookup operations. In particular, *partially external tree* noticeably simplifies delete operations by leaving a routing node in the tree if it has two children. Furthermore, updates have fixed write sets that are known in advance. The authors also presented an atomic clone operation based on COW. Note that a lookup operation could be blocked by an update operation, and may start over when the update operation is changing the links the lookup operation is traversing. I didn't verify if there are any livelock issues, but it seems suspicious.

* **[BER14]** Trevor Brown, Faith Ellen, and Eric Ruppert. "A General Technique for Non-blocking Trees". In PPoPP'14.
  * This paper presents a general technique for building non-blocking implementations of search trees. The core technique is an update template which, by using a multi-word generalization of LL/SC/VL primitives, can atomically update a contiguous portion of down-trees where pointers are directed from parents to children.

* **[AA14, Citrus]** Maya Arbel and Hagit Attiya. "Concurrent Updates with RCU: Search Tree as an Example". In PODC'14.
  * This paper presents CITRUS, an unbalanced, internal search tree that uses fine-grained locking to serialize concurrent updates to nodes. One notable issue of fine-grained based data structures is that operations touching several items cannot get consistent views of the data structures. CITRUS solves this problem by using RCU to synchronize concurrent reads and updates. The price is that the update operation of CITRUS may invoke synchronize_rcu while holding locks. 
  * [Paul's comments](https://lwn.net/Articles/619355/)

* **[WSJ18 LFCA]** Kjell Winblad, Konstantinos Sagonas, and Bengt Jonsson. "Lock-free Contention Adapting Search Trees". In SPAA'18.
  * This paper presents LFCA, a lock-free external search tree that can dynamically adjust the granularity of its base nodes (The base nodes are implemented by using Treaps in the paper). Similar to other external, Java-oriented search trees, each of LFCA's update operations replaces the target base node with a new node in which the update has been applied. To be lock-free, the rangeScan() operation adopts a helping mechanism that allows concurrent update operations to help it to complete. Similarly, update operations can *discard* or help suspended Join() operations.

* **[BBB20 KiWi]** Dmitry Basin, Edward Bortnikov, and Anastasia Braginsky, et. al. "KiWi: A Key-value Map for Scalable Real-Time Analytics". In PPoPP'17.
  * This paper presents KiWi, a high-performance KV map, which, in some sense, is similar to a B+-tree in architecture. In addition to standard operations, KiWi supports wait-free scans and lock-free rebalancing operations which can not only merge and split chunks (leaf nodes) but also reorganize the data layout of each chunk to facilitate scan operations. KiWi employs a global PSA array and a group of per-chunk PPA arrays to allow scan operations and put operations to respectively broadcast their existence. Therefore, scan operations can help suspended update operations set their timestamp values, dramatically simplifying the synchronization mechanisms. Moreover, KiWi utilizes MVCC to enable wait-free scans, and uses helping mechanisms to resolve conflicts in a single chunk. The global timestamp is maintained by Scans, instead of Updates.

* **[BP12]** Anastasia Braginsky and Erez Petrank. "A Lock-Free B+ Tree". In SPAA'12.
  * This paper presents the first lock-free B+-tree. The building block is a special data structure named chunk, which is also from Petrank's group. A chunk can maintain lower- and upper-bound on the number of its elements, and can be split and joined in a lock-free manner. The search paths are manipulated in a lock-free way by using notable non-blocking techniques (mainly, helping). The algorithm has restrictions. Key repetition is not allowed, and the key and the data must fit into a single (double) word. Nevertheless, this is a very complex yet elegant design.
  


* **Related projects**: [ASCYLIB](https://github.com/LPD-EPFL/ASCYLIB), [synchrobench](https://github.com/gramoli/synchrobench).


#### Trees in DBMS

* **[GRA10]** Goetz Graefe "A Survey of B-Tree Locking Techniques". In TODS'10.
  * This is a survey on concurrent trees that can reblance. Graefe presents a comprehensive background and primilinaries, a very illustrative discussion on the differences between locks and latches in DBMS. Graefe then discusses in detail how a B-Tree's internal structure should be correctly protected by using latches, and how its logical contents can be protected by using locks.

* **BTree in real DBMS** [PostgreSQL](https://www.youtube.com/watch?v=VO5xmj-6aT4)

* **[SBL20, P-Tree]** Yihan Sun, Guy E. Blelloch, Wan Shen Lim, and Andrew Pavlo. On Supporting Efficient Snapshot Isolation for
 Hybrid Workloads with Multi-Versioned Indexes. In VLDB'20.
  * https://www.youtube.com/watch?v=532ONpw2pGk
