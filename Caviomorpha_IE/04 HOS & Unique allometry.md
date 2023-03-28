
```r
# This one is not used because in the superfamily categories there is "NA" with all extinct taxa of uncertain affinities. This probably creates a supplemental category and causes errors in the analyse, especially when you compare it to the result by removing them (lowers the p-value by a lot in mean all species)

# All specimens (no averaging)
fit.unique <- procD.lm(coords ~ log(Csize) * superfamily, 
                       data = gdf, print.progress = FALSE, iter=9999, RRPP = T) 
fit.common <- procD.lm(coords~ log(Csize) + superfamily, 
                       data = gdf, print.progress = FALSE, iter=9999, RRPP = T) 
                       
anova(fit.common, fit.unique, print.progress = FALSE)

# Result : Significative difference between the allometric slope of superfamilies -> Non Homogeneity of Slopes so they aren't supposed to follow a common allometric pattern but a group-specific pattern instead. However the Rsq is very low (0.06) so it means that there probably isn't a lot of differences btw the alometric pattern of the superfamilies. 
```

```r
# Problem : Which one is better to use in the two following HOS tests ? The first one considers all extant specimens (n = 108) but there is a lot of redundant species. The second one (mean extant-only specimens) uses the mean size and shape of all extant species (n= 65), with only one species in the regression. What I used was the second one as I don't have the same number of specimens for each species otherwise, is it ok for the paper ?
```


#### The one used for PCA allo-free

```r
#Allometric slopes per superfamilies of all specimens (mean all specimens)
fit.unique <- procD.lm(coords ~ log(Csize) * superfamily, 
                       data = gdf_mean, print.progress = FALSE, iter=9999, RRPP = T) 
fit.common <- procD.lm(coords~ log(Csize) + superfamily, 
                       data = gdf_mean, print.progress = FALSE, iter=9999, RRPP = T) 

anova(fit.common, fit.unique, print.progress = FALSE)

# Result : Using the mean shape and size of species, there is no significative differences between the allometric slopes of the superfamilies (p-value=0.03) -> No Homogeneity of Slopes -> They aren't supposed to follow a common allometry pattern
```

#### Choose this one 

```r

# THIS IS THE ONE CHOSEN

#Allometric slopes per superfamilies (mean extant-only specimens)
fit.unique <- procD.lm(coords ~ log(Csize) * superfamily, 
                       data = gdf_mean.ac, print.progress = FALSE, iter=9999, RRPP = T) 
fit.common <- procD.lm(coords~ log(Csize) + superfamily, 
                       data = gdf_mean.ac, print.progress = FALSE, iter=9999, RRPP = T) # Common allometry or unique groupe allometry ?
HOS_test<-anova(fit.common, fit.unique, print.progress = FALSE)

# Removing the taxa of uncertain affinities (n=5), and the mean of all extant specimens, there is no significative difference between the allometric slopes of the superfamilies -> Homogeneity of slopes --> Common allometric pattern. 

#So I have to use the model that is proposed here : fit.common

```

#### Graphical representation

```r
# Graphical representation of group-specific allometric trajectories 

# COMMON (Chosen because of the results of the HOS test and PW comparisons)
common.allo<-plotAllometry(
  fit.common,
  frame=F,
  logsz = F,
  method = "PredLine",
  size = log(gdf_mean.ac$Csize),
  xlab = "log(cs)",
  cex = 0.07*(gdf_mean.ac$Csize),
  pch = 16,
  col = c("#cc242f", "#00AFBB", "#E7B800", "#D07DFC")[gdf_mean.ac$superfamily],
  lwd=NA
)
text(
  common.allo$plot.args$x,
  common.allo$plot.args$y,
  labels = row.names(Mfitallo.ac$Y),
  cex = 0.6,
  font = 3,
  adj = 1.5
)

# UNIQUE 
unique.allo<-plotAllometry(
  fit.unique,
  frame=F,
  logsz = F,
  method = "PredLine",
  size = log(gdf_mean.ac$Csize),
  xlab = "log(cs)",
  cex = 0.07*(gdf_mean.ac$Csize),
  pch = 16,
  col = c("#cc242f", "#00AFBB", "#E7B800", "#D07DFC")[gdf_mean.ac$superfamily],
  lwd=NA
)
text(
  unique.allo$plot.args$x,
  unique.allo$plot.args$y,
  labels = row.names(Mfitallo.ac$Y),
  cex = 0.6,
  font = 3,
  adj = 1.5
)
```