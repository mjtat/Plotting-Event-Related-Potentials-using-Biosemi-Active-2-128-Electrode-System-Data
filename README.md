# Plotting Event Related Potentials using Biosemi Active-2 128-Electrode System Data

**Open the .Rmd file, press CTRL+ALT+R to run the entire script.**

**Load library used to read in data and manipulate data.**

    library(plyr)

**First set your working directory.** *Press CTRL+SHIFT+H*

**Reads directory where files are located. Note that the "path" argument
needs to be set to the directory where all the txt files exist.** Note
the path argument below needs to change if the files are in a different
place.

    filesCD <- list.files(path = "/media/mihcelle/ChelleUSB/SubMemory 7.20/CD/", pattern="*.txt", full.names=TRUE)
    filesFAM <- list.files(path = "/media/mihcelle/ChelleUSB/SubMemory 7.20/FAM/",pattern="*.txt", full.names=TRUE)
    filesDK <- list.files(path = "/media/mihcelle/ChelleUSB/SubMemory 7.20/DK/", pattern="*.txt", full.names=TRUE)

**\# Imports all files TEXT files into R, store data frames in a large
list object named `dataCD`, `dataFAM`, and `dataDK`.**

    dataCD<-lapply(filesCD, read.delim, header = FALSE)
    dataFAM<-lapply(filesFAM, read.delim, header = FALSE)
    dataDK<-lapply(filesDK, read.delim, header = FALSE)

**Set the subject IDs.**

    IDs <- c("1115", "1140", "1181", "1220", "1337", "1414", "1424", "1498", "1513", "1581", "1651", "1775", "1833", "2000","2001", "2003", "2004", "2005", "2007", "2010", "2011", "2013", "2014", "2015" ,"2016", "2017", "2018", "2020","2022", "2023", "2025" ,"2027", "2028", "2029")

**This next part snips unnecessary electrode data.**

    dataCD<-lapply(dataCD, function(x) x[, -c(129:140)])
    dataFAM<-lapply(dataFAM, function(x) x[,-c(129:140)])
    dataDK<-lapply(dataDK, function(x) x[,-c(129:140)])

**Let's check that it did it properly.**

    ncol(dataCD[[1]])

    ## [1] 128

**The `ncol()` function returns 128 columns.** This corresponds to 128
electrodes.

**Now we need to find the grand means for each response type.** This
means finding the mean for each individual element (i.e., individuall
spreadsheet cell) across *all subjects*. This will spit out matrices
titled Mean\_CD, Mean\_FAM, and Mean\_DK.

    Mean_CD = aaply(laply(dataCD, as.matrix),c(2,3), mean)
    Mean_FAM = aaply(laply(dataFAM, as.matrix),c(2,3), mean)
    Mean_DK= aaply(laply(dataDK, as.matrix),c(2,3), mean)

    Mean_CD <- as.data.frame(Mean_CD)
    Mean_FAM <- as.data.frame(Mean_FAM)
    Mean_DK <- as.data.frame(Mean_DK)

