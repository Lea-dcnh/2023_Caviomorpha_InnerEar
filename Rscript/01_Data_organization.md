```r
setwd("data")

Dgeom<-readland.tps("Caviomorpha_extended_IE.tps",specID = "imageID")
genus<-as.factor(substr(dimnames(Dgeom)[[3]], 1,3))		
species<-as.factor(substr(dimnames(Dgeom)[[3]], 1,7))
superfamily<-as.factor(substr(dimnames(Dgeom)[[3]], 9,10))
superfamily[superfamily=="NA"]<- NA
superfamily<-droplevels(superfamily)
superfamilyNA<-as.factor(substr(dimnames(Dgeom)[[3]], 9,10))
family<-as.factor(substr(dimnames(Dgeom)[[3]], 12,14))
family[family=="NAx"]<- NA
family<-droplevels(family)
familyNA<-as.factor(substr(dimnames(Dgeom)[[3]], 12,14))
names<-paste(substr(dimnames(Dgeom)[[3]],1,1),substr(dimnames(Dgeom)[[3]],4,6), sep="")
```

# All specimens

```r
# Remove redundant landmarks and input curve file
Dgeom_reduced<-Dgeom[-c(1:17,72,87),,1:dim(Dgeom)[3]]
dat_curv<-as.matrix(read.csv("Sliding_Curve_InnerEar.txt",sep=',',header=T))
dat_curv_all<-as.matrix(read.csv("Model_Pred_Curves.txt",sep=',',header=T))

#Landmarks of the SCs and Cochlea only
Dgeom_cochlea <- Dgeom_reduced[c(1:20),,1:dim(Dgeom)[3]]
Dgeom_CS <-Dgeom_reduced[c(21:40,41:54,55:68,69:73),,1:dim(Dgeom)[3]]

# Generalized Procruste Analysis of all specimens
cavio.gpa<-gpagen(Dgeom_reduced,curves=dat_curv, ProcD= TRUE)


###### Mean size & shape all

# Calculate the mean shape and mean csize for each species

# Mean GPA-aligned Procrustes coordinates
# mshape() uses GPA-aligned data
lvl.group<-levels(species)
group<-species

p <- dim(cavio.gpa$coords)[1]
k <- dim(cavio.gpa$coords)[2]
n <- dim(cavio.gpa$coords)[3]
meanDgeomR<- array(NA, dim=c(p,k,length(lvl.group)))
dimnames(meanDgeomR)[[3]] <- lvl.group

for (i in 1:length(lvl.group)){
  grp <- cavio.gpa$coords[,,which(group==lvl.group[i])]    
  meanDgeomR[,,i] <- mshape(grp)
}

# Mean Csize of species
d<-as.data.frame(cavio.gpa$Csize)
row.names(d)<-NULL
data<-cbind(species,d)
data<-aggregate(cavio.gpa$Csize~species,data,mean)

meanDgeomcs<-as.numeric(data$`cavio.gpa$Csize`)
names(meanDgeomcs)<-dimnames(meanDgeomR)[[3]]


# Remove duplicates in the variables to make it correspond to the mean species
t<-data.frame(cbind(as.vector(species),as.vector(superfamily)))
tn<-t[!duplicated(t),]
superfamily_mean<-as.factor(tn$X2)

t<-data.frame(cbind(as.vector(species),as.vector(superfamilyNA)))
tn<-t[!duplicated(t),]
superfamily_meanNA<-as.factor(tn$X2)

t<-data.frame(cbind(as.vector(species),as.vector(family)))
tn<-t[!duplicated(t),]
family_mean<-as.factor(tn$X2)

t<-data.frame(cbind(as.vector(species),as.vector(familyNA)))
tn<-t[!duplicated(t),]
family_meanNA<-as.factor(tn$X2)

```

# Removal of fossil taxa of unknown affinities 

