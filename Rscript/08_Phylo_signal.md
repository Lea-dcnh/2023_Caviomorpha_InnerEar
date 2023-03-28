
# Phylogenetic Signal

```r
# Test of the phylogenetic signal of the residuals of the regression
Kres<-physignal(A=Mfitallo.phy$residuals,phy=treCAVIO,iter=9999)
plot(Kres)

```

```r

# Phylogenetic signal of csize 
PS.size <- physignal(A=log(gdf.phylo_mean$Csize),phy=treCAVIO,iter=9999)
summary(PS.size)
# Phylogenetic signal of the whole Inner Ear
PS.shape <- physignal(A=gdf.phylo_mean$coords,phy=treCAVIO,iter=9999)
summary(PS.shape)

# Phylogenetic signal of the SCCs only
PS.SCs <- physignal(A=gdf.phylo_meanSC$coords,phy=treCAVIO,iter=9999)
summary(PS.SCs)

# Phylogenetic signal of the Cochlea only
PS.cochlea <- physignal(A=gdf.phylo_meancoch$coords,phy=treCAVIO,iter=9999)
summary(PS.cochlea)


```
