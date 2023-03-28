

# HOS test

```r
#Allometric slopes per superfamilies (without Amblyrhiza, Clidomys and Elasmodontmys)

fit.unique <- procD.lm(coords ~ log(Csize) * superfamily, 
                       data = gdf_mean.ac, print.progress = FALSE, iter=9999, RRPP = T) 
fit.common <- procD.lm(coords~ log(Csize) + superfamily, 
                       data = gdf_mean.ac, print.progress = FALSE, iter=9999, RRPP = T)
                       
HOS_test<-anova(fit.common, fit.unique, print.progress = FALSE)
```

#### Graphical representation

```r
# Graphical representation of group-specific allometric trajectories 

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
