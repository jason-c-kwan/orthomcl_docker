Docker image for running [OrthoMCL](http://orthomcl.org/orthomcl/) for determining orthologs in a set of proteomes. Suggested config file:

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
