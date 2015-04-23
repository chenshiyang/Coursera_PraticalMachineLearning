Introduction
     Using devices such as Jawbone Up, Nike FuelBand, and Fitbit it is now possible to collect a large amount of data about personal activity relatively inexpensively. These type of devices are part of the quantified self movement ¨C a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. One thing that people regularly do is quantify how much of a particular activity they do, but they rarely quantify how well they do it.
      In this project, your goal will be to use data from accelerometers on the belt, forearm, arm, and dumbell of 6 participants.

Data
The training data for this project are available here: 

https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv

The test data are available here: 

https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv

The data for this project come from this source: http://groupware.les.inf.puc-rio.br/har. If you use the document you create for this class for any purpose please cite them as they have been very generous in allowing their data to be used for this kind of assignment. 

View the data
      Load the training data and testing data into R console, naming training and testing.
Use str() function, we can  see that there are 19622 observations in the training set, each has 160 features. And there are 20 observations in the testing set.
     Using names() function, we can take a look at the features, and we can know that the class label is the last feature ,"classe", which has 5 levels, namely "A" through "E".
     Since there are too many features, we may need to remove some useless features. That is to say, we need to do some data cleaning before use it.
     Further, since we need to estimate the out sample error, we should split the training set into a train set and a validation set. Then, we can use the train set to build our model, and use the validation set to evaluate the model.

Data cleaning
     In this step, we will remove some useless observations and useless features.
First, remove columns that have NA values.

> training <- training[, colSums(is.na(training)) == 0]
> testing <- testing[, colSums(is.na(testing)) == 0]

     After that, we can retain 93 features.
     Then, we remove features that do no contribution to the accelerometer measurements.Such as feature "X", "raw_timestamp_part_1",
"raw_timestamp_part_2","new_window"  and so on.
     At last, we get 19622 observations and 53 features, and there are no NA value.


Data Splitting
     We should split the training set into a train set and a validation set. The split ratio is set to be 0.7. That is to say,  we use 70% of the training set as a train set, and 30% of the training set as a validation set.
     The seed is set as 144.

> set.seed(144)
> library(caTools)
> spl = sample.split(training$classe, SplitRatio = 0.7)
> train = subset(training, spl ==TRUE)
> validation = subset(training, spl == FALSE)

Then, the train set has 13735 observations. The validation set has 5887 observations.

Create Model
We use Random Forest algorithm to train the model, since RF is one of the most robust and accurate classification algorithm,  and we use a 4 fold CV to prevent overfitting.
     The procedure is as this:
> controlRF <- trainControl(method = "cv", 5)
> modelRF <- train(classe ~ ., data = train, method = "rf", trControl = controlRF, ntree = 200)
> modelRF
Random Forest

Then we get the result:

13735 samples
   52 predictor
    5 classes: 'A', 'B', 'C', 'D', 'E'

No pre-processing
Resampling: Cross-Validated (5 fold)

Summary of sample sizes: 10987, 10989, 10987, 10988, 10989

Resampling results across tuning parameters:

  mtry  Accuracy   Kappa      Accuracy SD  Kappa SD  
   2    0.9890064  0.9860932  0.001999337  0.002527211
  27    0.9903172  0.9877514  0.002617216  0.003308718
  52    0.9785950  0.9729210  0.003226834  0.004069693

Accuracy was used to select the optimal model using  the largest value.
The final value used for the model was mtry = 27. 

Use the validation set to estimate the out sample error. We can get :

> predValidation <- predict(modelRF, validation)
> confusionMatrix(validation$classe, predValidation)
Confusion Matrix and Statistics

          Reference
Prediction    A    B    C    D    E
         A 1671    2    1    0    0
         B    6 1127    6    0    0
         C    0    3 1018    6    0
         D    0    3   10  952    0
         E    0    0    1    2 1079

Overall Statistics
                                         
               Accuracy : 0.9932         
                 95% CI : (0.9908, 0.9951)
    No Information Rate : 0.2849         
    P-Value [Acc > NIR] : < 2.2e-16      
                                         
                  Kappa : 0.9914         
Mcnemar's Test P-Value : NA             

Statistics by Class:

                     Class: A Class: B Class: C Class: D Class: E
Sensitivity            0.9964   0.9930   0.9826   0.9917   1.0000
Specificity            0.9993   0.9975   0.9981   0.9974   0.9994
Pos Pred Value         0.9982   0.9895   0.9912   0.9865   0.9972
Neg Pred Value         0.9986   0.9983   0.9963   0.9984   1.0000
Prevalence             0.2849   0.1928   0.1760   0.1631   0.1833
Detection Rate         0.2838   0.1914   0.1729   0.1617   0.1833
Detection Prevalence   0.2844   0.1935   0.1745   0.1639   0.1838
Balanced Accuracy      0.9979   0.9952   0.9904   0.9945   0.9997
From the overall statistics we can know that the accuracy is 0.9932, so that the out sample error is 1 - 0.9932 = 0.0068.

predict on the test set
     So far we know that our model performs well on the validation set, thus we can use it to predict on the testing set. And before we conduct the prediction , we should remove the "problem_id" feature from the testing set.

> result <- predTesting <- predict(modelRF, testing)
> result
[1] B A B A A E D B A A B C B A E E A B B B
Levels: A B C D E

By far, we have get the prediction of the 20 testing observations.