**Let's just check and make sure it's completed properly...**

    head(Mean_CD,5)

    ##              V1           V2           V3           V4           V5
    ## 1  4.103468e-08 2.228300e-07 1.489553e-07 1.805165e-07 1.263515e-07
    ## 2  3.366382e-08 2.077146e-07 1.402744e-07 1.704094e-07 1.198974e-07
    ## 3  1.355176e-08 1.654538e-07 1.140310e-07 1.393947e-07 9.953441e-08
    ## 4 -1.429353e-08 1.052385e-07 7.140706e-08 8.901676e-08 6.564235e-08
    ## 5 -4.384382e-08 4.015735e-08 1.874441e-08 2.934941e-08 2.343891e-08
    ##              V6            V7           V8            V9           V10
    ## 1 -7.093794e-08 -3.760353e-08 9.295088e-08 -6.829618e-08 -2.815968e-07
    ## 2 -7.469882e-08 -4.192794e-08 9.399935e-08 -6.422706e-08 -2.608305e-07
    ## 3 -8.460882e-08 -5.434824e-08 9.334559e-08 -5.470588e-08 -2.063335e-07
    ## 4 -9.693203e-08 -7.220412e-08 8.387559e-08 -4.560085e-08 -1.373312e-07
    ## 5 -1.073703e-07 -9.144529e-08 6.209500e-08 -4.247744e-08 -7.643235e-08
    ##             V11           V12           V13           V14           V15
    ## 1 -3.687167e-07 -2.866782e-07 -2.875587e-07 -3.569044e-07 -2.987374e-07
    ## 2 -3.519382e-07 -2.673503e-07 -2.689071e-07 -3.376285e-07 -2.823912e-07
    ## 3 -3.062265e-07 -2.137406e-07 -2.181768e-07 -2.846192e-07 -2.382309e-07
    ## 4 -2.446597e-07 -1.415618e-07 -1.495476e-07 -2.111388e-07 -1.777835e-07
    ## 5 -1.857218e-07 -7.921938e-08 -8.430315e-08 -1.363654e-07 -1.147011e-07
    ##             V16           V17           V18          V19          V20
    ## 1 -1.292874e-07 -6.117088e-08  2.321765e-09 2.327068e-07 9.107794e-08
    ## 2 -1.157251e-07 -4.510235e-08  1.920882e-09 2.197712e-07 9.112382e-08
    ## 3 -8.295353e-08 -8.292647e-09 -2.794706e-09 1.808250e-07 8.614971e-08
    ## 4 -4.650941e-08  2.464374e-08 -1.671000e-08 1.192759e-07 6.746259e-08
    ## 5 -1.769647e-08  3.437706e-08 -3.894402e-08 4.739000e-08 3.269118e-08
    ##            V21           V22           V23           V24           V25
    ## 1 1.063794e-08 -9.588853e-08 -1.977818e-07 -1.688021e-07 -1.787985e-07
    ## 2 1.870735e-08 -8.213068e-08 -1.843047e-07 -1.494869e-07 -1.671762e-07
    ## 3 3.444676e-08 -5.093600e-08 -1.516042e-07 -1.002547e-07 -1.368288e-07
    ## 4 4.247559e-08 -2.257812e-08 -1.156897e-07 -4.056765e-08 -9.652118e-08
    ## 5 3.447353e-08 -1.106059e-08 -8.921591e-08  1.151853e-08 -5.665121e-08
    ##             V26           V27           V28          V29          V30
    ## 1 -3.138295e-07 -2.749918e-07 -2.628253e-07 1.201421e-07 1.472597e-07
    ## 2 -2.600779e-07 -2.272129e-07 -2.118529e-07 1.277353e-07 1.492932e-07
    ## 3 -1.350753e-07 -1.169488e-07 -9.300353e-08 1.319364e-07 1.373794e-07
    ## 4 -1.605412e-08 -1.387429e-08  1.619471e-08 1.090753e-07 9.172588e-08
    ## 5  4.764559e-08  3.998088e-08  6.267794e-08 6.798515e-08 2.997765e-08
    ##            V31           V32           V33          V34          V35
    ## 1 2.393388e-07  1.523959e-07  1.657241e-07 1.437912e-07 2.590379e-07
    ## 2 2.367062e-07  1.524424e-07  1.540943e-07 1.476107e-07 2.507412e-07
    ## 3 2.053149e-07  1.273179e-07  1.176614e-07 1.425556e-07 2.259652e-07
    ## 4 1.258762e-07  6.050265e-08  5.750179e-08 1.018503e-07 1.857385e-07
    ## 5 3.478118e-08 -1.330882e-09 -1.274568e-08 3.164529e-08 1.345029e-07
    ##            V36          V37          V38          V39           V40
    ## 1 1.296529e-07 1.988699e-07 9.911824e-08 5.614397e-08 -1.359757e-07
    ## 2 1.257912e-07 2.016644e-07 1.109262e-07 7.307824e-08 -1.142469e-07
    ## 3 1.135800e-07 2.026529e-07 1.344912e-07 1.085909e-07 -6.132265e-08
    ## 4 9.216565e-08 1.885563e-07 1.459894e-07 1.314347e-07 -4.082647e-09
    ## 5 6.332706e-08 1.536432e-07 1.323989e-07 1.226918e-07  3.514676e-08
    ##             V41           V42          V43          V44          V45
    ## 1 -2.792982e-07 -1.029662e-07 5.746294e-08 1.881291e-07 1.170988e-07
    ## 2 -2.453929e-07 -8.468353e-08 6.965879e-08 1.939131e-07 1.170562e-07
    ## 3 -1.659976e-07 -3.464265e-08 1.028459e-07 2.081882e-07 1.154751e-07
    ## 4 -8.321059e-08  3.011882e-08 1.428388e-07 2.213403e-07 1.079774e-07
    ## 5 -2.330029e-08  8.005471e-08 1.658500e-07 2.198338e-07 8.862029e-08
    ##            V46          V47          V48          V49          V50
    ## 1 1.178753e-07 1.148618e-07 1.658796e-07 9.949176e-08 1.716986e-07
    ## 2 1.200379e-07 1.060709e-07 1.637494e-07 9.121882e-08 1.649085e-07
    ## 3 1.312700e-07 8.445412e-08 1.583303e-07 7.021676e-08 1.442950e-07
    ## 4 1.557800e-07 5.885479e-08 1.494429e-07 4.249979e-08 1.094094e-07
    ## 5 1.844953e-07 3.364647e-08 1.342818e-07 1.070706e-08 6.350941e-08
    ##            V51          V52           V53          V54          V55
    ## 1 1.780091e-07 2.182056e-07  1.100079e-07 1.389088e-07 1.609191e-07
    ## 2 1.682750e-07 2.007521e-07  1.057142e-07 1.290435e-07 1.527836e-07
    ## 3 1.413676e-07 1.522728e-07  8.985350e-08 1.013112e-07 1.313291e-07
    ## 4 9.960529e-08 8.351444e-08  5.468176e-08 6.084206e-08 1.010165e-07
    ## 5 4.543824e-08 1.124782e-08 -4.050882e-10 1.663176e-08 6.649765e-08
    ##            V56          V57          V58          V59          V60
    ## 1 1.676068e-07 5.330415e-08 1.788682e-07 1.528374e-07 6.442529e-08
    ## 2 1.555294e-07 4.879135e-08 1.524488e-07 1.474779e-07 5.656353e-08
    ## 3 1.271618e-07 4.425779e-08 9.326441e-08 1.405156e-07 4.508641e-08
    ## 4 9.666912e-08 5.660235e-08 4.387993e-08 1.447700e-07 4.925029e-08
    ## 5 7.324988e-08 9.483941e-08 3.531765e-08 1.582453e-07 7.003882e-08
    ##             V61          V62           V63           V64           V65
    ## 1  7.213406e-08 2.007612e-07  1.551929e-07  1.775421e-07  1.136214e-07
    ## 2  4.705888e-08 1.788176e-07  1.329097e-07  1.527988e-07  9.711088e-08
    ## 3 -1.082044e-08 1.259694e-07  7.824382e-08  8.928365e-08  5.371274e-08
    ## 4 -6.858526e-08 6.896059e-08  1.643500e-08  1.078332e-08 -3.784118e-09
    ## 5 -1.043352e-07 2.837882e-08 -3.174147e-08 -5.975265e-08 -6.217765e-08
    ##             V66           V67           V68           V69          V70
    ## 1  1.788059e-08 -4.308235e-09 -2.468771e-08  2.215029e-08 9.951835e-08
    ## 2 -6.343765e-09 -2.119406e-08 -4.550815e-08  7.751471e-09 9.123647e-08
    ## 3 -6.741059e-08 -6.174465e-08 -9.728809e-08 -2.847029e-08 7.344000e-08
    ## 4 -1.398365e-07 -1.060265e-07 -1.578425e-07 -7.052265e-08 5.835676e-08
    ## 5 -1.989091e-07 -1.389299e-07 -2.070397e-07 -1.003835e-07 5.378294e-08
    ##            V71          V72           V73           V74           V75
    ## 1 1.744291e-07 4.527059e-08 -1.735206e-08 -8.318382e-08 -9.982765e-08
    ## 2 1.670713e-07 4.008206e-08 -4.911147e-08 -1.028648e-07 -1.079474e-07
    ## 3 1.557038e-07 3.658494e-08 -1.186165e-07 -1.450876e-07 -1.265057e-07
    ## 4 1.581544e-07 5.331118e-08 -1.720343e-07 -1.799312e-07 -1.473466e-07
    ## 5 1.776235e-07 9.079412e-08 -1.718894e-07 -1.951306e-07 -1.675679e-07
    ##             V76           V77           V78           V79           V80
    ## 1 -2.359853e-07 -2.521798e-07 -1.844891e-07 -1.323294e-07 -8.639941e-08
    ## 2 -2.535403e-07 -2.642586e-07 -2.025859e-07 -1.578550e-07 -8.344676e-08
    ## 3 -2.927899e-07 -2.886012e-07 -2.419497e-07 -2.152741e-07 -7.361294e-08
    ## 4 -3.267318e-07 -3.039527e-07 -2.730035e-07 -2.713756e-07 -6.243565e-08
    ## 5 -3.370682e-07 -3.005215e-07 -2.766076e-07 -3.115318e-07 -6.270506e-08
    ##             V81           V82           V83           V84           V85
    ## 1 -3.730862e-07 -3.223153e-07 -4.101803e-07 -3.742750e-07 -2.172568e-07
    ## 2 -3.755028e-07 -3.299197e-07 -4.237268e-07 -3.840285e-07 -2.241256e-07
    ## 3 -3.713388e-07 -3.413074e-07 -4.534326e-07 -4.014603e-07 -2.377774e-07
    ## 4 -3.468053e-07 -3.382009e-07 -4.780182e-07 -4.065650e-07 -2.468374e-07
    ## 5 -3.117838e-07 -3.193003e-07 -4.822976e-07 -3.926685e-07 -2.492666e-07
    ##             V86           V87           V88           V89           V90
    ## 1 -1.726465e-07 -3.943041e-08 -1.838273e-07 -3.229262e-07 -3.823165e-07
    ## 2 -1.848909e-07 -5.531529e-08 -1.933118e-07 -3.294091e-07 -3.798538e-07
    ## 3 -2.138629e-07 -9.479853e-08 -2.149950e-07 -3.408529e-07 -3.635979e-07
    ## 4 -2.453476e-07 -1.416435e-07 -2.363774e-07 -3.436759e-07 -3.231918e-07
    ## 5 -2.705871e-07 -1.840438e-07 -2.527642e-07 -3.352271e-07 -2.693479e-07
    ##             V91           V92           V93           V94           V95
    ## 1 -3.543568e-07 -3.199459e-07 -3.199647e-07 -4.517874e-07 -4.457471e-07
    ## 2 -3.576412e-07 -3.387735e-07 -3.496029e-07 -4.572644e-07 -4.389218e-07
    ## 3 -3.585347e-07 -3.756765e-07 -4.056247e-07 -4.535991e-07 -4.079498e-07
    ## 4 -3.410969e-07 -3.966750e-07 -4.255588e-07 -4.046000e-07 -3.421871e-07
    ## 5 -3.023091e-07 -3.911335e-07 -3.814691e-07 -3.034385e-07 -2.602815e-07
    ##             V96           V97           V98           V99          V100
    ## 1 -3.531671e-07  6.881679e-08 -1.290874e-07 -2.751826e-07 -3.528838e-07
    ## 2 -3.505444e-07  5.489853e-08 -1.308543e-07 -2.774126e-07 -3.552171e-07
    ## 3 -3.371300e-07  1.777176e-08 -1.351071e-07 -2.809956e-07 -3.600908e-07
    ## 4 -3.068568e-07 -3.289015e-08 -1.396121e-07 -2.829700e-07 -3.644085e-07
    ## 5 -2.694447e-07 -8.709824e-08 -1.419509e-07 -2.886468e-07 -3.698609e-07
    ##            V101          V102          V103          V104          V105
    ## 1 -2.392003e-07 -2.710481e-07 -3.154618e-07  2.714853e-08 -3.535588e-08
    ## 2 -2.445839e-07 -2.837426e-07 -3.309679e-07  1.490996e-08 -4.825944e-08
    ## 3 -2.583280e-07 -3.059194e-07 -3.584397e-07 -1.268559e-08 -7.731441e-08
    ## 4 -2.754962e-07 -3.078518e-07 -3.603829e-07 -3.639706e-08 -1.047181e-07
    ## 5 -2.943958e-07 -2.771505e-07 -3.208559e-07 -4.501294e-08 -1.235709e-07
    ##            V106          V107          V108          V109          V110
    ## 1 -1.958500e-08 -4.593824e-08 -7.577818e-08  1.082676e-08 -5.865000e-09
    ## 2 -2.733676e-08 -4.737118e-08 -8.186618e-08  2.982912e-09 -1.336909e-08
    ## 3 -4.666838e-08 -4.955676e-08 -9.832771e-08 -1.564088e-08 -3.085500e-08
    ## 4 -7.085971e-08 -5.210088e-08 -1.207919e-07 -3.678250e-08 -4.856324e-08
    ## 5 -1.007376e-07 -6.343000e-08 -1.467165e-07 -6.023588e-08 -6.126700e-08
    ##            V111          V112         V113          V114          V115
    ## 1  5.032765e-08  6.835824e-08 7.773385e-08 -8.756912e-08 -1.258226e-07
    ## 2  4.047871e-08  6.016853e-08 7.264153e-08 -9.101988e-08 -1.305359e-07
    ## 3  1.327118e-08  3.866209e-08 5.827500e-08 -9.725471e-08 -1.394465e-07
    ## 4 -2.487776e-08  1.130853e-08 3.652618e-08 -9.899029e-08 -1.427921e-07
    ## 5 -6.557794e-08 -1.443765e-08 8.730941e-09 -9.463794e-08 -1.373683e-07
    ##            V116          V117          V118          V119          V120
    ## 1 -4.548735e-08  1.396111e-08  4.014971e-08  3.589441e-08 -2.928703e-08
    ## 2 -4.947053e-08  3.498471e-09  3.079235e-08  2.136644e-08 -3.279694e-08
    ## 3 -5.793235e-08 -2.239794e-08  8.556765e-09 -1.267088e-08 -3.969491e-08
    ## 4 -6.368738e-08 -5.095853e-08 -1.164824e-08 -4.615912e-08 -3.977529e-08
    ## 5 -6.352647e-08 -7.089124e-08 -2.204294e-08 -6.764971e-08 -2.314662e-08
    ##            V121         V122          V123         V124         V125
    ## 1 -1.041588e-08 2.357344e-08 -2.235029e-08 5.756824e-08 1.356691e-07
    ## 2 -9.829706e-09 2.347294e-08 -2.435618e-08 5.425618e-08 1.402782e-07
    ## 3 -7.663479e-09 2.342562e-08 -2.949176e-08 4.517941e-08 1.494662e-07
    ## 4 -3.103294e-09 2.371853e-08 -3.691118e-08 3.185618e-08 1.525715e-07
    ## 5  2.522941e-09 2.181726e-08 -4.990206e-08 1.444441e-08 1.398761e-07
    ##            V126          V127          V128
    ## 1 -3.913529e-09 -1.068085e-07 -1.568559e-07
    ## 2  1.644412e-09 -9.583276e-08 -1.472541e-07
    ## 3  1.544706e-08 -6.641912e-08 -1.220035e-07
    ## 4  3.161588e-08 -2.711853e-08 -8.869206e-08
    ## 5  4.097638e-08  8.331765e-09 -5.902019e-08

