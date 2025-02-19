# Anserini: Experiments on [MS MARCO (Passage)](https://github.com/microsoft/MSMARCO-Passage-Ranking)

This page contains basic instructions for getting started on the MS MARCO *passage* ranking task.
Note that there is a separate [MS MARCO *document* ranking task](experiments-msmarco-doc.md).

## Data Prep

We're going to use `msmarco-passage/` as the working directory.
First, we need to download and extract the MS MARCO passage dataset:

```
mkdir msmarco-passage

wget https://msmarco.blob.core.windows.net/msmarcoranking/collectionandqueries.tar.gz -P msmarco-passage
tar -xzvf msmarco-passage/collectionandqueries.tar.gz -C msmarco-passage
```

To confirm, `collectionandqueries.tar.gz` should have MD5 checksum of `31644046b18952c1386cd4564ba2ae69`.

Next, we need to convert the MS MARCO tsv collection into Anserini's jsonl files (which have one json object per line):

```
python ./src/main/python/msmarco/convert_collection_to_jsonl.py \
 --collection_path msmarco-passage/collection.tsv --output_folder msmarco-passage/collection_jsonl
```

The above script should generate 9 jsonl files in `msmarco-passage/collection_jsonl`, each with 1M lines (except for the last one, which should have 841,823 lines).

We can now index these docs as a `JsonCollection` using Anserini:

```
sh ./target/appassembler/bin/IndexCollection -collection JsonCollection \
 -generator LuceneDocumentGenerator -threads 9 -input msmarco-passage/collection_jsonl \
 -index msmarco-passage/lucene-index-msmarco -storePositions -storeDocvectors -storeRawDocs 
```

The output message should be something like this:

```
2019-06-08 08:53:47,351 INFO  [main] index.IndexCollection (IndexCollection.java:632) - Total 8,841,823 documents indexed in 00:01:31
```

Your speed may vary... with a modern desktop with an SSD, indexing takes less than two minutes.

## Retrieving and Evaluating the Dev set

Since queries of the set are too many (+100k), it would take a long time to retrieve all of them. To speed this up, we use only the queries that are in the qrels file: 

```
python ./src/main/python/msmarco/filter_queries.py --qrels msmarco-passage/qrels.dev.small.tsv \
 --queries msmarco-passage/queries.dev.tsv --output_queries msmarco-passage/queries.dev.small.tsv
```

The output queries file should contain 6980 lines.

We can now retrieve this smaller set of queries:

```
python ./src/main/python/msmarco/retrieve.py --hits 1000 --index msmarco-passage/lucene-index-msmarco \
 --qid_queries msmarco-passage/queries.dev.small.tsv --output msmarco-passage/run.dev.small.tsv
```

Note that by default, the above script uses BM25 with tuned parameters `k1=0.82`, `b=0.72` (more details below).
The option `-hits` specifies the of documents per query to be retrieved.
Thus, the output file should have approximately 6980 * 1000 = 6.9M lines. 

Retrieval speed will vary by machine:
On a modern desktop with an SSD, we can get ~0.06 s/query (taking about seven minutes).
Alternatively, we can run the same script implemented in Java (which for some reason seems to be slower, see [issue 604](https://github.com/castorini/anserini/issues/604)):

```
./target/appassembler/bin/SearchMsmarco  -hits 1000 -index msmarco-passage/lucene-index-msmarco \
 -qid_queries msmarco-passage/queries.dev.small.tsv -output msmarco-passage/run.dev.small.tsv
```

Finally, we can evaluate the retrieved documents using this the official MS MARCO evaluation script: 

```
python ./src/main/python/msmarco/msmarco_eval.py \
 msmarco-passage/qrels.dev.small.tsv msmarco-passage/run.dev.small.tsv
```

And the output should be like this:

```
#####################
MRR @10: 0.18751751034702308
QueriesRanked: 6980
#####################
```

We can also use the official TREC evaluation tool, `trec_eval`, to compute other metrics than MRR@10. 
For that we first need to convert runs and qrels files to the TREC format:

```
python ./src/main/python/msmarco/convert_msmarco_to_trec_run.py \
 --input_run msmarco-passage/run.dev.small.tsv --output_run msmarco-passage/run.dev.small.trec

python ./src/main/python/msmarco/convert_msmarco_to_trec_qrels.py \
 --input_qrels msmarco-passage/qrels.dev.small.tsv --output_qrels msmarco-passage/qrels.dev.small.trec
```

And run the `trec_eval` tool:

```
./eval/trec_eval.9.0.4/trec_eval -c -mrecall.1000 -mmap \
 msmarco-passage/qrels.dev.small.trec msmarco-passage/run.dev.small.trec
```

The output should be:

```
map                   	all	0.1956
recall_1000           	all	0.8578
```

Average precision and recall@1000 are the two metrics we care about the most.

## BM25 Tuning

Note that this figure differs slightly from the value reported in [Document Expansion by Query Prediction](https://arxiv.org/abs/1904.08375), which uses the Anserini (system-wide) default of `k1=0.9`, `b=0.4`.

Tuning was accomplished with the `tune_bm25.py` script, using the queries found [here](https://github.com/castorini/Anserini-data/tree/master/MSMARCO).
There are five different sets of 10k samples (from the `shuf` command).
We tune on each individual set and then average parameter values across all five sets (this has the effect of regularization).
Note that we are currently optimizing recall@1000 since Anserini output will serve as input to later stage rerankers (e.g., based on BERT), and we want to maximize the number of relevant documents the rerankers have to work with.
The tuned parameters using this method are `k1=0.82`, `b=0.72`.

Here's the comparison between the Anserini default and tuned parameters:

Setting                     | MRR@10 | MAP    | Recall@1000 |
:---------------------------|-------:|-------:|------------:|
Default (`k1=0.9`, `b=0.4`) | 0.1839 | 0.1925 | 0.8526
Tuned (`k1=0.82`, `b=0.72`) | 0.1875 | 0.1956 | 0.8578
