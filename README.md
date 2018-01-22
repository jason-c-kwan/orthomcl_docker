Docker image for running [OrthoMCL](http://orthomcl.org/orthomcl/) for determining orthologs in a set of proteomes. 

The image contains a script, run_orthomcl.py, which automates much of the process. Before you start, assemble all
the proteomes you want to compare in one directory (one fasta file for each species). Then make a table containing
all the fasta filenames in the first column. In the second column write a unique species abbreviation up to four
characters long. You can get the table started like this:

```
ls -1 *.fasta > fasta_table
``` 

Next make an OrthoMCL config file. The suggested contents are shown below.

```
dbVendor=mysql
dbConnectString=dbi:mysql:orthomcl:mysql_local_infile=1
dbLogin=root
dbPassword=PAssw0rd
similarSequencesTable=SimilarSequences
orthologTable=Ortholog
inParalogTable=InParalog
coOrthologTable=CoOrtholog
interTaxonMatchView=InterTaxonMatch
percentMatchCutoff=50
evalueExponentCutoff=-5
oracleIndexTblSpc=NONE
```

Now you can run the pipeline as follows:

```
docker run --rm --volume current_dir:/output_dir:rw jasonkwan/orthomcl_docker:latest run_orthomcl.py \
	--table_path fasta_table --config_file orthomcl.config --processors 16
```

The script run_orthomcl.py also allows you to specify the start and end point of the pipeline with the --start_stage 
and --end_stage options. These take integers from 1 to 4, according to the following scheme:

1. Processing the fasta files with orthomclAdjustFasta, and orthomclFilterFasta
2. Carrying out the all-vs-all blast (in this case with [DIAMOND](https://github.com/bbuchfink/diamond) rather than NCBI blast
3. Identifying pairs using a MySQL database
4. Processing the pairs with MCL

At the end you should have a groups.txt file that contains the identified groups.