**Next, we want to define the Regions of Interest (ROIs).** In each
dataframe, columns titled *V1-V120* refer to the electrode site on each
EEG cap. Each column in the raw datafiles represent an electrode. ROIs
can then be grouped however you wish to group them. You just need to
change up the names in the variables below to do that.

    ROI1 <- c("V94","V95","V101","V102","V103","V104","V105");
    ROI2 <- c("V59","V60","V69","V70","V71","V72","V73")
    ROI3 <- c("V88","V89","V98","V99","V100","V108","V109")
    ROI4 <- c("V63","V64","V66","V67","V68","V75","V76")
    ROI5 <- c("V6","V7","V8","V113","V122","V123","V124","V125")
    ROI6 <- c("V5","V6","V7","V17","V18")
    ROI7 <- c("V30","V31","V32","V35","V36")
    ROI8 <- c("V79","V80","V81","V82","V83","V92","V93")
    ROI9 <- c("V3","V4","V5","V19", "V20", "V32", "V34", "V112")

**Set up time points for later plotting.**

    time<-seq(-200, 2000, length = 564)
    time<-round(time)

**Find the row means for each ROI, convert the variable into a
dataframe.**

    Mean_CD_ROI1 <- rowMeans(Mean_CD[ROI1]); Mean_CD_ROI1 <- as.data.frame(Mean_CD_ROI1)
    Mean_CD_ROI2 <- rowMeans(Mean_CD[ROI2]); Mean_CD_ROI2 <- as.data.frame(Mean_CD_ROI2)
    Mean_CD_ROI3 <- rowMeans(Mean_CD[ROI3]); Mean_CD_ROI3 <- as.data.frame(Mean_CD_ROI3)
    Mean_CD_ROI4 <- rowMeans(Mean_CD[ROI4]); Mean_CD_ROI4 <- as.data.frame(Mean_CD_ROI4)
    Mean_CD_ROI5 <- rowMeans(Mean_CD[ROI5]); Mean_CD_ROI5 <- as.data.frame(Mean_CD_ROI5)
    Mean_CD_ROI6 <- rowMeans(Mean_CD[ROI6]); Mean_CD_ROI6 <- as.data.frame(Mean_CD_ROI6)
    Mean_CD_ROI7 <- rowMeans(Mean_CD[ROI7]); Mean_CD_ROI7 <- as.data.frame(Mean_CD_ROI7)
    Mean_CD_ROI8 <- rowMeans(Mean_CD[ROI8]); Mean_CD_ROI8 <- as.data.frame(Mean_CD_ROI8)
    Mean_CD_ROI9 <- rowMeans(Mean_CD[ROI9]); Mean_CD_ROI9 <- as.data.frame(Mean_CD_ROI9)

