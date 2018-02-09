library(sp)
library(rgdal)
# library(raster)
library(rgeos)

SIA <- readOGR(dsn = "//deqhq1/wqnps/Agriculture/Coordinated_Streamside_Management/CoordinatedStreamsideManagement/GIS/DEQ_SIAs", layer = "SIAs_2018_0124")
SIA <- spTransform(SIA , CRS("+init=epsg:4269"))
wq_limited <- readOGR(dsn = '//deqhq1/wqnps/Agriculture/Coordinated_Streamside_Management/CoordinatedStreamsideManagement/GIS', 
                      layer = 'OR_Streams_WaterQuality_2012_WQLimited', 
                      verbose = FALSE)

wq_limited <- wq_limited[wq_limited$POLLUTANT %in% 
                           c("Temperature", "pH", "Fecal Coliform", "E. Coli",
                             "Enterococcus", "Dissolved Oxygen", 'Sedimentation', "Phosphorus"),]
wq_limited <- spTransform(wq_limited, CRS("+init=epsg:4269"))

for (i in 1:length(unique(SIA$HU12_Name))) {
  sub <- SIA[SIA$HU12_Name == unique(SIA$HU12_Name)[i],]
  
  tryCatch(wq_limited_sub <- wq_limited[sub,], error = function(err) {"woops"})
  
  if(!exists("wq_limited_sub")) {
    next
  }
  
  wq_limited_sub_df <- wq_limited_sub@data
  wq_limited_sub_df$PlanName <- unique(SIA$HU12_Name)[i]
  
  if (i == 1) {
    wq_limited_df <- wq_limited_sub_df
  } else {
    wq_limited_df <- rbind(wq_limited_df, wq_limited_sub_df)
  }
  
  rm(sub, wq_limited_sub, wq_limited_sub_df)
}

#write.csv(wq_limited_df, '//deqhq1/wqnps/Agriculture/Coordinated_Streamside_Management/CoordinatedStreamsideManagement/GIS/wq_limited_df_temp_bact_ph_DO_TP_Sediment_2012.csv', row.names = FALSE)