```r
# Because we remove specimens, we have to adapt factors levels with droplevels()
superfamily.ac<-droplevels(superfamily[-grep(TRUE,is.na(superfamily))])
family.ac<-droplevels(family[-grep(TRUE,is.na(superfamily))])
species.ac<-droplevels(species[-grep(TRUE,is.na(superfamily))])

# Setup coords & GPA
Dgeom.ac_reduced<-Dgeom_reduced[,,-grep(TRUE,is.na(superfamily))]
cavio.ext_gpa<- gpagen(Dgeom.ac_reduced,curves=dat_curv, ProcD= TRUE)


###### Mean size & shape 


t<-data.frame(cbind(as.vector(species.ac),as.vector(superfamily.ac)))
tn<-t[!duplicated(t),]
superfamily_mean.ac<-as.factor(tn$X2)
t<-data.frame(cbind(as.vector(species.ac),as.vector(family.ac)))
tn<-t[!duplicated(t),]
family_mean.ac<-as.factor(tn$X2)

# Mean GPA-aligned Procrustes coordinates
# mshape() uses GPA-aligned data
lvl.group<-levels(species.ac)
group<-species.ac

p <- dim(cavio.ext_gpa$coords)[1]
k <- dim(cavio.ext_gpa$coords)[2]
n <- dim(cavio.ext_gpa$coords)[3]
meanDgeomR.ac<- array(NA, dim=c(p,k,length(lvl.group)))
dimnames(meanDgeomR.ac)[[3]] <- lvl.group

for (i in 1:length(lvl.group)){
  grp <- cavio.ext_gpa$coords[,,which(group==lvl.group[i])]    
  meanDgeomR.ac[,,i] <- mshape(grp)
}

# Mean Csize of species
d<-as.data.frame(cavio.ext_gpa$Csize)
row.names(d)<-NULL
data<-cbind(species.ac,d)
data<-aggregate(cavio.ext_gpa$Csize~species.ac,data,mean)

meanDgeomcs.ac<-as.numeric(data$`cavio.ext_gpa$Csize`)
names(meanDgeomcs.ac)<-dimnames(meanDgeomR.ac)[[3]]

```

# Extant species only


```r
grep(TRUE,is.na(superfamily))
ext.foss<-c("BORoffe", "BORtorr", "BROvora", "HETinsu", "HEXphen", "ISOport", "NEOacre", "RHIlemk")

grep(paste(ext.foss,collapse="|"),species)
remove_extinct<-(sort(c(grep(TRUE,is.na(superfamily)),grep(paste(ext.foss,collapse="|"),species))))

# Variables adapted for specimen removal
superfamily.extant<-droplevels(superfamily[-remove_extinct])
family.extant<-droplevels(family[-remove_extinct])
species.extant<-droplevels(species[-remove_extinct]) 

# Setup coords & GPA
Dgeom.extant_reduced<-Dgeom_reduced[,,-remove_extinct]
cavio.extantonly_gpa<- gpagen(Dgeom.extant_reduced,curves=dat_curv, ProcD= TRUE)

# Variables adapted for mean
t<-data.frame(cbind(as.vector(species.extant),as.vector(superfamily.extant)))
tn<-t[!duplicated(t),]
superfamily_mean.extant<-as.factor(tn$X2)

t<-data.frame(cbind(as.vector(species.extant),as.vector(family.extant)))
tn<-t[!duplicated(t),]
family_mean.extant<-as.factor(tn$X2)

# Mean GPA-aligned Procrustes coordinates
# mshape() uses GPA-aligned data
lvl.group<-levels(species.extant)
group<-species.extant

p <- dim(cavio.extantonly_gpa$coords)[1]
k <- dim(cavio.extantonly_gpa$coords)[2]
n <- dim(cavio.extantonly_gpa$coords)[3]
meanDgeomR.extant<- array(NA, dim=c(p,k,length(lvl.group)))
dimnames(meanDgeomR.extant)[[3]] <- lvl.group

for (i in 1:length(lvl.group)){
  grp <- cavio.extantonly_gpa$coords[,,which(group==lvl.group[i])]    
  meanDgeomR.extant[,,i] <- mshape(grp)
}

# Mean Csize of species
d<-as.data.frame(cavio.extantonly_gpa$Csize)
row.names(d)<-NULL
data<-cbind(species.extant,d)
data<-aggregate(cavio.extantonly_gpa$Csize~species.extant,data,mean)

meanDgeomcs.extant<-as.numeric(data$`cavio.extantonly_gpa$Csize`)
names(meanDgeomcs.extant)<-dimnames(meanDgeomR.extant)[[3]]



```






# Specimens only in phylogeny

