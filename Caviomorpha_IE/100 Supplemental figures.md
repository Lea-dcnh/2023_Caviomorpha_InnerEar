## Compare the shape of the inner ear graphically from R

```r

setwd("/home/lea.dacunha/Work/M2/R/data/cavio/morphogeom")
dat<-read.csv("Model_Pred_Curves.txt",sep=',',header=T)
dat_curv_all<-as.matrix(dat)

#Find where are the specimens we want to compare

grep(T,dimnames(gdf_mean$coords)[[3]]=="CARsulc")
grep(T,dimnames(gdf_mean$coords)[[3]]=="CAPpilo")
grep(T,dimnames(gdf_mean$coords)[[3]]=="AMBinun")
grep(T,dimnames(gdf_mean$coords)[[3]]=="CLIspxx")
grep(T,dimnames(gdf_mean$coords)[[3]]=="ELAobli")
grep(T,dimnames(gdf_mean$coords)[[3]]=="DINbran")

plotRefToTarget(gdf_mean$coords[,,2],gdf_mean$coords[,,14],
                gridPars = gridPar(
                  pt.size = 1,
                  pt.bg = "#e0e0e0",
                  link.lwd = 3,
                  link.col="#958f8f",
                  tar.pt.bg = "#de4444",
                  tar.link.col = "#de4444",
                  tar.pt.size = 1,
                  tar.link.lwd = 3),
                method = "points",
                links = dat_curv_all)

# Amblyrhiza and Clidomys are almost identical! Amblyrhiza is extremely similar to Dinomys with slight diffecence : DIN has very slightly larger and more oval ASC and PSC but slighly shorter and rounder LSC. Cochlea is identical. 

library(pandoc)


```

## PCAs min and max
```r
setwd("C:/Users/lea_d/Work/R/graph/R_brut")
width<-1000
par3d(windowRect = 50 + c( 0, 0, width, width ))
rgl.viewpoint(theta = 110, phi = 90)

M<-mshape(gdf_mean$coords)

```

```r
# PCA Allometry min and max shapes

# PC1
plotRefToTarget(M, PCA$shapes[1]$shapes.comp1$min,
                gridPars = gridPar(
                  pt.size = 1,
                  pt.bg = "#e0e0e0",
                  link.lwd = 3,
                  link.col="#958f8f",
                  tar.pt.bg = "#4264fc",
                  tar.link.col = "#4264fc",
                  tar.pt.size = 1,
                  tar.link.lwd = 3),
                method = "points",
                links = dat_curv_all)
rgl.snapshot("PC1min_allo_l.png",fmt="png")
plotRefToTarget(M, PCA$shapes[1]$shapes.comp1$max,
                gridPars = gridPar(
                  pt.size = 1,
                  pt.bg = "#e0e0e0",
                  link.lwd = 3,
                  link.col="#958f8f",
                  tar.pt.bg = "#de4444",
                  tar.link.col = "#de4444",
                  tar.pt.size = 1,
                  tar.link.lwd = 3),
                method = "points",
                links = dat_curv_all)
rgl.snapshot("PC1max_allo_l.png",fmt="png")

# PC2
plotRefToTarget(M, PCA$shapes[2]$shapes.comp2$min,
                 gridPars = gridPar(
                  pt.size = 1,
                  pt.bg = "#e0e0e0",
                  link.lwd = 3,
                  link.col="#958f8f",
                  tar.pt.bg = "#4264fc",
                  tar.link.col = "#4264fc",
                  tar.pt.size = 1,
                  tar.link.lwd = 3),
                method = "points",
                links = dat_curv_all)
rgl.snapshot("PC2min_allo_l.png",fmt="png")
plotRefToTarget(M, PCA$shapes[2]$shapes.comp2$max,
                gridPars = gridPar(
                  pt.size = 1,
                  pt.bg = "#e0e0e0",
                  link.lwd = 3,
                  link.col="#958f8f",
                  tar.pt.bg = "#de4444",
                  tar.link.col = "#de4444",
                  tar.pt.size = 1,
                  tar.link.lwd = 3),
                method = "points",
                links = dat_curv_all)
rgl.snapshot("PC2max_allo_l.png",fmt="png")

# PC3
plotRefToTarget(M, PCA$shapes[3]$shapes.comp3$min,
                 gridPars = gridPar(
                  pt.size = 1,
                  pt.bg = "#e0e0e0",
                  link.lwd = 3,
                  link.col="#958f8f",
                  tar.pt.bg = "#4264fc",
                  tar.link.col = "#4264fc",
                  tar.pt.size = 1,
                  tar.link.lwd = 3),
                method = "points",
                links = dat_curv_all)
rgl.snapshot("PC3min_allo_l.png",fmt="png")

plotRefToTarget(M, PCA$shapes[3]$shapes.comp3$max,
                gridPars = gridPar(
                  pt.size = 1,
                  pt.bg = "#e0e0e0",
                  link.lwd = 3,
                  link.col="#958f8f",
                  tar.pt.bg = "#de4444",
                  tar.link.col = "#de4444",
                  tar.pt.size = 1,
                  tar.link.lwd = 3),
                method = "points",
                links = dat_curv_all)
rgl.snapshot("PC3max_allo_l.png",fmt="png")
```

