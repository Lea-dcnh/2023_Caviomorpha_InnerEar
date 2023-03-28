
```r
library(phytools)

#add tips to phylogeny for fossil specimens
treCAVIOf<-bind.tip(treCAVIO, tip.label="CLIsp",where=57)
treCAVIOf<-bind.tip(treCAVIOf, tip.label="AMBinun",where=58)

treCAVIOf_elaCH<-bind.tip(treCAVIOf, tip.label="ELAobli",where=59)
treCAVIOf_elaOCCH<-bind.tip(treCAVIOf, tip.label="ELAobli",where=58)
treCAVIOf_elaOC<-bind.tip(treCAVIOf, tip.label="ELAobli",where=68)

```

### Setting up the traits of extant species
```r
# Variables in matrix and numerical format for the functions:

#csize
x<-as.matrix(gdf.phylo_mean$Csize) #matrix
gdf.phylo_mean$Csize #numerical

# SCR/IEH

# Number of cochlea turns


#function used with matrix (but can't use contMap() with it)
plotTree.barplot(treCAVIO,x,
                 args.plotTree=list(plot=T))

# Contrast map
cMap<-contMap(treCAVIO,gdf.phylo_mean$Csize,plot=FALSE)
#Set color maps
cMap<-setMap(cMap,c("#FAF9F6","red","black"))
cMap<-setMap(cMap,c("lightblue","purple","black"))
cMap<-setMap(cMap,c("#440154", "#414487", "#2A788E", "#22A884", "#7AD151" ,"#FDE725")) #from viridis palette

# Barplot (for future positionning with par())
obj<-barplot(gdf.phylo_mean$Csize[treCAVIO$tip.label],space=0.5, plot=F)

```

###### Figure extant species-only

```r
# Layouts 
# layout(matrix(c(1,2),1,2),widths=c(0.3,0.7)) #divided left/right
# layout(matrix(c(2,1),2,1),heights=c(0.5,0.6)) #divided up/down

#csize

#Figure extant species-only (680w 550h)
layout(matrix(c(2,1),2,1),heights=c(0.5,0.6)) #divided up/down
plot(cMap, mar=c(0,3,0,1.1),outline=F,fsize=0.7, lwd =3, direction="upwards",tips=obj, offset=4, leg.txt="log(cs)")
par(mar=c(0,3,2.1,1.1))
barplot(gdf.phylo_mean$Csize[treCAVIO$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)


```

### Setting up the traits of Heptaxodontidae
```r
# Traits

#csize
library(dplyr)
gdf_mean$Csize[c("AMBinun", "CLIspxx", "ELAobli")]
meanCS_PhyloFoss<-append(gdf.phylo_mean$Csize,gdf_mean$Csize[c("AMBinun", "CLIspxx", "ELAobli")])
meanCS_PhyloFoss<-meanCS_PhyloFoss[sort(names(meanCS_PhyloFoss))]
names(meanCS_PhyloFoss)<-recode(names(meanCS_PhyloFoss), CLIspxx = "CLIsp")

#barplot par
obj<-barplot(meanCS_PhyloFoss[treCAVIOf_elaCH$tip.label],space=0.5, plot=F)

###### Linear data

ld<-read.csv('/home/lea.dacunha/Work/M2/R/data/cavio/table/Linear_measures_extended.csv')

#remove the specimens not interesting in the phylogeny
morpho_taxa<-ld$Abbrv
tre_label_all<-treCAVIOf_elaCH$tip.label
not_phy<-setdiff(morpho_taxa,tre_label_all)
remove<-grep(paste(not_phy,collapse="|"), morpho_taxa)
ld_phylofoss<-ld[-remove,]

# traits values

# ratio SCR/IEH
mean_SCRratio<-aggregate(SCR_ratio~Abbrv,ld_phylofoss,mean)
x<-mean_SCRratio$Abbrv
mean_SCRratio<-as.vector(mean_SCRratio[,2])
names(mean_SCRratio)<-x

# Number of turns
mean_NUMturns<-aggregate(Number.of.turns~Abbrv,ld_phylofoss,mean)
x<-mean_NUMturns$Abbrv
mean_NUMturns<-as.vector(mean_NUMturns[,2])
names(mean_NUMturns)<-x

# Cochlear_ratio
mean_COCHratio<-aggregate(Cochlear.ratio~Abbrv,ld_phylofoss,mean)
x<-mean_COCHratio$Abbrv
mean_COCHratio<-as.vector(mean_COCHratio[,2])
names(mean_COCHratio)<-x


meanCoSize<-aggregate(CoSize~Abbrv,ld_phylofoss,mean)
mean_ANGLal<-aggregate(Angle.ASC.LSC~Abbrv,ld_phylofoss,mean)
mean_ANGlepl<-aggregate(Angle.PSC.LSC~Abbrv,ld_phylofoss,mean)
mean_ANGap<-aggregate(Angle.ASC.PSC~Abbrv,ld_phylofoss,mean)

```