**Again, we need to check it it was completed properly.**

    head(Mean_CD_ROI2,5)

    ##   Mean_CD_ROI2
    ## 1 7.732556e-08
    ## 2 6.586733e-08
    ## 3 4.346343e-08
    ## 4 3.161224e-08
    ## 5 3.974454e-08

**Now we do this for Familiar responses and Don't Know responses...**

    Mean_FAM_ROI1 <- rowMeans(Mean_FAM[ROI1]); Mean_FAM_ROI1 <- as.data.frame(Mean_FAM_ROI1)
    Mean_FAM_ROI2 <- rowMeans(Mean_FAM[ROI2]); Mean_FAM_ROI2 <- as.data.frame(Mean_FAM_ROI2)
    Mean_FAM_ROI3 <- rowMeans(Mean_FAM[ROI3]); Mean_FAM_ROI3 <- as.data.frame(Mean_FAM_ROI3)
    Mean_FAM_ROI4 <- rowMeans(Mean_FAM[ROI4]); Mean_FAM_ROI4 <- as.data.frame(Mean_FAM_ROI4)
    Mean_FAM_ROI5 <- rowMeans(Mean_FAM[ROI5]); Mean_FAM_ROI5 <- as.data.frame(Mean_FAM_ROI5)
    Mean_FAM_ROI6 <- rowMeans(Mean_FAM[ROI6]); Mean_FAM_ROI6 <- as.data.frame(Mean_FAM_ROI6)
    Mean_FAM_ROI7 <- rowMeans(Mean_FAM[ROI7]); Mean_FAM_ROI7 <- as.data.frame(Mean_FAM_ROI7)
    Mean_FAM_ROI8 <- rowMeans(Mean_FAM[ROI8]); Mean_FAM_ROI8 <- as.data.frame(Mean_FAM_ROI8)
    Mean_FAM_ROI9 <- rowMeans(Mean_FAM[ROI9]); Mean_FAM_ROI9 <- as.data.frame(Mean_FAM_ROI9)

    Mean_DK_ROI1 <- rowMeans(Mean_DK[ROI1]); Mean_DK_ROI1 <- as.data.frame(Mean_DK_ROI1)
    Mean_DK_ROI2 <- rowMeans(Mean_DK[ROI2]); Mean_DK_ROI2 <- as.data.frame(Mean_DK_ROI2)
    Mean_DK_ROI3 <- rowMeans(Mean_DK[ROI3]); Mean_DK_ROI3 <- as.data.frame(Mean_DK_ROI3)
    Mean_DK_ROI4 <- rowMeans(Mean_DK[ROI4]); Mean_DK_ROI4 <- as.data.frame(Mean_DK_ROI4)
    Mean_DK_ROI5 <- rowMeans(Mean_DK[ROI5]); Mean_DK_ROI5 <- as.data.frame(Mean_DK_ROI5)
    Mean_DK_ROI6 <- rowMeans(Mean_DK[ROI6]); Mean_DK_ROI6 <- as.data.frame(Mean_DK_ROI6)
    Mean_DK_ROI7 <- rowMeans(Mean_DK[ROI7]); Mean_DK_ROI7 <- as.data.frame(Mean_DK_ROI7)
    Mean_DK_ROI8 <- rowMeans(Mean_DK[ROI8]); Mean_DK_ROI8 <- as.data.frame(Mean_DK_ROI8)
    Mean_DK_ROI9 <- rowMeans(Mean_DK[ROI9]); Mean_DK_ROI9 <- as.data.frame(Mean_DK_ROI9)

