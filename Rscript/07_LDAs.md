# LDA

```r
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
