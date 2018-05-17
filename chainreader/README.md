# analyze historical blocks
In this case, the "Tobalaba" chain of the energywebfoundation. More background information [here](https://gitlab.com/electronDLT/training-material/tree/master/EWF) (private).

## prep
All happens in virtualenv (see [../README.md](README.md)).

Start virtualenv, install dependencies:
```
source py3eth/bin/activate
pip3 install web3 pandas jupyter ipykernel matplotlib 
ipython kernel install --user --name="Python.3.py3eth"
```

## step 0: sync
Sync the chain. Possibly with `archive` flag on:

```
cd energywebfoundation_energyweb-client
./target/release/parity --pruning=archive --geth --chain tobalaba --rpcapi "web3,eth,personal" --db-compaction=ssd --cache-size=2048
```
It took about ~3 hours for >3 million transactions in the >4 million blocks of the "Tobalaba" chain.


## step 1: create DB
Parity is slow to query.

So we first extract all block information data, and dump it into an SQLite database.

```
python3 blocksDB_create.py
```

## step 2: analyze & visualize

Python Jupyter is a nice graphics enabled IDE to show tables, diagrams, etc inline that are created with Python pandas, numpy, matplotlib.
```
jupyter notebook
```

then execute all cells in [blocksDB_analyze.ipynb](blocksDB_analyze.ipynb)

Actually, the gitlab rendering of that ^ file is not bad,
so (even though it blows up the filesize),
I kept the results, incl all tables and diagrams, in that file. 

## results
```
blocknumber_max               4392279
blocksize_max                 118,576 bytes
txcount_max                   1,179 transactions in block 1,210,825
txcount_sum                   3,313,470 transactions in total
txcount_average               0.754 transactions per block
blocks_nonempty_count         1,951,767
average txcount per nonempty blocks =  1.698
```
That biggest block is block 1,210,825 - see [block explorer](https://tobalaba.etherscan.com/block/1210825).
### TPS = transactions per second:  

For many more such diagrams, see [blocksDB_analyze.ipynb](blocksDB_analyze.ipynb). 

![img/TPS_allBlocks.png](img/TPS_allBlocks.png)

some images are in [img/](img)