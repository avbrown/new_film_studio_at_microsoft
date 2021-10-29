# New-Film-Studio-at-Microsoft
This repository uses exploratory data analysis to provide a business stakeholder with recommendations about what types of films are currently doing the best/worst at the box office.
 
 
# Bottom Line Up Front
There are some general best practices for making ROI in movies.
1. The genres to invest in are...
2. Trust directors over actors
3. A good runtime is...
4. The best months to release are
 
 
## Business Understanding
 
Our assumption here is that our stakeholders are executives at Microsoft. The problem they face is how to most efficiently invest Microsoft's resources into movie making ventures. Our recommendations are made on the assumption that a movie studio WILL be set up, and do not address other avenues of investment. We assume that the stakeholders derive their compensation based directly or indirectly on the value of the company, which in turn is based directly or indirectly on profits and losses.
 
Therefore, we are choosing to base our recommendations primarily using Return on Investment as the desired outcome metric. Factors like prestige, awards, and ratings were considered only insofar as they affected ROI.

Given the datasets on hand, it is assumed that production budgets and worldwide gross represent the overall investment and return, respectively

Ultimately we chose to focus on topline descisions that could be made by executives in the board room the selection of genre, talent, release date and final run time

All that said, a fair amount of ROI is driven by outliers. Even with the genre, talent, release date and final run time.[^1]
  

## Analyzing the Data
We used the following datasets:

bom.movie_gross.csv.gz 
imdb.name.basics.csv.gz
imdb.title.akas.csv.gz 
imdb.title.basics.csv.gz
imdb.title.crew.csv.gz
imdb.title.principals.csv.gz
imdb.title.ratings.csv.gz
rt.movie_info.tsv.gz
rt.reviews.tsv.gz
tmdb.movies.csv.gz
tn.movie_budgets.csv.gz`
 
These sets were provided by the course as our starting points in exploration.  They contain information about how much movies made domestically and worldwide, Genres, runtime of the movies, release time, and crews of the movies. However none of these datasets alone had all the data we needed.
 
This information is incomplete in some ways. Some data points were present in one data set, but not others, leading to lost information during merges.
 
Additionally, some of these numbers for both production budget and gross are only as good as their sources.[^2] Without a forensic accountant and access to proprietary documents, we will simple have to deal with this.
 
It is also reasonable to suspect that these data sets may be skewed towards Western made films, and Hollywood films in particular. Some data sets are also skewed in time, deal with more recent releases where some types of data and reporting are more readily available. In other cases the datasets were more up to date on recent develpments than others.

## The Notebooks 

Early data exploration was conducted in ancillary notebooks, most named after the cohort member doing the exploration, others name after specific tasks or threads being explored. Important information and final findings from those explorations has been pulled from those early notebooks into the Phase1_Project_Notebook.

## Navigating Phase1_Project_Notebook

The first major block of the Phase1_Project_Notebook contain the cleaning of the and the creation of the money_metrics_merge_ready_df, a data frame that contains needed ROI calculations and is also ready to merge onto other data frames. Considerable care was made to adjust movie names to make them unique.[^3]  This process was partially automated, with some cleaning by hand when duplicates fell to managable numbers. Other data frames had unique values built in, which made later merges easier.

After some further cleaning to get distinct genres, the notebook then proceeds to calculate the mean ROI by genre, resulting in this graph:

*******



*******


The next section of code deals with the relative success and failure of directors and actors.

Because the data sample size dealing with actors and directors are so small and noisy, actors and directors simply had their films broken into two categories. "Above average" films simply return a better ROI than the mean film. Even the best directors have only reached this relatively low mark four times in a career!

A similar analysis was performed for actors.


Next we examined "poor" performing films. Now, there are many poor performing films. In this case we're examing studios specifically selecting which directors to grant more resources, or actors to bring into larger films. With that in mind, we've also limited this breakdown to films with high budgets. In essence, if someone took an early role or a small indy film to success, we want to reward that. However, if an actor takes a flyer on an art film or happened to be in some minor flops before they hit big, we're not trying to punish that.

What we're looking at here is how well a track record of big successes can *ensure* that a film won't face big failures. ROI tends to be flukey, we want to catch lighting without investing big in projects that get us burned.

Ultimately, many of the names on our list of repeatably good film makers also end up on our list of notable repeat failures. However, it seems that repeatably successful directors tend to much more rarely also put out flops. 'Good' actors, meanwhile, can't always seem to save underperforming films.

*******
![README md graph](https://user-images.githubusercontent.com/81991136/139350135-0d5755b3-294f-48fd-bc84-cb51964ad818.png)
*******

In effect, if you must chose between a proven director or a proven actor, the director will more reliably avoid failure.









README.md includes concise summary of project with all data science steps
README.md links to presentation and sources
README.md includes instructions for navigating the repository

[^1]: As an example Warner Brothers chased the Marvel model - long films about superheroes with a marketable cast and moneymaking directors -  and still stumbled. Given the unpredictable nature of the industry we recommend a strategy of broad diversification within our best practices, rather than betting large on a very specific combination.

[^2]: For instance, one outlier in the dataset, a film called "Deep Throat" is alleged to have an inflated box office return. A 2005 LA Times article (https://www.latimes.com/archives/la-xpm-2005-feb-24-fi-golden24-story.html) raises numerous issues with the box office claims, including allegations of mob involvement in adjusting box office numbers.


[^3]: To illustrate this issue, here's a list of films called 'Home' pulled from Wikipedia:

Films

    Home (1915 film), a 1915 silent film featuring A.V. Bramble
    Home, a 1919 American film directed by Lois Weber
    Home, a 1998 short film starring Alan Devine
    Home (2003 film), a UK TV dark comedy
    Home (2006 film), a documentary by Alan Cooke
    Home (2008 American film), an American drama film
    Home (2008 Swiss film), a Swiss drama
    Home (2009 film), a documentary by Yann Arthus-Bertrand
    Home (2011 film), a Russian drama film
    Home (2012 film), a Thai drama directed by Chookiat Sakveerakul
    Home, the working title of the 2014 film At the Devil's Door
    Home (2015 film), a computer-animated film by DreamWorks Animation
    Home (2016 American film), an American horror film
    Home (2016 Belgian film), a Belgian film
    Home (2016 British film), a United Kingdom/Kosovo drama film
    Home (2020 film), a German-French-Dutch drama film
    Home (2021 film), a Malayalam drama film