```r
#PCA Allometry-free min and max shapes

#PC1
plotRefToTarget(M, PCAadj$shapes[1]$shapes.comp1$min,
                 gridPars = gridPar(
                  pt.size = 1,
                  pt.bg = "#e0e0e0",
                  link.lwd = 3,
                  link.col="#958f8f",
                  tar.pt.bg = "#4264fc",
                  tar.link.col = "#4264fc",
                  tar.pt.size = 1,
                  tar.link.lwd = 3),
                method = "points",
                links = dat_curv_all)

rgl.snapshot("PC1min_allofree_l.png",fmt="png")
plotRefToTarget(M, PCAadj$shapes[1]$shapes.comp1$max,
               gridPars = gridPar(
                  pt.size = 1,
                  pt.bg = "#e0e0e0",
                  link.lwd = 3,
                  link.col="#958f8f",
                  tar.pt.bg = "#de4444",
                  tar.link.col = "#de4444",
                  tar.pt.size = 1,
                  tar.link.lwd = 3),
                method = "points",
rgl.snapshot("PC1max_allofree_l.png",fmt="png")

# PC2
plotRefToTarget(M, PCAadj$shapes[2]$shapes.comp2$min,
                 gridPars = gridPar(
                  pt.size = 1,
                  pt.bg = "#e0e0e0",
                  link.lwd = 3,
                  link.col="#958f8f",
                  tar.pt.bg = "#4264fc",
                  tar.link.col = "#4264fc",
                  tar.pt.size = 1,
                  tar.link.lwd = 3),
                method = "points",
                links = dat_curv_all)
rgl.snapshot("PC2min_allofree_l.png",fmt="png")
plotRefToTarget(M, PCAadj$shapes[2]$shapes.comp2$max,
                gridPars = gridPar(
                  pt.size = 1,
                  pt.bg = "#e0e0e0",
                  link.lwd = 3,
                  link.col="#958f8f",
                  tar.pt.bg = "#de4444",
                  tar.link.col = "#de4444",
                  tar.pt.size = 1,
                  tar.link.lwd = 3),
                method = "points",
                links = dat_curv_all)
rgl.snapshot("PC2max_allofreel_d.png",fmt="png")

# PC3
plotRefToTarget(M, PCAadj$shapes[3]$shapes.comp3$min,
                 gridPars = gridPar(
                  pt.size = 1,
                  pt.bg = "#e0e0e0",
                  link.lwd = 3,
                  link.col="#958f8f",
                  tar.pt.bg = "#4264fc",
                  tar.link.col = "#4264fc",
                  tar.pt.size = 1,
                  tar.link.lwd = 3),
                method = "points",
                links = dat_curv_all)
rgl.snapshot("PC3min_allofree_l.png",fmt="png")
plotRefToTarget(M, PCAadj$shapes[3]$shapes.comp3$max,
                gridPars = gridPar(
                  pt.size = 1,
                  pt.bg = "#e0e0e0",
                  link.lwd = 3,
                  link.col="#958f8f",
                  tar.pt.bg = "#de4444",
                  tar.link.col = "#de4444",
                  tar.pt.size = 1,
                  tar.link.lwd = 3),
                method = "points",
                links = dat_curv_all)
rgl.snapshot("PC3max_allofree_l.png",fmt="png")
```