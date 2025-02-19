# Anserini Regressions Log

The following change log details commits to regression tests that alter effectiveness and the addition of new regression tests.
This documentation is useful for figuring why results may have changed over time.

### June 9. 2019

+ commit [93f8f3c](https://github.com/castorini/anserini/commit/93f8f3c769bc9f95b2c351dc8347f0bbe940d1b2) (6/9/2019)
+ commit [781d9ed](https://github.com/castorini/anserini/commit/781d9ed111b41883c2342ff4d367ca77cfd9ebb2) (6/8/2019)

Added regressions for MS MARCO passage and document ranking tasks.

### June 3, 2019

+ commit [3545350](https://github.com/castorini/anserini/commit/3545350f3a12bd55a616e41057aa51d3b5cfaed2) (6/3/2019)
+ commit [a3ccdef](https://github.com/castorini/anserini/commit/a3ccdef9486816b756c6e34052f43e6db20b6afb) (6/3/2019)

Fixed bug in topic reader for CAR. Better parsing of New York Times documents. Regression numbers in both cases improved slightly.

### May 31, 2019

+ commit [27493ed](https://github.com/castorini/anserini/commit/27493ed999b230db6153e98a4113b5a44ff362d9) (5/31/2019)

Per [Issue 658](https://github.com/castorini/anserini/issues/658): fixed broken regression in Core18 introduced by commit [c4ab6b](https://github.com/castorini/anserini/commit/c4ab6bfbe38648337ad824503e8945f5111ac673) (4/18/2019).

### May 11, 2019

+ commit [3eef2fb](https://github.com/castorini/anserini/commit/3eef2fb4808d88608d2c467418b77516eba3538a) (5/11/2019)
+ commit [2ba2b95](https://github.com/castorini/anserini/commit/2ba2b9582ee942aee714301b78015a2ded16da8c) (5/11/2019)
+ commit [d911bba](https://github.com/castorini/anserini/commit/d911bba180aceabbe052624b7f715a2189e25dbb) (5/10/2019)

CAR regression refactoring: added v2.0 regression and renamed existing regression to v1.5. Both use `benchmarkY1-test` to support consistent comparisons.

### January 2, 2019

+ commit [407f308](https://github.com/castorini/Anserini/commit/407f308cc543286e39701caf0acd1afab39dde2c) (1/2/2019)

Added fine tuning results (i.e., SIGIR Forum article experiments) for axiomatic semantic term matching.

### December 24, 2018

+ commit [1aa3970](https://github.com/castorini/Anserini/commit/1aa3970bd32b456025ada608389f7e4896eff19e) (12/24/2018)

Changed RM3 defaults to match settings in Indri.

### December 20, 2018

+ commit [e71df7a](https://github.com/castorini/Anserini/commit/26fbb3936cb2db1d69f02ad990d83e773e7d87c2) (12/20/2018)

Added Axiomatic F2Exp and F2Log ranking models back into Anserini (previously, we were using the default Lucene implementation as part of version 7.6 upgrade).

### December 18, 2018

+ commit [e71df7a](https://github.com/castorini/Anserini/commit/e71df7aee42c7776a63b9845600a4075632fa11c) (12/18/2018)

Upgrade to Lucene 7.6.

### November 30, 2018

+ commit [e5b87f0](https://github.com/castorini/Anserini/commit/e5b87f0d6c16b47d0be6cc8fd587acd20e3fbb0d) (11/30/2018)

Added default regressions for TREC 2018 Common Core Track.

### November 16, 2018

+ commit [2c8cd7a](https://github.com/castorini/Anserini/commit/2c8cd7a550faca0fc450e4159a4a874d4795ac25) (11/16/2018)

This is the commit id references in the [SIGIR Forum 2018 article](http://sigir.org/wp-content/uploads/2019/01/p040.pdf).
Note that commit [18c3211](https://github.com/castorini/Anserini/commit/18c3211117f35f72cbc1019c125ff885f51056ea) (12/9/2018) contains minor fixes to the code.

### October 22, 2018

+ commit [10255e0](https://github.com/castorini/Anserini/commit/10255e0f15c8caca94f8d5376a2c7c9ad1f5b5fd) (10/22/2018)

Fixed incorrect implementation of `-rm3.fbTerms`.

### September 26, 2018

+ commit [7c882d3](https://github.com/castorini/Anserini/commit/7c882d310564e27351ed51e0c8a669a13f33b48a) (9/26/2018)

Fixed bug as part of [Issue 429](https://github.com/castorini/Anserini/issues/429): `cw12` and `mb13` regression tests changed slightly in effectiveness.

### August 8, 2018

+ commit [d4b3272](https://github.com/castorini/Anserini/commit/d4b3272e7f07fa274e5d8ffd50976beb0c08de52) (8/8/2018)

Added regressions tests for CAR17.

### August 5, 2018

+ commit [c0da510](https://github.com/castorini/Anserini/commit/c0da5105429a15fb85158d1740e0516305cd9de6) (8/5/2018)

This commit adds the effectiveness verification testing for the [JDIQ2018 Paper](experiments-jdiq2018.md).

### July 22, 2018

+ commit [3a7beee](https://github.com/castorini/Anserini/commit/3a7beee3485526f3146e69f57899a3033e20f504) (7/22/2018)
+ commit [ec5fd3d](https://github.com/castorini/Anserini/commit/ec5fd3d7fbee3308cd63321b77231d8b10e495a8) (7/22/2018)
+ commit [5f8c26d3](https://github.com/castorini/Anserini/commit/5f8c26d328dd67e6cc538d5f9b4af44acdbc74e5) (7/22/2018)

These three commits establish the new regression testing infrastructure with the following tests:

+ [Experiments on Disks 1 &amp; 2](experiments-disk12.md): {BM25, QL} &#10799; {RM3, Ax} &#10799; {AP, P30}
+ [Experiments on Disks 4 &amp; 5 (Robust04)](experiments-robust04.md): {BM25, QL} &#10799; {RM3, Ax} &#10799; {AP, P30}
+ [Experiments on AQUAINT (Robust05)](experiments-robust05.md): {BM25, QL} &#10799; {RM3, Ax} &#10799; {AP, P30}
+ [Experiments on New York Times (Core17)](experiments-core17.md): {BM25, QL} &#10799; {RM3, Ax} &#10799; {AP, P30}
+ [Experiments on Wt10g](experiments-wt10g.md): {BM25, QL} &#10799; {RM3, Ax} &#10799; {AP, P30}
+ [Experiments on Gov2](experiments-gov2.md): {BM25, QL} &#10799; {RM3, Ax} &#10799; {AP, P30}
+ [Experiments on ClueWeb09 (Category B)](experiments-cw09b.md): {BM25, QL} &#10799; {RM3, Ax} &#10799; {AP, P30, NDCG@20, ERR@20}
+ [Experiments on ClueWeb12-B13](experiments-cw12b13.md): {BM25, QL} &#10799; {RM3, Ax} &#10799; {AP, P30, NDCG@20, ERR@20}
+ [Experiments on ClueWeb12](experiments-cw12.md): {BM25, QL} &#10799; {RM3} &#10799; {AP, P30, NDCG@20, ERR@20}
+ [Experiments on Tweets2011 (MB11 &amp; MB12)](experiments-mb11.md): {BM25, QL} &#10799; {RM3, Ax} &#10799; {AP, P30}
+ [Experiments on Tweets2013 (MB13 &amp; MB14)](experiments-mb13.md): {BM25, QL} &#10799; {RM3, Ax} &#10799; {AP, P30}
