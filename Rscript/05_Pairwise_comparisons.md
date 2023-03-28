# Pairwise comparisons

```r
PW<-pairwise(Mfitallo.ac,groups=gdf_mean.ac$superfamily, covariate = log(gdf_mean.ac$Csize))

slope_angle<-summary(PW,test.type="VC",angle.type="deg",show.vectors=F,confidence=0.95) 
slope_length<-summary(PW,test.type="dist",show.vectors=F,confidence=0.95) 
```
# Multiple comparisons corrections

```r
#p-values adjusted for multiple comparisons. It gives a strong control of the family-wise error rate : the probability of making a Type I error among a specified group, or "family," of tests.

# Bonferroni correction (p-values multiplied by the number of comparisons)
slopea_corB<-p.adjust(slope_angle$summary.table$`Pr > angle`,method="bonferroni")
sloped_corB<-p.adjust(slope_length$summary.table$`Pr > d`,method="bonferroni")

# Holm correction
slopea_corHol<-p.adjust(slope_angle$summary.table$`Pr > angle`,method="holm")
sloped_corHol<-p.adjust(slope_length$summary.table$`Pr > d`,method="holm")
