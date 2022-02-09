# QuantifyRC
Steps for quantifying read counts

1. Macs2 call peaks for each sample

2. Use R package ChIPpeakAnno findOverlapsofPeaks() function to find overlapped peaks across replicates within each condition, use reproduced peak as conditional peak, for example
    
Cond1.bed which have at least 3 columns Chr Start End

3.  Merge reproduced conditional peaks into a union peak as final list, you can simplify use

cat Cond1.bed Cond2.bed Cond3.bed | sort -k1,1  -k2,2n | bedtools merge -i - > Union_Peak.bed 

4. Use either featureCounts or bedtools coverage to get RC

5. Then do any downstream analysis based on this RC files. (edited) 
