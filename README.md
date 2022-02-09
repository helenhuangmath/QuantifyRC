# Quantify Read Counts
Steps for quantifying read counts

1. Macs2 call peaks for each sample

2. Use R package ChIPpeakAnno findOverlapsofPeaks() function to find overlapped peaks across replicates within each condition, use reproduced peak as conditional peak, for example
    
Cond1.bed which have at least 3 columns Chr Start End

3.  Merge reproduced conditional peaks into a union peak as final list, you can simplify use

cat Cond1.bed Cond2.bed Cond3.bed | sort -k1,1  -k2,2n | bedtools merge -i - > Union_Peak.bed 

4. Use either featureCounts or bedtools coverage to get RC

Manual for bedtools coverage https://bedtools.readthedocs.io/en/latest/content/tools/coverage.html 

Manual for featureCounts http://subread.sourceforge.net/featureCounts.html

If using bedtools coverage, you need to run sample by sample and then merge them based on peakID

bedtools coverage -counts -a Union_Peak.bed -b Sample1.clean.bam > RawRC_Sample1.txt

If using featureCounts, you can run all samples together, but need to reformat *bed to a *saf format

awk 'OFS="\t" {print $5,$1,$2,$3,"."}' Union_peak.bed | sed '1 i GeneID\tChr\tStart\tEnd\tStrand' > Union_peak.saf

featureCounts -p -a Union_peak.saf -F SAF -o RawRC_eatureCounts.txt BAM/*clean.bam


5. Then do any downstream analysis based on this RC files. (edited) 