**OK, like in the grand average script, we need to bind everything into
a single data frame.**

    Omnibus_DF <- cbind(Mean_CD_ROI1, Mean_CD_ROI2, Mean_CD_ROI3, Mean_CD_ROI4, Mean_CD_ROI5, Mean_CD_ROI6, Mean_CD_ROI7, Mean_CD_ROI8, Mean_CD_ROI9, Mean_FAM_ROI1, Mean_FAM_ROI2, Mean_FAM_ROI3, Mean_FAM_ROI4, Mean_FAM_ROI5, Mean_FAM_ROI6, Mean_FAM_ROI7, Mean_FAM_ROI8, Mean_FAM_ROI9, Mean_DK_ROI1, Mean_DK_ROI2, Mean_DK_ROI3, Mean_DK_ROI4,Mean_DK_ROI5, Mean_DK_ROI6, Mean_DK_ROI7, Mean_DK_ROI8, Mean_DK_ROI9)

**Double check the `Omnibus_DF` data frame.**

    head(Omnibus_DF, 5)

    ##    Mean_CD_ROI1 Mean_CD_ROI2  Mean_CD_ROI3  Mean_CD_ROI4  Mean_CD_ROI5
    ## 1 -2.473503e-07 7.732556e-08 -1.898370e-07 -2.027613e-09  3.207547e-08
    ## 2 -2.555471e-07 6.586733e-08 -1.950126e-07 -2.126073e-08  3.045816e-08
    ## 3 -2.677480e-07 4.346343e-08 -2.065729e-07 -6.831593e-08  2.515537e-08
    ## 4 -2.616619e-07 3.161224e-08 -2.178026e-07 -1.215094e-07  1.531258e-08
    ## 5 -2.321008e-07 3.974454e-08 -2.279146e-07 -1.630013e-07 -2.192353e-10
    ##    Mean_CD_ROI6 Mean_CD_ROI7  Mean_CD_ROI8 Mean_CD_ROI9 Mean_FAM_ROI1
    ## 1 -8.207824e-09 1.855371e-07 -2.806030e-07 1.430192e-07 -1.467237e-07
    ## 2 -7.982176e-09 1.829948e-07 -2.941182e-07 1.377122e-07 -1.407784e-07
    ## 3 -1.010200e-08 1.619115e-07 -3.194667e-07 1.160588e-07 -1.198070e-07
    ## 4 -1.911201e-08 1.112018e-07 -3.312956e-07 7.330826e-08 -8.320795e-08
    ## 5 -3.598873e-08 5.225159e-08 -3.228888e-07 2.093633e-08 -4.525187e-08
    ##   Mean_FAM_ROI2 Mean_FAM_ROI3 Mean_FAM_ROI4 Mean_FAM_ROI5 Mean_FAM_ROI6
    ## 1  2.462555e-08 -2.668618e-07 -2.154800e-07  1.993676e-07  2.994560e-07
    ## 2  2.657379e-08 -2.525860e-07 -2.027288e-07  2.020875e-07  2.962976e-07
    ## 3  2.029313e-08 -2.132776e-07 -1.692717e-07  2.071095e-07  2.848086e-07
    ## 4 -7.467004e-09 -1.602599e-07 -1.229708e-07  2.069133e-07  2.628870e-07
    ## 5 -4.398894e-08 -1.120584e-07 -7.038300e-08  1.930734e-07  2.311770e-07
    ##   Mean_FAM_ROI7 Mean_FAM_ROI8 Mean_FAM_ROI9  Mean_DK_ROI1  Mean_DK_ROI2
    ## 1  2.826733e-07 -2.172209e-07  2.680026e-07 -1.015511e-07 -3.615434e-07
    ## 2  2.405289e-07 -2.126560e-07  2.491516e-07 -1.241709e-07 -3.687762e-07
    ## 3  1.553579e-07 -2.030238e-07  2.069349e-07 -1.835152e-07 -3.826817e-07
    ## 4  8.765766e-08 -1.919313e-07  1.655738e-07 -2.565481e-07 -3.858184e-07
    ## 5  3.573365e-08 -1.746163e-07  1.306804e-07 -3.223655e-07 -3.578924e-07
    ##    Mean_DK_ROI3  Mean_DK_ROI4 Mean_DK_ROI5 Mean_DK_ROI6 Mean_DK_ROI7
    ## 1 -2.404533e-07 -5.431852e-07 3.988278e-07 3.698638e-07 4.341831e-07
    ## 2 -2.554544e-07 -5.315505e-07 3.964107e-07 3.770777e-07 3.896944e-07
    ## 3 -2.941424e-07 -4.980671e-07 3.833581e-07 3.913436e-07 3.014597e-07
    ## 4 -3.413676e-07 -4.472163e-07 3.528656e-07 4.005137e-07 2.661083e-07
    ## 5 -3.795264e-07 -3.859750e-07 3.106789e-07 4.011346e-07 3.191079e-07
    ##    Mean_DK_ROI8 Mean_DK_ROI9
    ## 1 -3.852193e-07 1.219425e-07
    ## 2 -4.041201e-07 1.179036e-07
    ## 3 -4.501162e-07 1.154881e-07
    ## 4 -4.981506e-07 1.337929e-07
    ## 5 -5.220209e-07 1.745513e-07

