
#### Seed papers

* **[WLO85]** H. K. T. Wong, H.-F. Liu, F. Olken, D. Rotem, and L. Wong. "**Bit Transposed Files**". In VLDB'85.
  - The authors first examined the characteristics of Scientific/Statistical Databases (SSDB), and then proposed a new mechanism to store and query data in SSDB efficiently. AFAIK, this is the first research that (1) discusses in detail different encoding schemes and (2) proposes the bitvector organization that stores the Nth bits of all of the encoded values sequentially, in a single file, bringing several attractive features, including being amenable to compression. Furthermore, this paper presented a complete system design. Highly respect.

* **[OQ97]** Patrick O'Neil and Dallan Quass. "**Improved Query Performance with Variant Indexes**". In SIGMOD'97.
  - The authors performed a comprehensive comparison (in terms of expense) among different indexing schemes used in MODEL 240 (Value-List, Projection, and Bit-Sliced-based bitmap indexes). This paper introduced the concept of Segmentation and Bit-Sliced index.
 
* **[CI98]** Chee-Yong Chan, and Yannis E. Ioannidis. "**Bitmap Index Design and Evaluation**". In SIGMOD'98.
    - The authors examined the space-time tradeoff of various bitmap indices by using a proposed framework that evaluates indexes in two dimensions. Based on a proposed framework, they identify (1) the time-optimal bitmap indices, (2) the space-optimal bitmap indices, (3) the bitmap index with the optimal space-time tradeoff, and (4) the time-optimal bitmap indices under a given disk-space constraint. The authors presented a multi-component organization of bitmap indexes that can significantly reduce the number of bitvectors. However, it seems that the generated bitvectors are compression-unfriendly (at least by using RLE) and additional logical bitwise OPs are required to get the final results.
    Furthermore, this paper has a nice discussion on the underlying bit-matrix: "if one views this set as a two-dimensional bit matrix, the focus is on the columns. If the focus moves on the rows, however, then the index can be seen as the list of attribute values represented in some particular way. As there is a large number of possible representations for attribute values, this clearly opens up the way for defining a whole host of bitmap indexing schemes."

* **[CI99]** Chee-Yong Chan, and Yannis E. Ioannidis. "**An Efficient Bitmap Encoding Scheme for Selection Queries**". In SIGMOD'99.
    - This paper presents a detailed analysis of the performance of the existing two types of encoding schemes (equality- and range-encoded) with respect to various query operations (equality, (two-sided) range query, and membership query). The authors then propose a new encoding scheme (and four additional hybrid schemes), called *interval-encoding*, that takes about half the space of range encoding and at the same time, guarantees at most a two-scan evaluation for any query.

---

#### Cardinality

* **[WSS2008]** Kesheng Wu, Kurt Stockinger, and Arie Shoshani. "**Breaking the Curse of Cardinality on Bitmap Indexes**". in SSDBM'08.
  - The authors attempted to remove the limitation of building bitmap indexes on large-cardinality attributes by improving binning technique. The core is an interlayer data structure, named OrBiC (Order-preserving Bin-based Clustering), between a bitmap index and the underlying storage. OrBiC is fundamentally a projection of the whole attribute, but is reordered according to the bin numbers and preserving the relative order of values in each bin. By checking OrBiC, we can reduce the number of candidate checks, improving system performance. However, the memory footprint of OrBiC could be a limitation for large datasets. E.g., for a dataset with 10B entries, OrBiC consumes 8*10B = 80GB of storage space. There are some optimizations in the paper, but the memory consumption is still huge.

---

#### Compression

* **[WLPS17]** Jianguo Wang, Chunbin Lin, Yannis Papakonstantinou, and Steven Swanson. "**An Experimental Study of Bitmap Compression vs. Inverted List Compression**". In SIGMOD'17.
    - Both bitmap compression and inverted list compression solve the same problem: how to store a collection of sorted integers with as few as possible bits and support query processing as fast as possible. The authors performed comprehensive evaluations to compare 9 bitmap compression methods (including WAH, CONCISE, BBC, and Roaring) and 12 inverted list compression methods (including VB, PforDelta, and SIMDBP) by using both synthetic and real-life datasets. They attempt to pick out the best compression method in terms of space overhead, decompression time, intersection time, and union time.

* **[WOS02, WAH]** Kesheng Wu, Ekow J. Otoo, and Arie Shoshani. "Compressing Bitmap Indexes for Faster Search Operations". In SSDBM'02.

* **[WAB09, FastBit]** K Wu, S Ahern, E W Bethel, et al. "**FastBit: Interactively Searching Massive Data**". Lawrence Berkeley National Laboratory.

