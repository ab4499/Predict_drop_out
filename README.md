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









