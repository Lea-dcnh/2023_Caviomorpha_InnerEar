#### All species

```r
# Allometry with all specimens of the sample
fitallo<-procD.lm(coords~log(Csize)*superfamily,
                  data = gdf,iter=9999, RRPP = T)
anova(fitallo)

# Allometry with the mean shape and Csize of all specimens
Mfitallo<-procD.lm(coords~log(Csize)*superfamily,
                  data = gdf_mean,iter=9999, RRPP = T)
anova(Mfitallo)

# log(csize)*superfamily barely significant, but is most probably due to the "NA" superfamily category of fossil taxa. See Mfitallo.ac instead.

#Simple allometry 
Mfit<-procD.lm(coords~log(Csize),
                  data = gdf_mean,iter=9999, RRPP = T)

# COMMENT Using the mean shape an csize of the species instead of all specimens lowers the significance of the csize*superfamily and increases the R-squared of superfamily.
```

#### Removal of uncertains fossils : Amblyrhiza, Clidomys and Elasmodontomys

```r
# Allometry of extant specimens only
fitallo.ac<-procD.lm(coords~log(Csize)*superfamily,
                     data = gdf.ac,iter=9999, RRPP = T)
anova(fitallo.ac)

# Allometry of the mean of certain specimens only
Mfitallo.ac<-procD.lm(coords~log(Csize)*superfamily,
                     data = gdf_mean.ac,iter=9999, RRPP = T)
anova(Mfitallo.ac)

#Simple allometry
Mfitac<-procD.lm(coords~log(Csize),
                     data = gdf_mean.ac,iter=9999, RRPP = T)

# COMMENT : The removal of fossil species of unknown affinities doesn't change the results too much compared to all specimens. 

```

## Mean shape & csize
```r

##### Allometry of the mean all species but AMB, CLI and ELA
Mfitallo.ac<-procD.lm(coords~log(Csize)*superfamily,
                     data = gdf_mean.ac,iter=9999, RRPP = T)
anova(Mfitallo.ac)

#### Allometry of extant species only

Mfitallo.extantonly<-procD.lm(coords~log(Csize)*superfamily,
                     data = gdf_mean.extantonly,iter=9999, RRPP = T)
anova(Mfitallo.extantonly)

#### Species in the phyloheny

Mfitallo.phy<- procD.lm(coords ~ log(Csize), 
                            phy = treCAVIO, 
                            data = gdf.phylo_mean, iter = 9999)

```

#### PGLS

```r

# Allometry with the mean shape and Csize of the specimens in the phylogeny
Mfitallo.pgls <- procD.pgls(coords ~ log(Csize), 
							phy = treCAVIO, 
                           data = gdf.phylo_mean, iter = 9999)
anova(Mfitallo.pgls)

# COMMENT : I can only do it with mean shape of species in order to correspond to the tips of the tree.
```
