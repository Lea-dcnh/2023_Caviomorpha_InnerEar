#### Mean all specimens
```r
PW<-pairwise(Mfitallo,groups=gdf_mean$superfamily, covariate = log(gdf_mean$Csize))

# correlation between mean vectors
slope_angle<-summary(PW,test.type="VC",angle.type="deg",show.vectors=F,confidence=0.95) 

# Result : Null hypothesis confirmed = Parallel vectors --> Common allometric pattern?

# distances between means (length of vectors?)
slope_length<-summary(PW,test.type="dist",show.vectors=F,confidence=0.95) 
```


#### Mean extant species (the one chosen)
```r
PW<-pairwise(Mfitallo.ac,groups=gdf_mean.ac$superfamily, covariate = log(gdf_mean.ac$Csize))

# correlation between mean vectors
slope_angle<-summary(PW,test.type="VC",angle.type="deg",show.vectors=F,confidence=0.95) 

# Result : Null hypothesis confirmed = Parallel vectors

# distances between means (length of vectors?)
slope_length<-summary(PW,test.type="dist",show.vectors=F,confidence=0.95) 
```

```r
#p-values adjusted for multiple comparisons. It gives a strong control of the family-wise error rate : the probability of making a Type I error among a specified group, or "family," of tests.

# Bonferroni correction (p-values multiplied by the number of comparisons)
slopea_corB<-p.adjust(slope_angle$summary.table$`Pr > angle`,method="bonferroni")
sloped_corB<-p.adjust(slope_length$summary.table$`Pr > d`,method="bonferroni")

#Holm correction
slopea_corHol<-p.adjust(slope_angle$summary.table$`Pr > angle`,method="holm")
sloped_corHol<-p.adjust(slope_length$summary.table$`Pr > d`,method="holm")

# Only to use if variables or independant

# Hochberg correction
slopea_corHoc<-p.adjust(slope_angle$summary.table$`Pr > angle`,method="hochberg")
sloped_corHoc<-p.adjust(slope_length$summary.table$`Pr > d`,method="hochberg")

# Hommel correction
slopea_corHom<-p.adjust(slope_angle$summary.table$`Pr > angle`,method="hommel")
sloped_corHom<-p.adjust(slope_length$summary.table$`Pr > d`,method="hommel")

# Tukey test to compare the intercepts of slopes?
TukeyHSD()

