#### Linearizable traversal

* **[AB18, EBR]** Maya Arbel-Raviv and Trevor Brown. "Harnessing Epoch-based Reclamation for Efficient Range Queries". In PPoPP'18.
    - This paper presents a technique to augment data structures to support linearizable range query operations. The core of EBR is a timestamping mechanism that linearizes an RQ and splits objects into two sets: (1) old objects that the RQ needs to visit, and (2) new objects that does not. The epoch-based memory reclamation is exploited to buffer deleted objects while the RQ is in progress. The authors demonstrated three flavors that use locks, TM, and lock-free techniques, respectively, and applied the technique to a variety of concurrent data structures.

* **[NHP21]** Jacob Nelson, Ahmed Hassan, and Roberto Palmieri. "Bundled References: An Abstraction for Highly-Concurrent Linearizable Range Queries." In PPoPP'21.
    - This paper presents *bundled references*, a new building block to provide linearizable range query operations. The core idea behind this technique is to replace the *next* field of each node with a list of its historical records, such that a range query operation can find and follow the correct--rather than the newest--record. This is the technique researchers in the database community refer to as Multi-Version Concurrency Control (MVCC). Note that an update operation can block a concurrent RQ operation (due to PENDING_TS), and that an update operation can block another concurrent update operation working on the same node (due to the lock in each node).
    - [Jacob's presentation](https://www.youtube.com/watch?v=BoX8QNgeGk0)
    - Bundling Linked Data Structures for Linearizable Range Queries. In PPoPP'22 from the same authors.


* **[WBB'21]** Yuanhao Wei, Naama Ben-David, Guy E. Blelloch, Panagiota Fatourou, Eric Ruppert, and Yihan Sun. "Constant-Time Snapshots with Applications to Concurrent Data Structures". In PPoPP'21.
    - This paper presents an effective approach for taking snapshots of CAS-based linked concurrent data structures. The core idea is that only taking-snapshot operations (instead of updates) could increment the global timestamp, and that concurrent read and update operations can help a successful CAS operation to set the timestamp of the latest version node that was just inserted by the CAS operations, which noticeably simplifies the process of atomically updating the data structure and inserting a new version node. Furthermore, the authors present optimizations such as avoiding indirection and removing redundant version nodes.
    - [Yuanhao's presentation](https://www.youtube.com/watch?v=k4MoXSuKKAE).
---

#### Atomic updates

* **[MSF15, RLU]** Alexander Matveev, Nir Shavit, Pascal Felber and Patrick Marlier. "Read-Log-Update: A Lightweight Synchronization Mechanism for Concurrent Programming". In SOSP'15.
  - This paper presents a synchronization mechanism that combines in spirit RCU and software transactional memory, which, respectively, allows readers to run concurrently with writers and allows atomic updates. The idea behind the RLU is a clock-based logging mechanism that effectively splits concurrent operations into two sets: (1) old operations that read the old object copies, and (2) new operations that ``steal'' new copies by accessing the per-thread write-log of the writer. Note that RLU uses a fine-grained locking mechanism and that the deferring flavor makes other operations non-linearizable.
  - [Alexander's presentation](https://www.youtube.com/watch?v=at9cxc3JTkY)
  - [Paul's comments on RLU](https://lwn.net/Articles/667720/)

* **[KMK19, MV-RLU]** Jaeho Kim, AjitMathew, Sanidhya Kashyap, Madhava K. Ramanathan, and Changwoo Min. "MV-RLU: Scaling RLU with Multi-Versioning". In ASPLOS'19.
  - The authors solved an update-side scalability issue in RLU, by combining RLU with MVCC to defer writing logs back to the shared data structure. The authors also discussed techniques including high-performance hardware timestamp, concurrent GC. Section 2.4 contains a nice discussion on the differences between snapshot isolation and serializability. Even though MV-RLU brings a performance benefit, an update operation can block others trying to access the same nodes.

* **[RS20]** Matthew Rodriguez and Michael Spear. "Optimizing Linearizable Bulk Operations on Data Structures". In ICPP'20.

---

#### Multi-Version Concurrency Control

* **[WAL17]** Yingjun Wu, Joy Arulraj, Jiexi Lin, Ran Xian, and Andrew Pavlo. "An Empirical Evaluation of In-Memory Multi-Version Concurrency Control". In VLDB 2017.
    - The authors performed an extensive study of MVCC's key design decisions: concurrency control protocol, version storage, garbage collection, and index management.
    - [Andy's lecture](https://www.youtube.com/watch?v=1Od_SuOQshM)
* **[DSS89]** James R. Driscoll, Neil Sarnak, Daniel D. Sleator, and Robert E. Tarjan. "Making Data Structure Persistent". In JCSS'89.

---

#### Snapshots
* **[PT13, snap-collector]** Erez Petrank and Shahar Timnat. "Lock-Free Data-Structure Iterators". In DISC'13.
  - This paper presents a technique for adding a *linearizable* wait-free iterator to a wait-free or a lock-free set data structure. To maintain linearizability, the algorithm allows threads, when necessary, to report operations (e.g., insert and delete) executed by themselves and other threads (which is an elegant way to achieve linear.). To support multiple iterators, the data structure holds a pointer to a special object denoted as *snap-collector*, which maintains the ``copied'' data structure and the reports to fix it. Note that the whole data structure must be first copied before an algorithm can iterate over it.

* **[Jay05]** Prasad Jayanti. "An Optimal Multi-Writer Snapshot Algorithm". In STOC'05.
    - This paper presents an m-component-n-process snapshot algorithm that uses the CAS primitive to synchronize concurrent update operations, and uses a helping mechanism to cooperate concurrent scan operations. Note that an atomic snapshot object supports only two types of operations: Update and Scan.