* **[WOS01]** Kesheng Wu, Ekow J. Otoo, and Arie Shoshani. "Compressed bitmap indices for efficient query processing". Lawrence Berkeley National Laboratory.
    - In this paper, the authors experimented with (WAH) compressed bitmap indexes with different encoding schemes. The conclusion is that the equality-encoded bitmap indexes are the most space-efficient, and the range-encoded versions are the most time-efficient. They also present two-level schemes (hierarchical design of different encoding schemes.)

* **[PLWAH10]** "**Position list word-aligned hybrid: optimizing space and performance for compressed bitmaps**". In EDBT'10.

---

#### Binning

* **[KOU]** Nick Koudas. "**Space Efficient Bitmap Indexing**. In CIKM'00.
  - This seed paper presents the binning technique and highlights the key performance factors: the frequency of access of the attribute and the frequency of occurrence of the attribute value. This paper gives a formal problem definition and complexity analysis. Respect!

* **[RSW05]** Doron Rotem, Kurt Stockinger, and Kesheng Wu. "**Optimizing Candidate Check Costs for Bitmap Indices**". In CIKM'05.
    - This paper underlines that *candidate check*, the unavoidable access to the original data when bitmap indices with binning are used, is the main performance bottleneck of common query operations, such that the placement strategies of bin boundaries are of importance. The authors then propose a dynamic-programming based mechanism which, by taking *query distribution* and *data distribution* of the dataset into consideration, can suggest an optimal binning strategy. Note that the algorithm assumes that (1) the data set is read-only, (2) characteristics of the dataset and query requests are known ahead of time.


---

#### Faster queries

* **[LZG20, BinDex]** Linwei Li, Kai Zhang, Jiading Guo, Wen He, et al. "**BinDex: A Two-Layered Index for Fast and Robust Scans**". In SIGMOD'20.

* **[LBL20, TEB]** Harald Lang, Alexander Berschl, Viktor Leis, et al. "**Tree-Encoded Bitmaps**". In SIGMOD'20.


---

#### Update-friendly

* **[AYI16, UpBit]** Manos Athanassoulis, Zheng Yan, and Stratos Idreos. "**UpBit: Scalable In-Memory Updatable Bitmap Indexing**". In SIGMOD'16.
    - This paper presents an updatable bitmap indexing. The core idea is to add an extra *update bit-vector* for each value bit-vector (in contrast, UCB uses a single *update bit-vector* for the whole bitmap index), such that (1) updates to different bit-vectors can execute in parallel, and (2) bit-vectors become sparser and their sizes after compression are smaller. Note that UpBit uses per-bit-vector mutex locks to synchronize concurrent lookup/scan, insert, delete, and merge operations.

* **[KSB21]** Steffen Kläbe, Kai-Uwe Sattler, and Stephan Baumann. "**Updatable Materialization of Approximate Constraints**". In ICDE'21 (short paper).

* **[CGF07, UCB]** Guadalupe Canahuate, Michael Gibas, and Hakan Ferhatosmanoglu. "Update Conscious Bitmap Indices". In SSDBM'07.


---

#### Other related work (optional)

* **[AI16]** Manos Athanassoulis, and Stratos Idreos. "**Design Tradeoffs of Data Access Methods**". In SIGMOD'16.
    - In this paper, the authors survey the state-of-the-art (as of 2016) developments in the data access methods of databases. They classify the methods in two dimensions. (1) The major design goals of a method -- read, update, or space. (2) The major techniques used -- *logarithmic design*, *continuous reorganization*, and *space efficient*. This is a very comprehensive survey on data access.

* **[IKM07]** Stratos Idreos, Martin L. Kersten, and Stefan Magegold. "**Database Cracking**". In CIDR'07.
  - This paper presents the first mature architecture for database cracking that can automatically organize indices according to the way users access data. The cracker index is (1) built dynamically (in contrast to upfront in traditional indices) while queries (in contrast to updates) are processed, and (2) adapts (in contrast to fixed) to changing hot query data. That is, indices built by cracking are discriminative (in contrast to non-discriminative ).

* **[GHI12]** Goetz Graefe, Felix Halim, Stratos Idreos, Harumi Kuno, and Stefan Manegold. "**Concurrency Control for Adaptive Indexing**". In VLDB'12.
  - This paper studies concurrency control in the context of adaptive indexing. This paper contains a comprehensive overview of the related works including *database cracking*, *adaptive merging*, and *hybrid indexing*.
 
* **[Roaring]** http://roaringbitmap.org/about/
