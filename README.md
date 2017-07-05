# WholeGenomeAnalysis
##Whole Genome Analysis by Plink ang GCTA
##Used for Basic association testing

Input file: IMMUNO chip from Alzheimer's Patients

#PLINK

./plink --noweb --bfile IMMUNO-PD --mind 0.05 --geno 0.05 --maf 0.05 --hwe 1e-3 --me 0.05 0.1 --make-bed --out IMMUNO-PD1

./plink --noweb --bfile IMMUNO-PD1 --filter-founders --make-bed --out IMMUNO-PD2

./plink --noweb --bfile IMMUNO-PD2 --indep 50 5 2 --out prunedsnps 

./plink --noweb --bfile IMMUNO-PD2 --extract prunedsnps.prune.in --make-bed --out IMMUNO-PD3

./plink --noweb --bfile IMMUNO-PD3 --cluster --neighbour 1 5

./plink --noweb --bfile IMMUNO-PD3 --remove outlier.txt --make-bed --out IMMUNO-PD4

./plink --noweb --bfile IMMUNO-PD4 --genome --out IMMUNO-PD4-IBD

./plink --noweb --bfile IMMUNO-PD4 --read-genome IMMUNO-PD4-IBD.genome --cluster --ppc 1e-3 --cc --mds-plot 2 --out IMMUNO-PD4-MDS

#GCTA
gcta64 --bfile test --autosome --maf 0.01 --make-grm --out test --thread-num 10

gcta64 --bfile test --chr 1 --maf 0.01 --make-grm --out test_chr1 --thread-num 10
gcta64 --bfile test --chr 2 --maf 0.01 --make-grm --out test_chr2 --thread-num 10
…
gcta64 --bfile test --chr 22 --maf 0.01 --make-grm --out test_chr22 --thread-num 10
gcta64 --mgrm grm_chrs.txt --make-grm --out test

gcta64 --grm test --grm-cutoff 0.025 --make-grm --out test_rm025

gcta64 --grm test --pca 20 --out test_pca