**Finally, we get to plotting.** We use the basic base-R plotting
functions here, but you could use the `ggplot` package if you wanted
nicer plots. Each plot has a lot of arguments, so check out ?plot,
?lines, and ?legend to understand them. I'll go through the plots more
generally step by step.

`plot()` calls the plotting function. Note we first build the plot using
the ROI 1 data first. The `lines()` function will overlay additional
lines on the original plot, in this case, we call ROI 1 data for
Familiar, and Don't Know, then overlay them over the Can Defines.

`line_names()` specifies the line names for a legend. `legend()` will
place a legend on the figure, consisting of our line names (which is
used in the argument.)

First, we convert the values into more readable data.

    Omnibus_DF <- Omnibus_DF*10**6
    Omnibus_DF <- cbind(Omnibus_DF, time)

**Now we start creating plots.** We start with ROI 1 thru ROI 9.

**Note that in the script itself, the plots will pop up, and you can right click to save them!**
------------------------------------------------------------------------------------------------

    plot(Omnibus_DF[c("time", "Mean_CD_ROI1")], type = 'l', xlab = "Time In Seconds", ylab = "uV (microvolts)", xlim=c(-200,2000), ylim=c(-3.5, 3.5), main = "ROI 1", col = "blue", cex.lab = .75, cex.axis = .65)
    lines(Omnibus_DF[c("time", "Mean_FAM_ROI1")], col = "green", lty = 2)
    lines(Omnibus_DF[c("time", "Mean_DK_ROI1")], col = "red", lty = 3)
    line_names <-c("Can Define", "Familiar", "Don't Know")
    legend('topright', line_names, col = c("blue", "green", "red"), lty = c(1,2,3), cex = .75)

