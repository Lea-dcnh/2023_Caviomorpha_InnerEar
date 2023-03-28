#### Allometry

```r
# Create dataframe
DF_meanac<- clean_names( data.frame(two.d.array(gdf_mean.ac$coords),superfamily=gdf_mean.ac$superfamily,check.names=FALSE))

# LOOCV 
v <-c()

for (i in 1:nrow(DF_meanac)){
  test<- DF_meanac[i,]
  train<- DF_meanac[-i,]
  modele <-lda(formula=superfamily ~ ., data=train)
  pred <-predict(modele, newdata=test)
  #takes back prediction
  v <-c(v,pred$class[1])
}

v<-as.factor(v)
levels(v)<-c("CA","CH","ER","OC")

# Confusion matrix
coord.mod<-confusionMatrix(reference=as.factor(DF_meanac$superfamily),data=v)
coord.mod


# Attribution of fossil taxa
lda.coord<-lda(superfamily~., data=DF_meanac)

cavio2D<-two.d.array(gdf_mean$coords)
colnames(cavio2D)<-colnames(DF_meanac)[1:219]

foss<-cavio2D[grep(TRUE,is.na(superfamily_mean)),]
foss<-foss[-4,]
pfoss<-predict(lda.coord,as.data.frame(foss))
pfoss
```

#### Figure LDA()

```r

plot(lda.coord, 
     panel = function(x, y, ...) {
       points(x, y, ...)
     },
     col=c("#cc242f","#00AFBB", "#E7B800","#D07DFC")[gdf_mean.ac$superfamily], 
     pch = 20)


center_scale <- function(x) {scale(x, scale = FALSE)}

proj<-two.d.array(gdf_mean.ac$coords) %*% lda.coord$scaling
LD1<-center_scale(proj[,1])
LD2<-center_scale(proj[,2])
LD3<-center_scale(proj[,3])

projf<-foss %*% lda.coord$scaling
fLD1<-center_scale(projf[,1])
fLD2<-center_scale(projf[,2])
fLD3<-center_scale(projf[,3])


plot(LD1,LD2, 
	 xlab =  "LD1 (61.1%)",
     ylab = "LD2 (27.5%)",
     pch = 20,
     col=c("#cc242f","#00AFBB", "#E7B800","#D07DFC")[gdf_mean.ac$superfamily])
points(fLD1,fLD2,pch=4, cex=2)
text(fLD1,fLD2,
     font = 3,
     labels = rownames(projf),
     pos=3,
     adj=2)

plot(LD1,LD3,  
	 xlab =  "LD1 (61.1%)",
     ylab = "LD3 (11.5%)",
     pch = 20,
     col=c("#cc242f","#00AFBB", "#E7B800","#D07DFC")[gdf_mean.ac$superfamily])
points(fLD1,fLD3, pch=4, cex=2)
text(fLD1,fLD3,
     font = 3,
     labels = rownames(projf),
     pos=3,
     adj=2)

plot(LD2,LD3, 
     pch = 20,
     col=c("#cc242f","#00AFBB", "#E7B800","#D07DFC")[gdf_mean.ac$superfamily])
points(fLD2,fLD3,pch=4, cex=2)
text(fLD2,fLD3,   
	 xlab =  "LD2 (27.5%)",
     ylab = "LD3 (11.5%)",
     font = 3,
     labels = rownames(projf),
     pos=3,
     adj=2)


```



#### Allometry-free (doesn't work because allometry is phylogenetic)

```R
# Use the adjusted shape from all mean species and remove the fossil specimen. Maybe the model of non-allometry is false because the fossil specimen increase allometric variation

cavfree.ac<-two.d.array(adj.shape)[-grep(TRUE,is.na(superfamily_mean)),]
Daf<-data.frame(cavfree.ac, superfamily_mean=droplevels(gdf_mean.ac$superfamily),check.names=FALSE)
Daf<-clean_names(Daf)

### LOOCV


v <-c()

for (i in 1:nrow(Daf)){
  test<- Daf[i,]
  train<- Daf[-i,]
  modele <-lda(formula=superfamily_mean ~ ., data=train)
  pred <-predict(modele, newdata=test)
  #r?cup?re la pr?diction
  v <-c(v,pred$class[1])
}

v<-as.factor(v)
levels(v)<-c("CA","CH","ER","OC")

coordaf.mod<-confusionMatrix(reference=as.factor(Daf$superfamily_mean),data=v)
coordaf.mod

# Attribution of fossil taxa

lda.coordaf<-lda(superfamily_mean~., data=Daf)

fossaf<-data.frame(two.d.array(adj.shape)[grep(TRUE,is.na(superfamily_mean)),],check.names=FALSE)
fossaf<-fossaf[-4,]
colnames(fossaf)<-colnames(Daf)[1:219]

pfossaf<-predict(lda.coordaf,fossaf)
pfossaf
```



```r
#This model directly uses the mean of extant species to adjust shape for allometry but the accuracy of it is <10%...

shape.resid.meanac <- arrayspecs(Mfitallo.ac$residuals,  p=dim(gdf_mean$coords)[1], k=dim(gdf_mean$coords)[2])

Daf<-clean_names(data.frame(two.d.array(adj.shape.meanac), superfamily=gdf_mean.ac$superfamily,check.names=FALSE))


### LOOCV


v <-c()

for (i in 1:nrow(Daf)){
  test<- Daf[i,]
  train<- Daf[-i,]
  modele <-lda(formula=superfamily ~ ., data=train)
  pred <-predict(modele, newdata=test)
  #r?cup?re la pr?diction
  v <-c(v,pred$class[1])
}

v<-as.factor(v)
levels(v)<-c("CA","CH","ER","OC")

coordaf.mod<-confusionMatrix(reference=as.factor(Daf$superfamily),data=v)
coordaf.mod

# Attribution of fossil taxa

lda.coordaf<-lda(superfamily_mean~., data=Daf)

fossaf<-data.frame(two.d.array(adj.shape)[grep(TRUE,is.na(superfamily_mean)),],check.names=FALSE)
fossaf<-fossaf[-4,]
colnames(fossaf)<-colnames(Daf)[1:219]

pfossaf<-predict(lda.coordaf,fossaf)
pfossaf
```

