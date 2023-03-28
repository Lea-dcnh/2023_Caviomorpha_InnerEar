# Allometry
## PC1 vs PC2

```r
# PCA with allometry

PCA<-gm.prcomp(gdf_mean$coords)
prems<-substr(rownames(PCA$x),1,1)
last<-substr(rownames(PCA$x),4,6)
rownames(PCA$x)<-paste(prems,last, sep="")

plot(
  PCA,
  frame.plot=F,
  axes=F,
   xlim=c(-0.125,0.1),
  ylim=c(-0.1,0.075),
  cex = gdf_mean$Csize*0.06,
  col = c("#cc242f", "#00AFBB", "#E7B800", "black", "#D07DFC")[superfamily_meanNA],
  pch = c(8,1,15,19,2,0,0,25,18,17,4,3,2)[family_meanNA],
  bg = c("#cc242f","#00AFBB", "#E7B800")[superfamily_meanNA],
  col.lab = "black")

text(
  PCA$x,
  font = 3,
  labels = rownames(PCA$x),
  cex = 0.7,
  pos=3,
  adj=2)
  
axis(1, pos=0,at = c(-0.125,-0.1,-0.075,-0.05,-0.025,0.025,0.05,0.075,0.1), tck=0.01, cex.axis=0.7, padj=-2)
axis(2, pos=0,at = c(-0.1,-0.075,-0.05,-0.025,0.025,0.05,0.075),tck=0.01, cex.axis=0.7, padj=2)


```
## PC1 vs PC3
```r

plot(
  PCA,
  axis1=1,
  axis2=3,
  frame.plot=F,
  lty=0,
  cex = gdf_mean$Csize*0.06,
  col = c("#cc242f", "#00AFBB", "#E7B800", "black", "#D07DFC")[superfamily_meanNA],
  pch = c(8,1,15,19,2,0,0,25,18,17,4,3,2)[family_meanNA],
  bg = c("#cc242f","#00AFBB", "#E7B800")[superfamily_meanNA],
  col.lab = "black")

text(
  PCA$x[,1], PCA$x[,3],
  font = 3,
  labels = rownames(PCA$x),
  cex = 0.7,
  pos=3,
  adj=2)

```
## PC2 vs PC3
```r

plot(
  PCA,
  axis1=2,
  axis2=3,
  frame.plot=F,
  lty=0,
  cex = gdf_mean$Csize*0.06,
  col = c("#cc242f", "#00AFBB", "#E7B800", "black", "#D07DFC")[superfamily_meanNA],
  pch = c(8,1,15,19,2,0,0,25,18,17,4,3,2)[family_meanNA],
  bg = c("#cc242f","#00AFBB", "#E7B800")[superfamily_meanNA],
  col.lab = "black")

text(
  PCA$x[,2], PCA$x[,3],
  font = 3,
  labels = rownames(PCA$x),
  cex = 0.7,
  pos=3,
  adj=2)

```

# Allometry-free

## PC1 vs PC2
```r
# PCA allometry-free
# The shape data of the mean of all specimen is corrected using the simple allometric regression of csize on shape (see Mfit).

shape.resid <- arrayspecs(Mfit$residuals,
                          p=dim(gdf_mean$coords)[1], k=dim(gdf_mean$coords)[2])
adj.shape <- shape.resid + array(mshape(gdf_mean$coords), dim(shape.resid))

PCAadj<-gm.prcomp(adj.shape) 
prems<-substr(rownames(PCAadj$x),1,1) 
last<-substr(rownames(PCAadj$x),4,6) 
rownames(PCAadj$x)<-paste(prems,last, sep="")

plot(
  PCAadj,
  frame.plot=F,
  col = c("#cc242f", "#00AFBB", "#E7B800", "black", "#D07DFC")[superfamily_meanNA],
  pch = c(8,1,15,19,2,0,0,25,18,17,4,3,2)[family_meanNA],
  bg = c("#cc242f","#00AFBB", "#E7B800")[superfamily_meanNA],
  col.lab = "black")
  
text(
  PCAadj$x,
  font = 3,
  labels = row.names(PCAadj$x),
  cex = 0.7,
  pos=3,
  adj=1,
  col = "black")
  
```

## PC1 vs PC3
```r

plot(
  PCAadj,
  axis1=1,
  axis2=3,
  frame.plot=F,
  lty=0,
   xlim=c(-0.1,0.05),
  ylim=c(-0.1,0.075),
  col = c("#cc242f", "#00AFBB", "#E7B800", "black", "#D07DFC")[superfamily_meanNA],
  pch = c(8,1,15,19,2,0,0,25,18,17,4,3,2)[family_meanNA],
  bg = c("#cc242f","#00AFBB", "#E7B800")[superfamily_meanNA],
  col.lab = "black")

text(
  PCAadj$x[,1], PCAadj$x[,3],
  font = 3,
  labels = rownames(PCAadj$x),
  cex = 0.7,
  pos=3,
  adj=2)

```
## PC2 vs PC3
```r

plot(
  PCAadj,
  axis1=2,
  axis2=3,
  frame.plot=F,
  lty=0,
  col = c("#cc242f", "#00AFBB", "#E7B800", "black", "#D07DFC")[superfamily_meanNA],
  pch = c(8,1,15,19,2,0,0,25,18,17,4,3,2)[family_meanNA],
  bg = c("#cc242f","#00AFBB", "#E7B800")[superfamily_meanNA],
  col.lab = "black")

text(
  PCAadj$x[,2], PCAadj$x[,3],
  font = 3,
  labels = rownames(PCA$x),
  cex = 0.7,
  pos=3,
  adj=2)

```