![](ROI1.png)

**ROI 2**

    plot(Omnibus_DF[c("time", "Mean_CD_ROI2")], type = 'l', xlab = "Time In Seconds", ylab = "uV (microvolts)", ylim=c(-3.5, 3.5), main = "ROI 2", col = "blue", cex.lab = .75, cex.axis = .75, xaxt = 'n')
    axis(1, seq(-200,2000, by = 200), cex.axis = .75)
    lines(Omnibus_DF[c("time", "Mean_FAM_ROI2")], col = "green", lty = 2)
    lines(Omnibus_DF[c("time", "Mean_DK_ROI2")], col = "red", lty = 3)
    line_names <-c("Can Define", "Familiar", "Don't Know")
    legend('topright', line_names, col = c("blue", "green", "red"), lty = c(1,2,3), cex = .75)

![](ROI2.png)

**ROI 3**

    plot(Omnibus_DF[c("time", "Mean_CD_ROI3")], type = 'l', xlab = "Time In Seconds", ylab = "uV (microvolts)", xlim=c(-200,2000), ylim=c(-3.5, 3.5), main = "ROI 3", col = "blue", cex.lab = .75, cex.axis = .75, xaxt = 'n')
    axis(1, seq(-200,2000, by = 200), cex.axis = .75)
    lines(Omnibus_DF[c("time", "Mean_FAM_ROI3")], col = "green", lty = 2)
    lines(Omnibus_DF[c("time", "Mean_DK_ROI3")], col = "red", lty = 3)
    line_names <-c("Can Define", "Familiar", "Don't Know")
    legend('topright', line_names, col = c("blue", "green", "red"), lty = c(1,2,3), cex = .75)

![](ROI3.png)

