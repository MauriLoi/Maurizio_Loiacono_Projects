
## [Body Mass Index BMI fitting Distribution with R](https://github.com/MauriLoi/Body-Mass-Index-BMI-fitting-Distribution-with-R)

The data used for the study are taken from a marketing campaign dataset based on a case of a retailer company in computer accessories. The data set contains 1500 customer records. Each record consists of 19 variables, which includes socio-demographic and product ownership information. 
The work is developed following the below index, starting from the Data Understanding , untill the development of a logistic regression Model.


1. Body Mass Index (BMI) Data set

   1.1  Introductionto to the Body Mass Index (BMI) data set 
    
   1.2  Selection of all the possible Distributions

   1.3  Distribution choice
    
   1.4  Output the Parameters
   
### 1.1 Introduction to the Body Mass Index (BMI) data set 

The aim of this section of the report is to analyze the data set previous given of the Body Mass Index(BMI) from the Fourth Dutch Growth Study, Fredrisk et al.(2000a) and find a suitable probability distribution to fit at the data. The dataset reports 7482 observations and has as explanatory variable the age. The age range from 0.03 (3 days) to 21.70 (21 years and 7 months).The first step is to create a sub set of observations for a single year. I have decided to choose as a year of my subset data the age from 14 to 15.
Fitting a distribution is the process of finding a mathematical function that represent at the best a statistical variable in our case the BMI. In practice given the unknown distribution density (pdf) derivate from our observation sample that is an univariate continuous distribution with domain[0,+∞ ] we need to select an appropriate distribution that is able to approximate the behavior of the empirical data. I will use a two steps approach the fitting stage and the diagnostic one. I will fit different distribution and compare them using the generalized Akaike information criterion (GAIC) given the nature of not nested gamlss models. After I have compered the different result I will choose the one with the smallest GAIC(k) after the selection of the value k. Reminding the fact that GAIK(k)=GD + (k *df), where df is the effective degree of freedom used in the model and GD is the fitted Global deviance. The first approach is to explore the data with histograms and short descriptive stats, the frequency histogram with 60 bins of the values, the empirical density function and the empirical cumulative frequency distributions are reported below (see figure 1,3 and 4 for the BMI data set ). Descriptive statistic of the distribution as the mean, standard deviation, skewness, kurtosis this could be done using the descdist(). A skewness-kurtosis plot proposed by Cullen and Frey (1999) provided by the descdist() function is shown above (see Figure 2 for the BMI data set).

![](/images/Picture1.png)   

Figure 1 Histogram of the frequencies distibution of the variable BMI)  Figure 2 Cullen and Frey graph)

![Cullen and Frey graph](/images/Picture2.png)

Figure 2 Cullen and Frey graph)

![](/images/Picture3.png)

Figure 3 Shows the pdf() of the BM &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;   Figure 4 Shows the cdf() of the BMI


The skewness and kurtosis could really help in the identification of the best fit for our distribution. Remanding the fact that when the value of the skewness is zero the distribution is a normal, perfectly symmetric around his mean, if we find a positive or negative skewness is the evidence of a non-symmetric distribution.
The kurtosis value quantifies the weight of tails in comparison to the normal distribution for which the kurtosis equals 3.
If we take a look on the Cullen and Frey plot, some of the distributions are represented as a single point due to the fact that they can only assume specific value of skewness and kurtosis. For other distributions, areas of possible values are represented, consisting in lines (as for gamma and lognormal distributions), or larger areas (as for beta distribution).Is always good to keep in mind that the suggestion given by the Cullen an Grey is always an approximation given the non-robust nature of the skewness and kurtosis and them uncertainty even when the method of the bootstraps(random sampling with replacement) has been applied to deal with this aspect. No consistent information has been given from the Cullen and Frey plot the data doesn’t follow any well know distribution. Analyzing the graphs of the distribution we see that it is slightly skewed on the right(positive skewness ) and leptokurtic (positive kurtosis). Knowing this we can start to formulate some hypothesis. In our case the distribution has 𝜇 = 19.748870, 𝜎 = 2.268981, 𝜗 = 1.510098 𝑎𝑛𝑑 𝜏 = 9.110169. A good idea is to start from the normal distribution even if we already know it will not be the best fit but it will give us some idea on the overall behave of the distribution.

###  1.2 Selection of all the possible Distributions

