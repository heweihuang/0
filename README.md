library("xcms")
library("dplyr")
xset <- xcmsSet("test.mzXML")
list <- read.table("list.csv",header = T); names(list)<-c("mz","RT")
list<-mutate(list,mz1=mz-0.05,mz2=mz+0.05,RT1=(RT-5)*60,RT2=(RT+5)*60)
list<-as.matrix(list)
list_mz<- vector("list",10);list_RT<- vector("list",10);list_XIC<- vector("list",10)
for (i in 1:10){
  png(filename = paste("mz_",i,".png",seq = ""),width=600,height = 480)
  list_mz[[i]]=t(as.matrix(list[i,c(3,4)]));list_RT[[i]]=t(as.matrix(list[i,c(5,6)]))
  list_XIC[[i]] = getEIC(xset,list_mz[[i]],list_RT[[i]],rt = "raw")
  plot(list_XIC[[i]])
  dev.off()
}