**ROI 4**

    plot(Omnibus_DF[c("time", "Mean_CD_ROI4")], type = 'l', xlab = "Time In Seconds", ylab = "uV (microvolts)", xlim=c(-200,2000), ylim=c(-3.5, 3.5), main = "ROI 4", col = "blue", cex.lab = .75, cex.axis = .75, xaxt = 'n')
    axis(1, seq(-200,2000, by = 200), cex.axis = .75)
    lines(Omnibus_DF[c("time", "Mean_FAM_ROI4")], col = "green", lty = 2)
    lines(Omnibus_DF[c("time", "Mean_DK_ROI4")], col = "red", lty = 3)
    line_names <-c("Can Define", "Familiar", "Don't Know")
    legend('topright', line_names, col = c("blue", "green", "red"), lty = c(1,2,3), cex = .75)

![](ROI4.png)

**ROI 5**

    plot(Omnibus_DF[c("time", "Mean_CD_ROI5")], type = 'l', xlab = "Time In Seconds", ylab = "uV (microvolts)", xlim=c(-200,2000), ylim=c(-3.5, 3.5), main = "ROI 5", col = "blue", cex.lab = .75, cex.axis = .75, xaxt = 'n')
    axis(1, seq(-200,2000, by = 200), cex.axis = .75)
    lines(Omnibus_DF[c("time", "Mean_FAM_ROI5")], col = "green", lty = 2)
    lines(Omnibus_DF[c("time", "Mean_DK_ROI5")], col = "red", lty = 3)
    line_names <-c("Can Define", "Familiar", "Don't Know")
    legend('topright', line_names, col = c("blue", "green", "red"), lty = c(1,2,3), cex = .75)

![](ROI5.png)

**ROI 6**

    plot(Omnibus_DF[c("time", "Mean_CD_ROI6")], type = 'l', xlab = "Time In Seconds", ylab = "uV (microvolts)", xlim=c(-200,2000), ylim=c(-3.5, 3.5), main = "ROI 6", col = "blue", cex.lab = .75, cex.axis = .75, xaxt = 'n')
    axis(1, seq(-200,2000, by = 200), cex.axis = .75)
    lines(Omnibus_DF[c("time", "Mean_FAM_ROI6")], col = "green", lty = 2)
    lines(Omnibus_DF[c("time", "Mean_DK_ROI6")], col = "red", lty = 3)
    line_names <-c("Can Define", "Familiar", "Don't Know")
    legend('topright', line_names, col = c("blue", "green", "red"), lty = c(1,2,3), cex = .75)

![](ROI6.png)

**ROI 7**

    plot(Omnibus_DF[c("time", "Mean_CD_ROI7")], type = 'l', xlab = "Time In Seconds", ylab = "uV (microvolts)", xlim=c(-200,2000), ylim=c(-3.5, 3.5), main = "ROI 7", col = "blue", cex.lab = .75, cex.axis = .75, xaxt = 'n')
    axis(1, seq(-200,2000, by = 200), cex.axis = .75)
    lines(Omnibus_DF[c("time", "Mean_FAM_ROI7")], col = "green", lty = 2)
    lines(Omnibus_DF[c("time", "Mean_DK_ROI7")], col = "red", lty = 3)
    line_names <-c("Can Define", "Familiar", "Don't Know")
    legend('topright', line_names, col = c("blue", "green", "red"), lty = c(1,2,3), cex = .75)

![](ROI7.png)

**ROI 8**

    plot(Omnibus_DF[c("time", "Mean_CD_ROI8")], type = 'l', xlab = "Time In Seconds", ylab = "uV (microvolts)", xlim=c(-200,2000), ylim=c(-3.5, 3.5), main = "ROI 8", col = "blue", cex.lab = .75, cex.axis = .75, xaxt = 'n')
    axis(1, seq(-200,2000, by = 200), cex.axis = .75)
    lines(Omnibus_DF[c("time", "Mean_FAM_ROI8")], col = "green", lty = 2)
    lines(Omnibus_DF[c("time", "Mean_DK_ROI8")], col = "red", lty = 3)
    line_names <-c("Can Define", "Familiar", "Don't Know")
    legend('topright', line_names, col = c("blue", "green", "red"), lty = c(1,2,3), cex = .75)

![](ROI8.png)

**ROI 9**

    plot(Omnibus_DF[c("time", "Mean_CD_ROI9")], type = 'l', xlab = "Time In Seconds", ylab = "uV (microvolts)", xlim=c(-200,2000), ylim=c(-3.5, 3.5), main = "ROI 9", col = "blue", cex.lab = .75, cex.axis = .75, xaxt = 'n')
    axis(1, seq(-200,2000, by = 200), cex.axis = .75)
    lines(Omnibus_DF[c("time", "Mean_FAM_ROI9")], col = "green", lty = 2)
    lines(Omnibus_DF[c("time", "Mean_DK_ROI9")], col = "red", lty = 3)
    line_names <-c("Can Define", "Familiar", "Don't Know")
    legend('topright', line_names, col = c("blue", "green", "red"), lty = c(1,2,3), cex = .75)

![](ROI9.png)
