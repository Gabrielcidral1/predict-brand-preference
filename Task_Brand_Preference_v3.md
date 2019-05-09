<img style="float: right;" src="https://media.timtul.com/media/network22/ubiqum.png">

Example of solution

Load packages and data
----------------------

    pacman:: p_load(dummies, caret, party, xlsx, corrplot, ggplot2, plotly, gplots, reshape2, dplyr, readr)

    complete <- read.csv("Complete_Responses_v1_inputR.csv", sep=";")

    incomplete <- read.csv("SurveyIncomplete v1.csv", 
                                    sep=";")

    caret:: modelLookup("svmLinear")

    ##       model parameter label forReg forClass probModel
    ## 1 svmLinear         C  Cost   TRUE     TRUE      TRUE

We see the samebrand preference behaviour in all classes of education
level, region and car

<!-- ```{r check distributions} -->
<!-- # check sample method -->
<!-- plot(complete$elevel, complete$brand) -->
<!-- plot(complete$region, complete$brand) -->
<!-- plot(complete$car, complete$brand) -->
<!-- # distribution of depedent variable -->
<!-- # change to pie chart -->
<!-- f <- ggplot(as.data.frame(table(complete$brand)), aes(x = "",y = Freq, fill = Var1)) +  -->
<!--   geom_bar(width = 1, stat = "identity") -->
<!-- f + coord_polar("y", start=0) -->
<!-- # check distribution complete versus incomplete  -->
<!-- complete$survey <- "complete" -->
<!-- incomplete$survey <- "incomplete" -->
<!-- all <- rbind(complete, incomplete) -->
<!-- for(i in names(all[ ,!names(all) %in% c("survey","brand")])){ -->
<!-- x <- ggplot(data = all,aes_string(i,color="survey")) + geom_density() -->
<!-- print(x) -->
<!-- } -->
<!-- ``` -->
<!-- all lines are unique, though just becasue of salary and credit -->
<!-- ```{r part 2} -->
<!-- x <- semi_join(incomplete,complete, by = c("elevel", "car", "region", "age","yearly.salary", "credit")) -->
<!-- x -->
<!-- ``` -->
<!-- ```{r part 3} -->
<!-- complete$survey <- NULL -->
<!-- incomplete$survey <- NULL -->
<!-- # Chi analysis  -->
<!-- chi_eleval_brand <- chisq.test(complete$elevel,complete$brand) -->
<!-- chi_eleval_brand -->
<!-- chi_car_brand <- chisq.test(complete$car,complete$brand) -->
<!-- chi_car_brand -->
<!-- chi_region_brand <- chisq.test(complete$region,complete$brand) -->
<!-- chi_region_brand -->
<!-- # decision tree -->
<!-- decisiontree <- ctree(brand~.,data = complete, controls = ctree_control(maxdepth = 3)) -->
<!-- plot(decisiontree) -->
<!-- ``` -->
<!-- ## Model -->
<!-- ```{r model, echo = FALSE } -->
<!-- set.seed(123) -->
<!-- inTraining <- createDataPartition(complete$brand,p=.75,list = FALSE) -->
<!-- training <- complete[inTraining,] -->
<!-- testing <- complete[-inTraining,] -->
<!-- fitControl <- trainControl(method = "cv", number=2, verboseIter = TRUE) -->
<!-- # loop for models -->
<!-- combined <- c() -->
<!-- a <- c("knn", "rf", "svmRadial", "svmLinear", "gbm") -->
<!-- for(i in a) { -->
<!-- Fit <- train(brand~yearly.salary+age,data = training,method = i,  -->
<!--                 na.action = na.omit, trControl = fitControl, tuneLength = 5,  -->
<!--                 preProcess = c("center", "scale")) -->
<!-- pred <- predict(Fit,testing) -->
<!-- res <- postResample(pred,testing$brand) -->
<!-- combined <- cbind(combined, res)  -->
<!-- } -->
<!-- colnames(combined) <- a -->
<!-- combined -->
<!-- ``` -->
<!-- ```{r part 4} -->
<!-- ## gbm -->
<!-- gbmFit <- train(brand~yearly.salary+age, data = training, method = "gbm",  -->
<!--                 na.action = na.omit, trControl = fitControl, tuneLength = 20,  -->
<!--                 preProcess = c("center", "scale")) -->
<!-- predictbrand_gbm <- predict(gbmFit,testing) -->
<!-- postResample(predictbrand_gbm,testing$brand) -->
<!-- confusionMatrix(testing$brand,predictbrand_gbm) -->
<!-- ## Applying prediction -->
<!-- incomplete$brand <- predict(gbmFit,incomplete) -->
<!-- # Visual results -->
<!-- f <- ggplot(incomplete, aes(age, yearly.salary)) -->
<!-- f + geom_point(aes(colour = factor(brand))) -->
<!-- ``` -->
<!-- ```{r viz} -->
<!-- all <- rbind(complete, incomplete) -->
<!-- by_group <- all %>% group_by(elevel, car) %>% dplyr::summarise(brand = sum(brand == "Acer")) -->
<!-- by_group1 <- by_group  -->
<!-- by_group <- reshape2::dcast(data = by_group, formula = car~elevel) -->
<!-- # need data as matrix -->
<!-- mm <- as.matrix(by_group) -->
<!-- mm <- mm[,2:6] -->
<!-- mm <- mapply(mm, FUN=as.numeric) -->
<!-- mm <- matrix(data=mm, ncol=5, nrow=20) -->
<!-- rownames(mm) <- by_group$car -->
<!-- colnames(mm) <- names(by_group[2:6]) -->
<!-- heatmap.2(x = mm, Rowv = FALSE, Colv = FALSE, dendrogram = "none", -->
<!--           cellnote = mm, notecol = "black", notecex = 2, -->
<!--           trace = "none", key = FALSE, margins = c(7, 11)) -->
<!-- ### Key States -->
<!-- key_states <- read_delim("key_states.csv", ";", escape_double = FALSE, col_types = cols(X3 = col_skip(), X4 = col_skip()), trim_ws = TRUE) -->
<!-- all$region <- as.character(all$region) -->
<!-- all_regions <- left_join(all, key_states, by = "region") -->
<!-- ## Export prediction -->
<!-- write.csv(all_regions, file = "all_regions.csv") -->
<!-- #aplot <- as.data.frame(table(incomplete$prediction,incomplete$car, incomplete$elevel)) -->
<!-- #g <- ggplot(aplot, aes(x = "",y = Freq, fill = Var1)) +  -->
<!--   #geom_bar(width = 1, stat = "identity") + coord_polar("y", start=0) + facet_grid(.~Var2) -->
<!-- #g  -->
<!-- ``` -->
