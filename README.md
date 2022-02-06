# tb-lineage-mutations
Reference list of lineage-defining mutations as derived from SNP-IT.

## Background

[SNP-IT](https://github.com/samlipworth/snpit) is a tool written by Sam Lipworth that infers the species, lineage and (if possible) sub-lineage of a Mycobacterial samples and is published as below:

> Lipworth S, Jajou R, De Neeling A, Bradley P, Van Der Hoek W, Maphalala G, Bonnet M, Sanchez-Padilla E, Diel R, Niemann S, Iqbal Z, Smith G, Peto T, Crook D, Walker T, Van Soolingen D. 2019. SNP-IT tool for identifying subspecies and associated lineages of Mycobacterium tuberculosis complex. Emerg Infect Dis 25:482–488. [doi:10.3201/eid2503.180894](https://doi.org/10.3201/eid2503.180894)

In the `lib/` folder it contains a file `library.csv` that lists the lineages covered. The `id` is the internal identifier given to that lineage by SNP-IT.

```
id,species,lineage,sublineage
Indo_Oceanic,M. tuberculosis,Lineage 1,
beijing,M. tuberculosis,Lineage 2,
East_African_Indian,M. tuberculosis,Lineage 3,
```

within the same `lib/` folder, each lineage has a single file named with its `id` e.g. `beijing` contains

```
1011511 C
1022003 C
1028217 A
1034758 T
1071966 G
1076689 C
1076880 T
1097442 T
1102468 A
```

This is a tab-limited file containing the genome-indices (1-based) of positions in the H37Rv version 3 genome which can be used to identify this lineage. If e.g. at a genome index of 1011511 there is a `C` then that is consistent with this sample belonging to Lineage 2.

## Approach

We simply read these files in, then using `gumpy`, apply all the single nucleotide changes identified for that lineage. Then, using `gumpy`, we create a list of the `VARIANTS` and, more usefully, by only considering those SNVs in genes and translating genes into amino acids, we create a list of `MUTATIONS`. E.g. for `VARIANTS`

```
SNPIT_ID	SPECIES	LINEAGE	SUBLINEAGE	VARIANT
Dassie	Dassie bacillus (ex Procavia capensis)	NaN	NaN	4087g>t
Dassie	Dassie bacillus (ex Procavia capensis)	NaN	NaN	5073g>a
Dassie	Dassie bacillus (ex Procavia capensis)	NaN	NaN	19052c>g
```
and for `MUTATIONS`

```
SNPIT_ID	SPECIES	LINEAGE	SUBLINEAGE	GENE	MUTATION
Dassie	Dassie bacillus (ex Procavia capensis)	NaN	NaN	PE_PGRS11	T469N
Dassie	Dassie bacillus (ex Procavia capensis)	NaN	NaN	PE_PGRS11	R512L
Dassie	Dassie bacillus (ex Procavia capensis)	NaN	NaN	PE_PGRS11	P518L
```

What is not yet clear is if SNPIT contains the complete set of lineage-defining mutations, or just sufficient to allow a sample to be identified. It likely does not include mutations deep enough in the phylogenetic tree that they cannot unambigiously define which lineage a sample belongs to, but merely narrow it down.

Philip W Fowler
6 Feb 2022
