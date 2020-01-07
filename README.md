### Introduction
In this project, I downloaded the entire month of October's github zipped files, each of which represents one
hour of a day. Of these 528 files, I chose 21 files to focus on, from October 20th to October 26th at 4-6pm.
Many fields are interesting to investigate, but the ones I specifically want to look at are how days of the week,
the existence of an organization, and their choice of programming languages relate to each other. Another
relevant investigation could be analyzing the content of the commit messages and how they vary on the
weekdays and the weekend.

### Cleaning and EDA
Getting the data out of the files in a relatively efficient period of time is perhaps the biggest challenge. Naively, I tried to read in all the files for the entire month of October, but that would take several hours, if not a day. Then, I decided to only extract the necessary fields while it's still a JSON object to avoid complete normalization.
However, the question is how many files to read in out of all these 528 files? Since I'm only concerned more
about the day of the week rather than how some fields are different in local time, I decided to pick a particular
full week during the late afternoon. In the end, I picked a date range from October 20th to October 26th, 2019 at
4-6pm to ensure the most updated information. As such, reading those 21 files that I picked would take about 3
minutes, rather than the original 30 minutes.

After putting all the data in one single dataframe, I created subsets of this dataframe, namely commits, pushed
events, forked events, and pull events. In every type of event, I examined the relation between the day of the
week and the organization or the programming language.

### Assessment of Missingness
Of all the columns that have null values, I picked the organization column. I ran 2 permutation tests to check
MAR dependence on the day of the week and on the top 5 programming languages used, respectively. Both pvalues are significantly lower than the significance level, which is 0.001. The results can be explained by the fact
that more organizations work less on the weekend than on the weekdays, and that organizations use Java more
whereas the usual users use Python more. Note that, however, the observed TVD for the second assessment is
much closer to the distribution, making the organization column less dependent on the top 5 programming
languages than on the day of the week.

Afterwards, I did another 2 permutation tests to check that the missingness of the organization column does not
depend on whether the repos have pages or how many watchers count a repo has. It's at this point that I
decided that, instead of taking all 500,000-1,000,000 rows, I'll just take a small sample to generate the
simulation. I compared two differing small sample sizes to see the effect they have on the p-value. Not
surprisingly, the different sample sizes can largely affect the spread of the distribution, thus affecting the area
under which the test statistics are as extreme as the observed statistics. This largely explains why the observed
statistics always seems very far off from the distribution, and that a non-dependent relationship is harder to
determine with such a large dataset.

### Hypothesis Test
I did a permutation test to answer whether the usage of the top 5 programming languages vary between usual
users and bot users, another permutation test to answer whether people use the top 5 programming languages
differently on weekdays and weekends.

For the first question, the setup is such:

* Null Hypothesis: Usage of the top 5 programming languages by usual users and bots have a similar distribution.
* Alternative Hypothesis: Usage of the top 5 programming languages by usual users and bots have different distributions.
* Significance Level: 0.01% because I'm using a a much larger dataset than I used for the previous 2 permutation tests.
* Test statistics: TVD because I'm checking whether the two categorical distributions are the same.
The p-value is 0, making us reject the null hypothesis in favor of the alternative hypothesis. In other words, the
usual users and bot users have different usage of the top 5 programming languages.

For the second question, the setup is such:
* Null Hypothesis: Usage of the top 5 programming languages on the weekdays and on the weekend have a similar distribution.
* Alternative Hypothesis: Usage of the top 5 programming languages on the weekdays and on the weekend have different distributions.
* Significance Level: 0.01% because I'm using a a much larger dataset than I used for the previous 2 permutation tests.
* Test statistics: TVD because I'm checking whether the two categorical distributions are the same.
The p-value is 0, making us reject the null hypothesis in favor of the alternative hypothesis. In other words,
people have different usage of the top 5 programming languages on the weekdays and on the weekend.

In both of these permutation tests, it's important to keep in mind that the dataframe used still has 200,000 rows,
which might affect the narrowness of the distribution of test statistics in some way.