The next step is to identify the possible ‘candidate’ distributions to be fitted and tested. Given the nature of the distribution of our data we can restrict the field to few functions able to model the skewness and the kurtosis. The Box- Cox Cole-Green family ,GA(gamma) the gamma distribution is appropriate for positively skew data, IG(inverse Gaussian) the inverse Gaussian distribution is appropriate for highly positive skew data, SEP1(Skew power exp, t1, parametrization of PE, TF(T family distribution) the t family distribution, is symmetric but able to model leptokurtosis), PE(Power exponential, the power exponential distribution is suitable for leptokurtic and platykurtic data.
Using the histDist() funtion that fits constants to the parameters of a GAMLSS family distribution and them plot the histogram and the fitted distribution[1]. I will compare all the fitted distributions graphically and with the gamlss() function that returns an object of class "gamlss" I will create a set of linear model for each of the distributions and compare the GD. The results of the histDist() are shown above.

![Histogram](/images/Picture4.png) 
![Histogram](/images/Picture5.png)  
![Histogram](/images/Picture6.png)

Figure 4  histdist() for all the distribution: NO, GA, IG, SEP1, TF, exGAUS


###  1.3 Distribution choice

After running all the gamlss objects and comparing the global deviance and the histDist() graphs results we have already an idea of the best distribution to fit. The fitted BCPE and exGauss are performing better in terms of Global Deviance . The GD of the BCPE is from the summary() function 1721.73 and the one for the exGAUS is 1723.54. The global deviance is not a good indicator for the comparison of those models given the not nested nature of them.
The GAIC(k) is a better estimator for the comparison of the models. The GAIC it tells nothing about the absolute quality of a models, only the quality relative to other models. The GAIC(k) is obtained adding to the fitted global deviance a fixed penalty k for each degree of freedom used in a model, is an increasing function of the numbers of the estimated parameters, the penalty discourage overfitting, because increasing the number of parameters in the model always improve the goodness of the fitting:
GAIC(k)= GD +(k * df);
Testing the distribution under the AIC that use k=2 and SBC that use k=log(n) and different values of k can help. The sensitivity of the selected model to the choice of k can be also tested. Testing the models build on the distributions selected we have this result for k=2 (AIC) k=~6(SBC,log(n=403) and k=2.5,3,3.5,4.

![Histogram](/images/Picture7.png) &emsp; ![Histogram](/images/Picture8.png)  
![Histogram](/images/Picture9.png)

Figure 5 GAIC(k) result: k=2, k=6, k=3, k=3. 5, k=4 

Given the result of the GAIC test we can assume that the best distributions to fit our data and build the model on it is the exGAUS and or the BCPE. As the penalization in respect to the number of parameters of the model is larger k=2,3,3.5,4,6 the best model is changing. For small number of k the BCPE is chosen for larger number of k ExGAUSS is selected. Following the criteria of the GAIC i will select the exGAUS. The plotted fitted distribution and the simple linear model of the exGAUS are shown in figure 6 and 7.

![Histogram](/images/Picture10.png) &emsp;&emsp; ![Histogram](/images/Picture11.png)

Figure 6 Plotted pdf(BMI) fitted with the exGauss distribution

The exGaus distribution is fitting well the distribution of our data dealing with the skewness and the kurtosis. The fitted parameters of the distribution are shown in the next paragraph. 

![Histogram](/images/Picture12.png)

Figure 7 Linear regression model using exGaus distribution

###  1.4 Output the Parameters

The outcomes of the linear model fitted with the exGAUS distribution are shown above in figure 8. The univariate function is expressed as y = ß0 +  ß1 (age), where ß0  is the intercept of value 11.2145 and ß1 = 0.4365. The sigma and the nu coefficients are expressing the value of the fitted sigma and the nu fitted value. The sigma coefficient of our fitted model, 0.25567 is the ln of the fitted variance 1.29132 of the distribution . The same as for the nu given the log link function. The AIC has value 1727.725 that alone doesn’t have a specific mining  but just when compared with the other model as said above.

![Histogram](/images/Picture13.png)
![Histogram](/images/Picture14.png)


Residual of the fitted distribution and the fitted model are show above

![Histogram](/images/Picture15.png)




## [LMS method (cenriles) study on the Handgrip with R]()

The exercise requires analyzing a sample extracted from the study of handgrip from Cohen et al (2010) concerning gender and age. From 3766 observation is extracted a new sample of 1000 observation from the English boys. The sample observation is reported in figure 1. The usual assumptions for data analysis are the standard assumptions of the linear model, i.e. the existence of additive effects, the constancy of variance, the normality of the variables and the independence of observations. If these assumptions are not satisfied, two alternatives are possible: either to devise a new analysis that meets these assumptions or to transform the data to meet these assumptions. It is almost always easier to use a satisfactory transformation than to develop a new method of analysis. The power transformation, for the x variables, is usually needed when the response variable has an early or late spell of fast growth. In those cases, the transformation of age can stretch the time scale making the smooth curve fitting easier[2]. We do not need to power the age because the growth of the grip is almost constant during all the different ages. We don't have any exponential or strong irregular growth of the grip in the different years. The relationship between age and grip is already almost perfectly linear. The plots of the not transformed age, the power 2 transformation and the 1.499942 power using the trans.x =True argument in the LMS function are plotted above. Not relevant changes are present in the distribution of the points, this confirms our hypothesis.

![](/Handgrip%20Pictures/Figure%201%20.png)