```r
# Setup topology 

tre<-read.tree("CAVIO_MCC_short.tre")
tip<-levels(species)
tre_label_all<-tre$tip.label


Dgeom<-readland.tps("Caviomorpha_extended_IE.tps",specID = "imageID")
morpho_taxa<-substr(dimnames(Dgeom)[[3]],1,7)

# Setting differences between sampling and phylogeny

not_phy<-setdiff(morpho_taxa,tre_label_all)
to_drop<-setdiff(tre_label_all,morpho_taxa)
treCAVIO<-drop.tip(tre,to_drop)

# Searching and removing taxa not in the phylogeny
not_phy
remove<-grep(paste(not_phy,collapse="|"), morpho_taxa) #Find the posisiton of the taxa of not_phy
Dgeom_phylo <- Dgeom[,,-remove]

# Factors adjusted to correspond to the phylogeny
genus_phy<-as.factor(substr(dimnames(Dgeom_phylo)[[3]], 1,3))		
species_phy<-as.factor(substr(dimnames(Dgeom_phylo)[[3]], 1,7))	       
superfamily_phy<-as.factor(substr(dimnames(Dgeom_phylo)[[3]], 9,10))
family_phy<-as.factor(substr(dimnames(Dgeom_phylo)[[3]], 12,14))

# Remove redundant landmarks
Dgeomphy_reduced<-Dgeom_phylo[-c(1:17,72,87),,1:dim(Dgeom_phylo)[3]]

# Generalized Procruste Analysis of all specimens
caviophy.gpa<-gpagen(Dgeomphy_reduced,curves=dat_curv, ProcD= TRUE)



###### Mean size & shape phylo


# Mean shape of species in the phylogeny 
lvl.group<-levels(species_phy)
group<-species_phy

p <- dim(caviophy.gpa$coords)[1]
k <- dim(caviophy.gpa$coords)[2]
n <- dim(caviophy.gpa$coords)[3]
meanDgeomphyR<- array(NA, dim=c(p,k,length(lvl.group)))
dimnames(meanDgeomphyR)[[3]] <- lvl.group

for (i in 1:length(lvl.group)){
  grp <- caviophy.gpa$coords[,,which(group==lvl.group[i])]    
  meanDgeomphyR[,,i] <- mshape(grp)
}

meanDgeomphyR

# Mean Csize of species
d<-as.data.frame(caviophy.gpa$Csize)
row.names(d)<-NULL
data<-cbind(species_phy,d)
data<-aggregate(caviophy.gpa$Csize~species_phy,data,mean)

meanDgeomphycs<-as.numeric(data$`caviophy.gpa$Csize`)
names(meanDgeomphycs)<-dimnames(meanDgeomphyR)[[3]]




# Remove duplicates in the variables to make it correspond to the mean species
t<-data.frame(cbind(as.vector(species_phy),as.vector(superfamily_phy)))
tn<-t[!duplicated(t),]
superfamily_phy_mean<-as.factor(tn$X2)
t<-data.frame(cbind(as.vector(species_phy),as.vector(family_phy)))
tn<-t[!duplicated(t),]
family_phy_mean<-as.factor(tn$X2)
```

## For SCs and cochlea only

```r
#Landmarks of the SCs and Cochlea only
Dgeomphy_cochlea <- Dgeomphy_reduced[c(1:20),,1:dim(Dgeomphy_reduced)[3]]
datco<-read.csv("Sliding_Curve_Cochlea.txt",sep=',',header=T)
dat_curvco<-as.matrix(datco)
caviophyco.gpa<-gpagen(Dgeomphy_cochlea,curves=dat_curvco, ProcD= TRUE)

Dgeomphy_CS<-Dgeomphy_reduced[c(21:40,41:54,55:68,69:73),,1:dim(Dgeomphy_reduced)[3]]
datcs<-read.csv("Sliding_Curve_CS.txt",sep=',',header=T)
dat_curvcs<-as.matrix(datcs)
caviophycs.gpa<-gpagen(Dgeomphy_CS,curves=dat_curvcs, ProcD= TRUE)
```

```r

# cochlea
lvl.group<-levels(species_phy)
group<-species_phy

p <- dim(caviophyco.gpa$coords)[1]
k <- dim(caviophyco.gpa$coords)[2]
n <- dim(caviophyco.gpa$coords)[3]
meanDgeomphyR_coch<- array(NA, dim=c(p,k,length(lvl.group)))
dimnames(meanDgeomphyR_coch)[[3]] <- lvl.group

for (i in 1:length(lvl.group)){
  grp <- caviophyco.gpa$coords[,,which(group==lvl.group[i])]    
  meanDgeomphyR_coch[,,i] <- mshape(grp)
}
# Mean Csize of species
d<-as.data.frame(caviophyco.gpa$Csize)
row.names(d)<-NULL
data<-cbind(species_phy,d)
data<-aggregate(caviophyco.gpa$Csize~species_phy,data,mean)

meanDgeomphycs_coch<-as.numeric(data$`caviophyco.gpa$Csize`)
names(meanDgeomphycs_coch)<-dimnames(meanDgeomphyR_coch)[[3]]

#SCs
lvl.group<-levels(species_phy)
group<-species_phy

p <- dim(caviophycs.gpa$coords)[1]
k <- dim(caviophycs.gpa$coords)[2]
n <- dim(caviophycs.gpa$coords)[3]
meanDgeomphyR_SC<- array(NA, dim=c(p,k,length(lvl.group)))
dimnames(meanDgeomphyR_SC)[[3]] <- lvl.group

for (i in 1:length(lvl.group)){
  grp <- caviophycs.gpa$coords[,,which(group==lvl.group[i])]    
  meanDgeomphyR_SC[,,i] <- mshape(grp)
}
# Mean Csize of species
d<-as.data.frame(caviophyco.gpa$Csize)
row.names(d)<-NULL
data<-cbind(species_phy,d)
data<-aggregate(caviophyco.gpa$Csize~species_phy,data,mean)

meanDgeomphycs_SC<-as.numeric(data$`caviophyco.gpa$Csize`)
names(meanDgeomphycs_SC)<-dimnames(meanDgeomphyR_SC)[[3]]


```

