<h1>Cloud Service Providers:  Google Cloud vs Amazon Web Services</h1>

Recent articles have posed the question, "Is the future of Data Science in the cloud?"
Cloud computing has become more cost efficient, secure and reliable.  It saves costs on infrastructure and helps speed up computing tasks such as dealing with very large sets of data.  This led me to ask: 
 - Is there a difference in performance between cloud service providers?
 - Does Google Cloud or Amazon Web Services have better performance?
 - What influences price?

### Data
I gathered data from Cloudspectator.com, a benchmarking and consulting firm focused on the Cloud computing industry.  The data contained the following features: provider, location, category, vm, cpu, ram, single score, multi score, price and price score.

<img width="991" alt="data" src="https://user-images.githubusercontent.com/30739929/56484392-5c23bd80-649d-11e9-8882-b5643a7846fa.png">

### Initial EDA
I first took a look at the distribution of data

![distribution - single score](https://user-images.githubusercontent.com/30739929/56484466-bde42780-649d-11e9-9880-ef569222c298.png)
![distribution multi score](https://user-images.githubusercontent.com/30739929/56484471-c0df1800-649d-11e9-9e85-46efbc115f77.png)
![distribution - price](https://user-images.githubusercontent.com/30739929/56484475-c4729f00-649d-11e9-9c20-357ae5cbbeb6.png)

Using Seaborn, I graphed the mean single score by provider and the mean multi score by provider

![bar - provider vs single](https://user-images.githubusercontent.com/30739929/56484630-888c0980-649e-11e9-9968-456e3fb671ed.png)
![bar - provider vs multi](https://user-images.githubusercontent.com/30739929/56484635-8e81ea80-649e-11e9-8d43-8c74cdcdcfac.png)

### Is performance significantly different?
I analyzed the data to see if they were significantly different.


<img width="829" alt="performance scores" src="https://user-images.githubusercontent.com/30739929/56484783-43b4a280-649f-11e9-9935-01c229577a68.png">
The difference between AWS and Google Cloud returned a P-Value of 7.217.  The difference between them are not statistically significant.  I then compared the provider with the highest performance score, Vultr, with the provider with the lowest performance score, Microsoft Azure, and found that while the P-Value was much lower at 0.067, the difference between them was still not statistically different.


### What affects price?
I then took a look if the data had any relationships.


![scatter - single vs price](https://user-images.githubusercontent.com/30739929/56484868-b4f45580-649f-11e9-8204-3d79c6f67f86.png)
![scatter - multi vs price](https://user-images.githubusercontent.com/30739929/56484867-b4f45580-649f-11e9-9cfe-72674266227d.png)

![scatter ram vs price](https://user-images.githubusercontent.com/30739929/56484869-b4f45580-649f-11e9-980d-3f920c5c363c.png)

There seemed to be no relationship between score and price.  The higher score did not yield or command a higher price.  There did seem to be a positive linear relationship between RAM and price though.

### Regression Models
I ran a regression model with the following features single score, multi score, price score and ram (gb).  This initial model returned a r-squared of 0.769.  R-Squared translates in the percent of variance explained by the model. 

<img width="532" alt="model - initial all variables" src="https://user-images.githubusercontent.com/30739929/56501400-a710f580-64dc-11e9-896f-a29092a658af.png">

I added dummy variables of each provider and after checking for multicolinearity and high P-Values, I narrowed down my features to Single_Score, RAM_GB, Provider_DigitalOcean, Provider_Google, Provider_IBM, Provider_Joyent, Provider_Linode, Provider_Azure, Provider_UpCloud, Provider_Vultr.  

<img width="551" alt="model dummy variables" src="https://user-images.githubusercontent.com/30739929/56501839-45ea2180-64de-11e9-9c89-ccf1672de48a.png">

This returned a r-squared value of 0.942.  With this model I made regression plots.

![regression](https://user-images.githubusercontent.com/30739929/56501974-cc9efe80-64de-11e9-8dad-3a34f2358720.png)

and plotted the residuals.

![resid](https://user-images.githubusercontent.com/30739929/56502183-8007f300-64df-11e9-9640-97a59e7025b4.png)

![residplot](https://user-images.githubusercontent.com/30739929/56516092-975ad680-6507-11e9-9d01-3875d0518b65.png)


I finally plotted interactions to see if there are differences between vendors while looking at price and single core performance.  The lines are not additive and there are interactions.

![interactions - price vs single](https://user-images.githubusercontent.com/30739929/56502364-04f30c80-64e0-11e9-87e6-9e4531621142.png)

We can see that Azure has the lowest performance score but starts at the highest price.  Azure then approaches the performance score of Amazon and Google, but at a much higher price.  Amazon is the cheapest of the 3 providers and its performance stays same.  

I also plotted interactions to see if there are differences between vendors while looking at price and amount of RAM.
![interactions - ram vs price](https://user-images.githubusercontent.com/30739929/56502365-04f30c80-64e0-11e9-908e-3c0c4e685672.png)

Azure again starts off at the highest price point but of the three vendors, it also offers the highest amount of RAM.  Amazon is the cheapest of the options but offers the least amount of RAM.


In conclusion, the differences between the performances of cloud providers were not statistically different.  Interactions showed how different vendors had different starting price points.