### Figures - Evolutive Scenarios Heptaxodontidae (680w 550h)
###### log(cs) and centroid size
```r
#size

# Elasmodontomys CH
cMap<-contMap(treCAVIOf_elaCH,log(meanCS_PhyloFoss), direction = "upwards",outline=FALSE, fsize=0.7,plot=FALSE)
cMap<-setMap(cMap,c("#440154", "#414487", "#2A788E", "#22A884", "#7AD151" ,"#FDE725"))

obj<-barplot(meanCS_PhyloFoss[treCAVIOf_elaCH$tip.label],space=0.5, plot=F)

layout(matrix(c(2,1),2,1),heights=c(0.5,0.6)) 
barplot(meanCS_PhyloFoss[treCAVIOf_elaCH$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)
plot(cMap, mar=c(0,3,0,1.1),outline=F,fsize=0.7, lwd =3, direction="upwards",tips=obj, offset=4, leg.txt="log(cs)")
par(mar=c(0,3,2.1,1.1))
barplot(meanCS_PhyloFoss[treCAVIOf_elaCH$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)

```

```r
# Elasmodontomys OC
cMap<-contMap(treCAVIOf_elaOC,log(meanCS_PhyloFoss), direction = "upwards",outline=FALSE, fsize=0.7,plot=FALSE)
cMap<-setMap(cMap,c("#440154", "#414487", "#2A788E", "#22A884", "#7AD151" ,"#FDE725"))

obj<-barplot(meanCS_PhyloFoss[treCAVIOf_elaOC$tip.label],space=0.5, plot=F)

layout(matrix(c(2,1),2,1),heights=c(0.5,0.6)) 
barplot(meanCS_PhyloFoss[treCAVIOf_elaOC$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)
plot(cMap, mar=c(0,3,0,1.1),outline=F,fsize=0.7, lwd =3, direction="upwards",tips=obj, offset=4, leg.txt="log(cs)")
par(mar=c(0,3,2.1,1.1))
barplot(meanCS_PhyloFoss[treCAVIOf_elaOC$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)
```

```r
# Elasmodontomys OCCH
cMap<-contMap(treCAVIOf_elaOCCH,log(meanCS_PhyloFoss), direction = "upwards",outline=FALSE, fsize=0.7,plot=FALSE)
cMap<-setMap(cMap,c("#440154", "#414487", "#2A788E", "#22A884", "#7AD151" ,"#FDE725"))

obj<-barplot(meanCS_PhyloFoss[treCAVIOf_elaOCCH$tip.label],space=0.5, plot=F)

layout(matrix(c(2,1),2,1),heights=c(0.5,0.6)) 
barplot(meanCS_PhyloFoss[treCAVIOf_elaOCCH$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)
plot(cMap, mar=c(0,3,0,1.1),outline=F,fsize=0.7, lwd =3, direction="upwards",tips=obj, offset=4, leg.txt="log(cs)")
par(mar=c(0,3,2.1,1.1))
barplot(meanCS_PhyloFoss[treCAVIOf_elaOCCH$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)

```
###### SCR/IEH ratio and barplot csize
```r
#ElaCH
cMap<-contMap(treCAVIOf_elaCH,mean_SCRratio, direction = "upwards",outline=FALSE, fsize=0.7,plot=FALSE)
cMap<-setMap(cMap,c("#440154", "#414487", "#2A788E", "#22A884", "#7AD151" ,"#FDE725"))

#obj<-barplot(mean_SCRratio[treCAVIOf_elaCH$tip.label],space=0.5, plot=F)
obj<-barplot(meanCS_PhyloFoss[treCAVIOf_elaCH$tip.label],space=0.5, plot=F)

layout(matrix(c(2,1),2,1),heights=c(0.5,0.6)) 
barplot(meanCS_PhyloFoss[treCAVIOf_elaCH$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)
plot(cMap, mar=c(0,3,0,1.1),outline=F,fsize=0.7, lwd =3, direction="upwards",tips=obj, offset=4, leg.txt="SCR/IEH ratio")
par(mar=c(0,3,2.1,1.1))
barplot(meanCS_PhyloFoss[treCAVIOf_elaCH$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)
```

