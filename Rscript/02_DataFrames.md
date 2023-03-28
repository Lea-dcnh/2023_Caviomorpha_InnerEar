# Mean Shape & csize

```r
# Resulting data frame with mean shape and csize of all specimens 
gdf_mean<-geomorph.data.frame(
  coords = meanDgeomR,
  Csize = meanDgeomcs,
  superfamily = superfamily_mean,
  family = family_mean)

# Dataframe of extant species of (certain) affinities
gdf_mean.ac <- geomorph.data.frame(
	coords = meanDgeomR.ac,
    Csize = meanDgeomcs.ac,
    superfamily =superfamily_mean.ac,
    family =family_mean.ac)

# Dataframe of all extant species only 
gdf_mean.extantonly <- geomorph.data.frame(
	coords = meanDgeomR.extant,
    Csize = meanDgeomcs.extant,
    superfamily =superfamily_mean.extant,
    family =family_mean.extant)

# Resulting data frame with mean shape and csize of specimens from the phylogeny
gdf.phylo_mean<-geomorph.data.frame(
  coords = meanDgeomphyR,
  Csize = meanDgeomphycs,
  superfamily = superfamily_phy_mean,
  family = family_phy_mean)
```

## For Cochlea and SCs only on the phylogeny

```r
# Cochlea
gdf.phylo_meancoch<-geomorph.data.frame(
  coords = meanDgeomphyR_coch,
  Csize = meanDgeomphycs_coch,
  superfamily = superfamily_phy_mean,
  family = family_phy_mean)

#SCs
gdf.phylo_meanSC<-geomorph.data.frame(
  coords = meanDgeomphyR_SC,
  Csize = meanDgeomphycs_SC,
  superfamily = superfamily_phy_mean,
  family = family_phy_mean)
```
