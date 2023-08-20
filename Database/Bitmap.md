
#### Seed papers

* **[OQ97]** Patrick O'Neil and Dallan Quass. "Improved Query Performance with Variant Indexes". In SIGMOD'97.
  - This paper made a comprehensive comparison among different indexing schemes including Value-List, Projection, and Bit-Sliced-based bitmap indexes.

---

#### Cardinality

* **[WSS2008]** Kesheng Wu, Kurt Stockinger, and Arie Shoshani. "**Breaking the Curse of Cardinality on Bitmap Indexes**". in SSDBM'08.
  - The authors attempted to remove the limitation of building bitmap indexes on large-cardinality attributes by improving binning technique. The core is an interlayer data structure, named OrBiC (Order-preserving Bin-based Clustering), between a bitmap index and the underlying storage. OrBiC is fundamentally a projection of the whole attribute, but is reordered according to the bin numbers and preserving the relative order of values in each bin. By checking OrBiC, we can reduce the number of candidate checks, improving system performance. However, the memory footprint of OrBiC could be a limitation for large datasets. E.g., for a dataset with 10B entries, OrBiC consumes 8*10B = 80GB of storage space. There are some optimizations in the paper, but the memory consumption is still huge.

---

#### Coding Schemes

* **[CI98]** Chee-Yong Chan, and Yannis E. Ioannidis. "**Bitmap Index Design and Evaluation**". In SIGMOD'98.
    - In this paper, the authors examined the space-time tradeoff of various bitmap indices (e.g. range encoding). Moreover, based on a proposed framework, they identify (1) the time-optimal bitmap indices, (2) the space-optimal bitmap indices, (3) the bitmap index with the optimal space-time tradeoff, and (4) the time-optimal bitmap indices under a given disk-space constraint.

* **[CI99]** Chee-Yong Chan, and Yannis E. Ioannidis. "**An Efficient Bitmap Encoding Scheme for Selection Queries**". In SIGMOD'99.
    - This paper presents a detailed analysis of the performance of the existing two types of encoding schemes (equality- and range-encoded) with respect to various query operations (equality, (two-sided) range query, and membership query). The authors then propose a new encoding scheme (and four additional hybrid schemes), called *interval-encoding*, that takes almost half the space of range encoding and at the same time, guarantees at most a two-scan evaluation for any query.

---

#### Compression

* **[WLPS17]** Jianguo Wang, Chunbin Lin, Yannis Papakonstantinou, and Steven Swanson. "**An Experimental Study of Bitmap Compression vs. Inverted List Compression**". In SIGMOD'17.
    - Both bitmap compression and inverted list compression solve the same problem: how to store a collection of sorted integers with as few as possible bits and support query processing as fast as possible. The authors performed comprehensive evaluations to compare 9 bitmap compression methods (including WAH, CONCISE, BBC, and Roaring) and 12 inverted list compression methods (including VB, PforDelta, and SIMDBP) by using both synthetic and real-life datasets. They attempt to pick out the best compression method in terms of space overhead, decompression time, intersection time, and union time.

* **[WOS02, WAH]** Kesheng Wu, Ekow J. Otoo, and Arie Shoshani. "Compressing Bitmap Indexes for Faster Search Operations". In SSDBM'02.

* **[WOS05, WAH]** Kesheng Wu, Ekow J. Otoo, and Arie Shoshani. "Optimizing Bitmap Indices with Efficient Compression". In TODS'05.
* 
* **[WAB09, FastBit]** K Wu, S Ahern, E W Bethel, et al. "**FastBit: Interactively Searching Massive Data**". Lawrence Berkeley National Laboratory.

---

#### Binning

* **[KOU]** Nick Koudas. "**Space Efficient Bitmap Indexing**. In CIKM'00.
  - This seed paper presents the binning technique and highlights the key performance factors: the frequency of access of the attribute and the frequency of occurrence of the attribute value. This paper gives a formal problem definition and complexity analysis. Respect!