```r
#ElaOC
cMap<-contMap(treCAVIOf_elaOC,mean_SCRratio, direction = "upwards",outline=FALSE, fsize=0.7,plot=FALSE)
cMap<-setMap(cMap,c("#440154", "#414487", "#2A788E", "#22A884", "#7AD151" ,"#FDE725"))

#obj<-barplot(mean_SCRratio[treCAVIOf_elaOC$tip.label],space=0.5, plot=F)
obj<-barplot(meanCS_PhyloFoss[treCAVIOf_elaOC$tip.label],space=0.5, plot=F)

layout(matrix(c(2,1),2,1),heights=c(0.5,0.6)) 
barplot(meanCS_PhyloFoss[treCAVIOf_elaOC$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)
plot(cMap, mar=c(0,3,0,1.1),outline=F,fsize=0.7, lwd =3, direction="upwards",tips=obj, offset=4, leg.txt="SCR/IEH ratio")
par(mar=c(0,3,2.1,1.1))
barplot(meanCS_PhyloFoss[treCAVIOf_elaOC$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)

```

```r
#ElaOCCH
cMap<-contMap(treCAVIOf_elaOCCH,mean_SCRratio, direction = "upwards",outline=FALSE, fsize=0.7,plot=FALSE)
cMap<-setMap(cMap,c("#440154", "#414487", "#2A788E", "#22A884", "#7AD151" ,"#FDE725"))

#obj<-barplot(mean_SCRratio[treCAVIOf_elaOCCH$tip.label],space=0.5, plot=F)
obj<-barplot(meanCS_PhyloFoss[treCAVIOf_elaOCCH$tip.label],space=0.5, plot=F)

layout(matrix(c(2,1),2,1),heights=c(0.5,0.6)) 
barplot(meanCS_PhyloFoss[treCAVIOf_elaOCCH$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)
plot(cMap, mar=c(0,3,0,1.1),outline=F,fsize=0.7, lwd =3, direction="upwards",tips=obj, offset=4, leg.txt="SCR/IEH ratio")
par(mar=c(0,3,2.1,1.1))
barplot(meanCS_PhyloFoss[treCAVIOf_elaOCCH$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)
```
###### Number of turns and barplot csize

```r
#ElaOCCH
cMap<-contMap(treCAVIOf_elaOCCH,mean_NUMturns, direction = "upwards",outline=FALSE, fsize=0.7,plot=FALSE)
cMap<-setMap(cMap,c("#440154", "#414487", "#2A788E", "#22A884", "#7AD151" ,"#FDE725"))

#obj<-barplot(mean_SCRratio[treCAVIOf_elaCH$tip.label],space=0.5, plot=F)
obj<-barplot(mean_NUMturns[treCAVIOf_elaOCCH$tip.label],space=0.5, plot=F)

layout(matrix(c(2,1),2,1),heights=c(0.5,0.6)) 
barplot(meanCS_PhyloFoss[treCAVIOf_elaOCCH$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)
plot(cMap, mar=c(0,3,0,1.1),outline=F,fsize=0.7, lwd =3, direction="upwards",tips=obj, offset=4, leg.txt="number of turns")
par(mar=c(0,3,2.1,1.1))
barplot(meanCS_PhyloFoss[treCAVIOf_elaOCCH$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)
```

```r
#ElaCH
cMap<-contMap(treCAVIOf_elaCH,mean_NUMturns, direction = "upwards",outline=FALSE, fsize=0.7,plot=FALSE)
cMap<-setMap(cMap,c("#440154", "#414487", "#2A788E", "#22A884", "#7AD151" ,"#FDE725"))

#obj<-barplot(mean_SCRratio[treCAVIOf_elaCH$tip.label],space=0.5, plot=F)
obj<-barplot(mean_NUMturns[treCAVIOf_elaCH$tip.label],space=0.5, plot=F)

layout(matrix(c(2,1),2,1),heights=c(0.5,0.6)) 
barplot(meanCS_PhyloFoss[treCAVIOf_elaCH$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)
plot(cMap, mar=c(0,3,0,1.1),outline=F,fsize=0.7, lwd =3, direction="upwards",tips=obj, offset=4, leg.txt="number of turns")
par(mar=c(0,3,2.1,1.1))
barplot(meanCS_PhyloFoss[treCAVIOf_elaCH$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)
```

```r
#ElaOC
cMap<-contMap(treCAVIOf_elaOC,mean_NUMturns, direction = "upwards",outline=FALSE, fsize=0.7,plot=FALSE)
cMap<-setMap(cMap,c("#440154", "#414487", "#2A788E", "#22A884", "#7AD151" ,"#FDE725"))

#obj<-barplot(mean_SCRratio[treCAVIOf_elaCH$tip.label],space=0.5, plot=F)
obj<-barplot(mean_NUMturns[treCAVIOf_elaOC$tip.label],space=0.5, plot=F)

layout(matrix(c(2,1),2,1),heights=c(0.5,0.6)) 
barplot(meanCS_PhyloFoss[treCAVIOf_elaOC$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)
plot(cMap, mar=c(0,3,0,1.1),outline=F,fsize=0.7, lwd =3, direction="upwards",tips=obj, offset=4, leg.txt="number of turns")
par(mar=c(0,3,2.1,1.1))
barplot(meanCS_PhyloFoss[treCAVIOf_elaOC$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)
```

