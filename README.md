# IR-code

### Halved Unicoil Collection
```
PATH = '/nvme/xiyue/JASS_unicoil/collection_JASS_half_Unicoil.trec'
```
The term frequency of each word has been halved. To test, replace the original collection file with the file above in step 1.

### 1. Build ATIRE Index
```
$ cd /home/xiyue/ATIRE
$ make
$ cd bin
$ ./index [-rtrec] -Q[impact, BM25, ...] [collection.trec]
```

To run Unicoil (whole):
```
$ ./index -rtrec -Qimpact /nvme/xiyue/JASS_unicoil/collection_JASS_Unicoil.trec
```

To run Unicoil (test with 1 doc):
```
$ ./index -rtrec -Qimpact /nvme/xiyue/JASS_unicoil/test_collection_unicoil.trec
```

### 2. Build JASS Index
```
$ cd /home/xiyue/JASS
$ make
$ ./atire_to_jass_index <index.aspt> [<queryfile>]
```

To run Unicoil (whole):
```
$ ./atire_to_jass_index ../ATIRE/bin/index.aspt /nvme/xiyue/JASS_unicoil/queries_JASS_Unicoil.tsv
```

To run Unicoil (test with 1 query):
```
$ ./atire_to_jass_index ../ATIRE/bin/index.aspt /nvme/xiyue/JASS_unicoil/test_query_unicoil.tsv
```

### 3. Run JASS
```
$ cd /home/xiyue/JASS
$ ./jass [<queryfile>] [topK] [postings] [> PATH_TO_RESULT_FILE]
```

To run Unicoil (whole):
```
$ ./jass /nvme/xiyue/JASS_unicoil/queries_JASS_Unicoil.tsv 10 9000000
```

To run Unicoil (test with 1 query):
```
$ ./jass /nvme/xiyue/JASS_unicoil/test_query_unicoil.tsv 10 9000000
```

### 4. Evaluate
Turn ranking result into trec format:
```
$ cd /home/xiyue/JASS
$ /usr/bin/python3 /home/xiyue/JASS/results/eval_format.py
```

Trec_eval:
```
$ /usr/bin/python3 /home/xiyue/JASS/results/trec_eval.py
```

Report Mean, Median, 95% time per query:
```
$ /usr/bin/python3 /home/xiyue/visualize/report.py [resultfile]
```
### 5.Some File you might like to look into
```
$ /home/xiyue/ATIRE/source/ranking_function_impact.c    \\ how term frequency is generated.
$ /home/xiyue/ATIRE/source/ranking_function.c           \\ where the author cap term frequency at 255.
$ /home/xiyue/JASS/jass.c                               \\ changing buffer size from 1024 would result in decrease of score.
```