* **[RSW05]** Doron Rotem, Kurt Stockinger, and Kesheng Wu. "**Optimizing Candidate Check Costs for Bitmap Indices**". In CIKM'05.
    - This paper underlines that *candidate check*, the unavoidable access to the original data when bitmap indices with binning are used, is the main performance bottleneck of common query operations, such that the placement strategies of bin boundaries are of importance. The authors then propose a dynamic-programming based mechanism which, by taking *query distribution* and *data distribution* of the dataset into consideration, can suggest an optimal binning strategy. Note that the algorithm assumes that (1) the data set is read-only, (2) characteristics of the dataset and query requests are known ahead of time.

* **[LZG20, BinDex]** Linwei Li, Kai Zhang, Jiading Guo, Wen He, et al. "**BinDex: A Two-Layered Index for Fast and Robust Scans**". In SIGMOD'20.

---

#### Update

* **[AYI16, UpBit]** Manos Athanassoulis, Zheng Yan, and Stratos Idreos. "**UpBit: Scalable In-Memory Updatable Bitmap Indexing**". In SIGMOD'16.
    - This paper presents an updatable bitmap indexing. The core idea is to add an extra *update bit-vector* for each value bit-vector (in contrast, UCB uses a single *update bit-vector* for the whole bitmap index), such that (1) updates to different bit-vectors can execute in parallel, and (2) bit-vectors become sparser and their sizes after compression are smaller. Note that UpBit uses per-bit-vector mutex locks to synchronize concurrent lookup/scan, insert, delete, and merge operations.

* **[KSB21]** Steffen Kläbe, Kai-Uwe Sattler, and Stephan Baumann. "**Updatable Materialization of Approximate Constraints**". In ICDE'21 (short paper).

* **[GK09]** Sairam Gurajada and Sreenivasa Kumar P. "**On-line Index Maintenance Using Horizontal Partitioning**". In CIKM'09.
  - This paper focuses on Inverted List, an indexing data structure commonly used by IR systems and can be converted to bitmap index, and vice versa. This paper has a very comprehensive discussion on the background and related work of IL. In this paper, the authors proposed a new updatable IL algorithm that first partitions the terms based on their frequency numbers derived from query logs, and then uses a lazy-merge approach for maintaining the infrequent-term index, and an active merge approach for the frequent-term index.

* **[CGF07, UCB]** Guadalupe Canahuate, Michael Gibas, and Hakan Ferhatosmanoglu. "Update Conscious Bitmap Indices". In SSDBM'07.

---

#### Adaptive indexing (i.e., cracking, indexing merging, and their combination)

* **[IKM07]** Stratos Idreos, Martin L. Kersten, and Stefan Magegold. "**Database Cracking**". In CIDR'07.
  - This paper presents the first mature architecture for database cracking that can automatically organize indices according to the way users access data. The cracker index is (1) built dynamically (in contrast to upfront in traditional indices) while queries (in contrast to updates) are processed, and (2) adapts (in contrast to fixed) to changing hot query data. That is, indices built by cracking are discriminative (in contrast to non-discriminative ).

* **[GHI12]** Goetz Graefe, Felix Halim, Stratos Idreos, Harumi Kuno, and Stefan Manegold. "**Concurrency Control for Adaptive Indexing**". In VLDB'12.
  - This paper studies concurrency control in the context of adaptive indexing. This paper contains a comprehensive overview of the related works including *database cracking*, *adaptive merging*, and *hybrid indexing*.

---

#### Broader domain (i.e., data access)

* **[AI16]** Manos Athanassoulis, and Stratos Idreos. "**Design Tradeoffs of Data Access Methods**". In SIGMOD'16.
    - In this paper, the authors survey the state-of-the-art (as of 2016) developments in the data access methods of databases. They classify the methods in two dimensions. (1) The major design goals of a method -- read, update, or space. (2) The major techniques used -- *logarithmic design*, *continuous reorganization*, and *space efficient*. This is a very comprehensive survey on data access.

