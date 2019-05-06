# Predict_drop_out

Prediction of student behavior has been a prominant area of research in learning analytics and a major concern for higher education institutions and ed tech companies alike. It is the bedrock of [methodology within the world of cognitive tutors](https://solaresearch.org/hla-17/hla17-chapter5/) and these methods have been exported to other areas within the education technology landscape. The ability to predict what a student is likely to do in the future so that interventions can be tailored to them has seen major growth and investment, [though implementation is non-trivial and expensive](https://www.newamerica.org/education-policy/policy-papers/promise-and-peril-predictive-analytics-higher-education/). Although some institutions, such as Purdue University, have seen success we are yet to see widespread adoption of these approaches as they tend to be highly institution specific and require very concrete outcomes to be useful.


In this project I will model student data using three flavors of tree algorithm. We will be using these algorithms to attempt to predict which students drop out of courses. Many universities have a problem with students over-enrolling in courses at the beginning of semester and then dropping most of them as the make decisions about which classes to attend. This makes it difficult to plan for the semester and allocate resources. However, schools don't want to restrict the choice of their students. One solution is to create predictions of which students are likley to drop out of which courses and use these predictions to inform semester planning. 

We will be using the tree algorithms to build models of which students are likely to drop out of which classes. 

### Software

In order to generate our models we will need several packages. The first package we should install is [caret](https://cran.r-project.org/web/packages/caret/index.html).There are many prediction packages available and they all have slightly different syntax. caret is a package that brings all the different algorithms under one hood using the same syntax. 

The other packages we install is party and [C50](https://cran.r-project.org/web/packages/C50/index.html).

### Data

The data comes from a university registrar's office. The code book for the variables are available in the file code-book.txt. Examine the variables and their definitions. Upload the drop-out.csv data into R as a data frame. 

The next step is to separate the data set into a training set and a test set. Randomly select 25% of the students to be the test data set and leave the remaining 75% for your training data set. 

```{r}
length(unique(dropout$student_id))*0.75 # the result shows that 75% of students if 511

inTrain<-data.frame(sample(unique(dropout$student_id), size=511))
names(inTrain)<-c("student_id")

training<-inner_join(dropout, inTrain, by="student_id")
testing<-anti_join(dropout, inTrain, by="student_id")

```

Here I will be predicting the student level variable "complete". Visualize the relationships between the chosen variables as a scatterplot matrix, and explore relationships with other visualizations.

![scatter](https://github.com/ab4499/Predict_drop_out/blob/master/graphs/scatter.png "github")

![eda1](https://github.com/ab4499/Predict_drop_out/blob/master/graphs/EDA1.png "github")

![eda2](https://github.com/ab4499/Predict_drop_out/blob/master/graphs/EDA2.png "github")


### CART Trees

Construct a classification tree that predicts complete using the caret package.

Check the results

![cart](https://github.com/ab4499/Predict_drop_out/blob/master/graphs/cart_result.png)

Plot ROC against complexity 

![roc](https://github.com/ab4499/Predict_drop_out/blob/master/graphs/ROC.png "github")

Now predict results from the test data. Generate model statistics

![predict1](https://github.com/ab4499/Predict_drop_out/blob/master/graphs/predict1.png "github")


## Conditional Inference Tree
Train a Conditional Inference Tree using the `party` package on the same training data and examine the results.

```{r}
# define the prediction model
partyFit<-ctree(complete~., data=training2)
# plot the tree
plot(partyFit)
```
![party](https://github.com/ab4499/Predict_drop_out/blob/master/graphs/Partyfit.png)

Now test the new Conditional Inference model by predicting the test data and generating model fit statistics.

![predict2](https://github.com/ab4499/Predict_drop_out/blob/master/graphs/predict2.png "github")

## C50

Install the C50 package, train and then test the C5.0 model on the same data.

```{r}
# Train the model using "C5.0"
c50Fit<-C5.0(training2[,-4], training2[,4])

# predict with c50Fit model
c50Classes<-predict(c50Fit,newdata=testing2)

confusionMatrix(c50Classes, testing2$complete)
```

![predict3](https://github.com/ab4499/Predict_drop_out/blob/master/graphs/predict3.png "github")