###### log(cochlear ratio) and barplot csize

```r
#ElaCH
cMap<-contMap(treCAVIOf_elaCH,log(log(mean_COCHratio)), direction = "upwards",outline=FALSE, fsize=0.7,plot=FALSE)
cMap<-setMap(cMap,c("#440154", "#414487", "#2A788E", "#22A884", "#7AD151" ,"#FDE725"))

#obj<-barplot(mean_SCRratio[treCAVIOf_elaCH$tip.label],space=0.5, plot=F)
obj<-barplot(mean_NUMturns[treCAVIOf_elaOCCH$tip.label],space=0.5, plot=F)

layout(matrix(c(2,1),2,1),heights=c(0.5,0.6)) 
barplot(meanCS_PhyloFoss[treCAVIOf_elaCH$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)
plot(cMap, mar=c(0,3,0,1.1),outline=F,fsize=0.7, lwd =3, direction="upwards",tips=obj, offset=4, leg.txt="log(cochlear ratio)")
par(mar=c(0,3,2.1,1.1))
barplot(meanCS_PhyloFoss[treCAVIOf_elaCH$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)
```

###### log(cochlear ratio) and barplot number of turns
```r
#ElaCH
 cMap<-contMap(treCAVIOf_elaCH,log(mean_COCHratio), direction = "upwards",outline=FALSE, fsize=0.7,plot=FALSE)
 cMap<-setMap(cMap,c("#440154", "#414487", "#2A788E", "#22A884", "#7AD151" ,"#FDE725"))
 
   #obj<-barplot(mean_SCRratio[treCAVIOf_elaCH$tip.label],space=0.5, plot=F)
   obj<-barplot(mean_NUMturns[treCAVIOf_elaOCCH$tip.label],space=0.5, plot=F)
 
   layout(matrix(c(2,1),2,1),heights=c(0.5,0.6)) 
 barplot(mean_NUMturns[treCAVIOf_elaCH$tip.label],names.arg="",
                   xlim=range(obj),
                   space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)
 plot(cMap, mar=c(0,3,0,1.1),outline=F,fsize=0.7, lwd =3, direction="upwards",tips=obj, offset=4, leg.txt="log(cochlear ratio)")
 par(mar=c(0,3,2.1,1.1))
 barplot(mean_NUMturns[treCAVIOf_elaCH$tip.label],names.arg="",
                   xlim=range(obj),
                   ylab=expression(paste("number or turns")),
                   space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)

```

```r
# Elasmodontomys OC
cMap<-contMap(treCAVIOf_elaOC,log(mean_COCHratio), direction = "upwards",outline=FALSE, fsize=0.7,plot=FALSE)
cMap<-setMap(cMap,c("#440154", "#414487", "#2A788E", "#22A884", "#7AD151" ,"#FDE725"))

obj<-barplot(mean_NUMturns[treCAVIOf_elaOC$tip.label],space=0.5, plot=F)

layout(matrix(c(2,1),2,1),heights=c(0.5,0.6)) 
barplot(mean_NUMturns[treCAVIOf_elaOC$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)
plot(cMap, mar=c(0,3,0,1.1),outline=F,fsize=0.7, lwd =3, direction="upwards",tips=obj, offset=4, leg.txt="log(cochlear ratio)")
par(mar=c(0,3,2.1,1.1))
barplot(mean_NUMturns[treCAVIOf_elaOC$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)
```

```r
# Elasmodontomys OCCH
cMap<-contMap(treCAVIOf_elaOCCH,log(mean_COCHratio), direction = "upwards",outline=FALSE, fsize=0.7,plot=FALSE)
cMap<-setMap(cMap,c("#440154", "#414487", "#2A788E", "#22A884", "#7AD151" ,"#FDE725"))

obj<-barplot(mean_NUMturns[treCAVIOf_elaOCCH$tip.label],space=0.5, plot=F)

layout(matrix(c(2,1),2,1),heights=c(0.5,0.6)) 
barplot(mean_NUMturns[treCAVIOf_elaOCCH$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)
plot(cMap, mar=c(0,3,0,1.1),outline=F,fsize=0.7, lwd =3, direction="upwards",tips=obj, offset=4, leg.txt="log(cochlear ratio)")
par(mar=c(0,3,2.1,1.1))
barplot(mean_NUMturns[treCAVIOf_elaOCCH$tip.label],names.arg="",
        xlim=range(obj),
        space=0.5,las=2,cex.axis=0.8,cex.lab=1.1)

```