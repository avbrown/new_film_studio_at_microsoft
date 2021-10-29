# New-Film-Studio-at-Microsoft
This repository uses exploratory data analysis to provide a business stakeholder with recommendations about what types/characteristics of films are currently doing the best at the box office.
 
 
# Bottom Line Up Front
These are our recommendations based on ROI of films:
1. The genres to invest in are horror, mystery, and thriller.
2. Trust directors over actors.
3. A long runtime is okay.
4. The best months to release are dependent on genre.
 
 
## Business Understanding
 
Our assumption here is that our stakeholders are executives at Microsoft. The problem they face is how to most efficiently invest Microsoft's resources into film production. Our recommendations are made on the assumption that a film studio WILL be set up, and do not address other avenues of investment. We assume that the stakeholders derive their compensation based directly or indirectly on the value of the company, which in turn is based directly or indirectly on profits and losses.
 
Therefore, we are choosing to base our recommendations primarily using ROI as the desired outcome metric. Factors like prestige, awards, and ratings were considered only insofar as they affected ROI.

Given the datasets on hand, it is assumed that production budgets and worldwide gross represent the overall investment and return, respectively.

Ultimately we chose to focus on topline decisions that could be made by executives in the board room: the selection of genre, talent, release date and final run time.

All that said, a fair amount of ROI is driven by outliers. Even with the 'correct' genre, talent, release date, and final run time, some films will flop.[^1]
  

## Analyzing the Data
We used the following datasets:

bom.movie_gross.csv.gz \
imdb.name.basics.csv.gz \
imdb.title.akas.csv.gz \
imdb.title.basics.csv.gz  \
imdb.title.crew.csv.gz  \
imdb.title.principals.csv.gz  \
imdb.title.ratings.csv.gz  \
rt.movie_info.tsv.gz \
rt.reviews.tsv.gz  \
tmdb.movies.csv.gz  \
tn.movie_budgets.csv.gz 
 
These sets were provided by the course as our starting points in exploration.  They contain information about how much films made domestically and worldwide, genres, runtime of the films, release time, and crews of the movies. However, none of these datasets alone had all the data we needed.
 
This information is incomplete in some ways. Some data points were present in one dataset, but not others, leading to lost information during merges.
 
Additionally, some of these numbers for both production budget and gross are only as good as their sources.[^2] Without a forensic accountant and access to proprietary documents, we will simply have to deal with this.
 
It is also reasonable to suspect that these datasets may be skewed towards Western-made films and Hollywood films in particular. Some datasets are also skewed in time, and deal more with recent releases where some types of data and reporting are more readily available. In other cases the datasets were more up to date on recent develpments than others.

## The Notebooks 

Early data exploration was conducted in ancillary notebooks, most named after the cohort member doing the exploration, others name after specific tasks or threads being explored. Important information and final findings from those explorations has been pulled from those early notebooks into the __*Phase1_Project_Notebook*__. The more exploratory notebooks have been moved to the subfolder titled exploratory_files.

# Phase1_Project_Notebook 

The first major block of the __*Phase1_Project_Notebook*__ contains the cleaning and creation of the money_metrics_merge_ready_df, a dataframe that contains needed ROI calculations and is also ready to merge onto other dataframes. This dataframe was read from __*tn.movie_budgets.csv.gz*__. Considerable care was made to adjust film names to make them unique.[^bignote]  This process was partially automated, with some cleaning by hand when duplicates fell to managable numbers. Other data frames had unique values built in, which made later merges easier.

### Genres

The next major section involves the cleaning and creation of the g_df dataframe, which was read from __*imdb.title.basics.csv.gz*__. This dataframe had relevant genre types for each movie title listed as well as release dates, which was useful for merging with money_metrics_merge_ready_df which had ROI data. 

This next bit is all about data cleaning and duplication resolution. There were some complications we ran into with merging g_df with money_metrics_merge_ready_df. The main difficulty lied in the fact that there were film name duplications in both dataframes. We came up with a solution by essentially tacking on the release year of duplicated names in both dataframes and merging on those updated names. However, what we didn't think of was, what if one film name had a duplicate in one datframe but not in the other. In other words, they would not merge together properly because one data frame would have a film name with release year and the other wouldn't. To fix this, we performed ANOTHER merge with a copy of the original g_df before any cleaning or merging whatsoever (so essentially __*imdb.title.basics.csv.gz*__). After merging, there were still a few film name duplications that we had to remove "manually", which was slightly tedious but proved effective. This required going to the sources, IMDB and TheNumber, and comparing the films' info on those websites to make the correct match. Afterwards, we had a fully merged dataframe with no duplicate film records called mm_and_g_df, which we used to pull specific genre types and ROI data from.

Next, we decided to plot bar charts comparing mean ROI vs. Genre and mean Production Budget vs. Genre for all films released in the past decade. We didn't want to include films released too long ago as those wouldn't show recent trends with film genres. Although when films included back to 1980 were incorporated, the overall trend of the top three genres with highest ROI did not change (aka Mystery, Horror, and Thriller). In addition, we decided to use the mean as the measures of central tendency for both plots because we wanted to embrace and introduce "big hit" movies, aka outliers, into the visualization since there tends to be a lot of uncertainty in the film industry.

For the moment you all have been waiting for, these are the resulting graphs based on data related to film genres:

![Capture](https://user-images.githubusercontent.com/92337358/139494985-092eb940-05e7-4e2d-846c-f1c736d3c45b.PNG)

Despite a seeming saturation of the market in different genres, Thrillers, Horrors, and Mystery films are all top three in returns. In addition, they tend to be lower budget films, which could bode well for a brand new film studio that's entering show biz!

### Horror, Mystery, and Thriller films have a high ROI.

The next section of code deals with the relative success and failure of directors and actors.

Because the data sample size dealing with actors and directors are so small and noisy, actors and directors simply had their films broken into two categories. "Above average" films simply return a better ROI than the mean film. Even the best directors have only reached this relatively low mark four times in a career!

A similar analysis was performed for actors.

Next we examined "poor" performing films. Now, there are many poor performing films. In this case we're examing studios specifically selecting which directors to grant more resources, or actors to bring into larger films. With that in mind, we've also limited this breakdown to films with high budgets. In essence, if someone took an early role or a small indy film to success, we want to reward that. However, if an actor takes a flyer on an art film or happened to be in some minor flops before they hit big, we're not trying to punish that.

What we're looking at here is how well a track record of big successes can *ensure* that a film won't face big failures. ROI tends to be fluky, we want to catch lighting without investing big in projects that get us burned.

Ultimately, many of the names on our list of repeatably good film makers also end up on our list of notable repeat failures. However, it seems that repeatably successful directors tend to much more rarely also put out flops. 'Good' actors, meanwhile, can't always seem to save underperforming films.

![README md graph](https://user-images.githubusercontent.com/81991136/139350135-0d5755b3-294f-48fd-bc84-cb51964ad818.png)

### In effect, if you must choose between investing in a proven director or a proven actor, the director will more reliably avoid failure.

### Runtime

Measuring the three central tendencies, there's a continuous increase over time in runtime lengths. 
The next section of code deals with the values of how long films run when shown in film over the years. 
Since the datasets focus on different time spans throughout history of cinema it is okay to look at them individually and allow their findings to support one another.
Now we examined and want to find what was the most common movie length that occured throughout history. 
Using the IMDB dataset the findings where values fell within that past 10 years, we found that 90 minutes was the most frequent runtime to occur in the last 10 years. 
In essence we are seeing that the feature length standard that was implemented back in 1920 is still used to this day.
After more analysis the push of the avergae year using the 90 minute runtime length has been leaning more towards the earlier part of the decade rather than being in the middle.
The longer runtimes being skewed towards the later part of the decade.

Breaking into the Rotten Tomatoes dataset you see the dataset ranges back to the 1920's and comes to modern times.
We examined the data and saw the 90 minute mark is still the most common runtime throughout history.
But further analysis see's that the mid-1980's and prior have more occurances of the 90 minute mark coming up.
As you look more and more into the data you see that the central tendecy of modern film times are greater than 90 minutes, the standard feature time.
Ultimatley we are wanting to show that there is a trend of longer film runtimes and that a new studio does not have to be restricted to a standard set back in the 1920's and it is safe to have longer films.
Modern films with longer than standard feature film lengths are still having ROI's greater than 1.
![Rogerfixedlinegraph](https://user-images.githubusercontent.com/92402366/139494101-1086b723-eaf1-4c6b-8d0c-3dd641ce138d.png)

### Accept the trend toward runtimes of longer than 90 minutes.

The next section of our code explores the seasonal variation in ROI. Seasonality has long been a part of the Hollywood box office schedule, from summer blockbusters, Halloween horrors, and awards season drama. We broke down our datasets by month to explore this phenomenon.

To a certain extent, this confirms our priors. July and May provide overall good returns, December and January less so.  However, there are two things to note. 

First, this data is noisy. Year to year there is no guarentee of good returns in a given month, and these averages are shaped in part by significant outliers.

Second, it would be naive to simply combine the data from above regarding genre with the findings above, and produced a horror film in July. Conventional wisdom holds that October is a better time for Horror. We cluster the monthly data by our top three genre, resulting in the following graph: 

![Months and Genres for notebook](https://user-images.githubusercontent.com/85522002/139470777-b359ad2e-4a9e-4f08-a41d-f3dea21a26ac.png)

This confirms some priors, but reveals some exciting new options. January might be a good month for horror, for instance, and a horror film in July doesn't seem bad at all! Note that our genre dataset covers multiple options, so these films might be hybrid horror-action or horror-sci-fi.

### We recommend tailoring release dates and genres according to the data here.

### Presentation available as [PDF](https://github.com/avbrown/New-Film-Studio-at-Microsoft/blob/main/presentation.pdf)

[^1]: As an example, Warner Brothers chased the Marvel model - long films about superheroes with a marketable cast and moneymaking directors -  and still stumbled. Given the unpredictable nature of the industry we recommend a strategy of broad diversification within our best practices, rather than betting large on a very specific combination.

[^2]: For instance, one outlier in the dataset, a film called "Deep Throat" is alleged to have an inflated box office return. A 2005 LA Times article (https://www.latimes.com/archives/la-xpm-2005-feb-24-fi-golden24-story.html) raises numerous issues with the box office claims, including allegations of mob involvement in adjusting box office numbers.


[^bignote]: To illustrate this issue, here's a list of films called 'Home' pulled from Wikipedia:
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
