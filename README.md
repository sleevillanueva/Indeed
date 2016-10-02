#Web Scraping with Indeed.com with BeautifulSoup
##Introduction
Indeed is an online job site. In this project, we want to practice two major skills: collecting data by scraping a website and building a binary classifier. Most of the listings do not come with the salary information, so the goal is to predict the expected salaries for these listings by using the ones that do. We are going to use the location, title and summary of the job as features to predict if the salary will be high or low. One immediate benefit of knowing the range of salaries is having a guide in negotiations for what the salary should be.

##Data
I obtained the dataset through web scraping Indeed.com using BeautifulSoup.
####*Data Cleaning/Engineering*
The first thing I noticed in the dataset is the salary. Some of the salaries are in a range of values, some of them are per year, some per month and some per week and all of them were in object type. I dropped all the rows that did not have the yearly salaries, and then I proceeded to get the average of the ranges, removed the dollar sign and converted the salaries into numeric. I also fixed the location to only return “city, state”. 

After cleaning the data, I proceeded to create a column for our binary classifier. Seeing that the distribution of the salaries was skewed to the right, it made sense to use the median as a benchmark instead of the mean. I created a target column that was either 0, if the salary was below the median, and 1, if the salary was considered a high salary. I made dummy variable for company, location and title.

##Modeling
The baseline for just guessing is 50%. I ran the cross validation score for Random Forest Classifier using only dummy variables from location, with n_folds=3 and got an initial score of .551± 0.036. We can see that this isn’t a very good score, which tells us that having only the location as a feature is not sufficient. I then created a status feature that represented the titles, and ran both the location and the status and got a cross validation score of .602 ± 0.013. I tried several variations of features and multiple models using the same number of folds, and saw a range of cross validation scores from .602 to .709 (Random Forest with all the dummy variables (location, company and title) and the status). I went ahead and used CountVectorizer without english stop words and a minimum of 3 occurrences of the word for the summaries of the jobs.

##Results
The model that gave the highest accuracy was Logistic Regression with all the dummy variables, status and the summary. Our highest accuracy is 0.714 ± 0.015, which is about 20% higher than just guessing. I scraped a few more job postings and this time without the salaries. I then used the logistic regression to predict if the salaries would be high or low for these positions. Additionally as a bonus, I used glassdoor’s API to get the ratings of the companies that had high salaries. The ratings may also be a good deciding factor when choosing between jobs.
