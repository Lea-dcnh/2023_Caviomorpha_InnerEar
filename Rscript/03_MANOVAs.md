# MANOVAs of mean shape and csize

```r
# All species

## Simple allometry 
Mfit<-procD.lm(coords~log(Csize),
                  data = gdf_mean,iter=9999, RRPP = T)
## Allometry with the mean shape and Csize of all specimens
Mfitallo<-procD.lm(coords~log(Csize)*superfamily,
                  data = gdf_mean,iter=9999, RRPP = T)
anova(Mfitallo)


# Removal of uncertains fossils : Amblyrhiza, Clidomys and Elasmodontomys
Mfitallo.ac<-procD.lm(coords~log(Csize)*superfamily,
                     data = gdf_mean.ac,iter=9999, RRPP = T)
anova(Mfitallo.ac)


# Allometry of extant species only
Mfitallo.extantonly<-procD.lm(coords~log(Csize)*superfamily,
                     data = gdf_mean.extantonly,iter=9999, RRPP = T)
anova(Mfitallo.extantonly)

			
```

# PGLS

```r
# Allometry with the mean shape and Csize of the specimens in the phylogeny
Mfitallo.pgls <- procD.pgls(coords ~ log(Csize), phy = treCAVIO, 
                           data = gdf.phylo_mean, iter = 9999)
anova(Mfitallo.pgls)

```
