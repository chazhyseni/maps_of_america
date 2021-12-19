<html>

<head>
<title>Rayshader</title>
</head>

<body>

<p>
  <b>Coming up with the best color palette for plotting the N. and S. American elevation rasters with the rayshader package
  </b>
</p>
<!--begin.rcode, message=FALSE, warning=FALSE, eval=FALSE
###required packages
if (!require("tidyverse")) install.packages("tidyverse")
if (!require("sp")) install.packages("sp")
if (!require("raster")) install.packages("raster")
if (!require("elevatr")) install.packages("elevatr")
if (!require("rayshader")) install.packages("rayshader")
if (!require("viridis")) install.packages("viridis")
if (!require("scales")) install.packages("scales")


###loading data
#S.Am point locations
mntns.sam <- read_tsv("Geographic Data/SAm/coords_mntns.txt")
#N.Am point locations
mntns.nam <- read_tsv("Geographic Data/NAm/coords_mntns.txt")

#longitude = column 5; latitude = column 4
americas.spp <- SpatialPoints(rbind(mntns.sam[,c(5,4)], mntns.nam[,c(5,4)]), proj4string = CRS("EPSG:4326"))
##get elevation
americas.elev <- get_elev_raster(americas.spp, z=5, src="aws")
##anything below 1 meter = NA
americas.elev[americas.elev < 1] = NA



###plots with rayshader

##color palette
par(mfrow=c(1,1),fg="gray50",pty='m',bty='o',mar=c(4.5,4.5,4.5,1),cex.main=1.3,cex.axis=1.1,cex.lab=1.2)
rokt <- rev(viridis::rocket(21))
mak <- rev(viridis::mako(21))
virds <- rev(viridis::viridis(21))
#scales::show_col(c(rokt[3], rokt[21], rokt[9], rokt[15], mak[8]))
#scales::show_col(c(virds[3], virds[21], virds[9], virds[15], mak[8]))
#scales::show_col(c(rokt[3], rokt[21], virds[9], virds[15], mak[8]))

##elevation matrix
americas.mat <- raster_to_matrix(americas.elev)
americas.mat[is.na(americas.mat)]=0
americas.mat <- round(americas.mat,0)


end.rcode-->

<p>v.1</p>

<!--begin.rcode, v1, fig.width=25, fig.height=33, eval=FALSE, message=FALSE
americas.mat %>% 
  sphere_shade(texture = create_texture(lightcolor = rokt[3], shadowcolor = rokt[21], leftcolor = rokt[9], rightcolor = rokt[15], centercolor = mak[8]), sunangle=225) %>% 
  add_water(detect_water(americas.mat, cutoff = 0.95), color = "steelblue3") %>% 
  plot_map()

end.rcode-->

<!--begin.rcode out.width="90%", echo=FALSE, eval=TRUE 
knitr::include_graphics("figure/v1-1.png")
end.rcode-->


<p>v.2</p>

<!--begin.rcode, v2, fig.width=25, fig.height=33, eval=FALSE, message=FALSE
americas.mat %>% 
  sphere_shade(texture = create_texture(lightcolor = virds[3], shadowcolor = virds[21], leftcolor = virds[9], rightcolor = virds[15], centercolor = mak[8]), sunangle=225) %>% 
  add_water(detect_water(americas.mat, cutoff = 0.95), color = "steelblue3") %>% 
  plot_map()

end.rcode-->

<!--begin.rcode out.width="90%", echo=FALSE, eval=TRUE 
knitr::include_graphics("figure/v2-1.png")
end.rcode-->


<p>v.3</p>

<!--begin.rcode, v3, fig.width=25, fig.height=33, eval=FALSE, message=FALSE
americas.mat %>% 
  sphere_shade(texture = create_texture(lightcolor = rokt[3], shadowcolor = rokt[21], leftcolor = virds[9], rightcolor = virds[15], centercolor = mak[8]), sunangle=225) %>% 
  add_water(detect_water(americas.mat, min_area = 200), color = "steelblue3") %>% 
  plot_map()

end.rcode-->

<!--begin.rcode out.width="90%", echo=FALSE, eval=TRUE 
knitr::include_graphics("figure/v3-1.png")
end.rcode-->


</body>
</html>
