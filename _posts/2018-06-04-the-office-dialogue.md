# The Office Dialogue
###### *04 June 2018*

According to "science", [The Office](https://www.imdb.com/title/tt0386676/) is a fantastic show. 
I did some analysis of the dialogue in the show and plotted some results because data is great and statistics are fun.

Before diving in:
[here](https://github.com/zengineering/the-office-dialogue) is the project and 
[here](http://officequotes.net/) is the source of the data.

All plots only consider characters that had at least 100 lines throughout the show.

## Lines of dialogue for each character
![lines]({{"/images/the-office-lines.png"|absolute_url}})
Here we have a breakdown of the number of lines of dialogue for each character across each season.
In a totally unsurpising [fashion](https://www.dailywritingtips.com/the-spellings-of-shun/), Michael has the most to say throughout the series.
What is surprising is how much of a lead he has, given that he was almost entirely absent from the final two seasons.
Also noteworthy is that other main characters (Dwight, Jim, Pam, and Andy) all have substantially more dialogue than the rest of the crew.
Some pie will help drive that point home:

![pie](/images/the-office-pie.png)
Michael speaks over 22% of the lines in the entire show.
Michael, Dwight, Jim, Pam, and Andy collectively speak almost two-thirds of the entirety of the series dialogue.

## Appearances with at least one line
![episodes](/images/the-office-episodes.png)
This plot shows the number of episodes in which a character had at least one line of dialogue.
It's about what you'd expect, with the core group appearing in the most episodes.
Dwight is the only character to appear and speak in every episode.
Jim is in all but one episode, and Pam is in all but four.
Interestingly, Andy (introduced in season 3), is a bit ahead of some full-series characters like Kelly, Ryan, Meredith, and Toby.
Those are somewhat minor characters, though, and some of them were absent for most or all of at least one season.

## Lines per episode
![lines_per_episode_seasonal](/images/the-office-lines-per-episode-seasonal.png)
Breaking it down a bit, here we have the number of lines per episode across all seasons (i.e. the sum of each season's average lines-per-episode).
Again, the core group of Michael, Dwight, Jim, Pam, and Andy lead the pack.
Of note are Holly, Jan, David Wallace, and Deangelo, who are in fewer episodes but have more to say when they are present.

![lines_per_episode_overall](/images/the-office-lines-per-episode-overall.png)
Disregarding seasons, the overall lines-per-episode numbers are a bit different.
(This is the total number of lines a character has vs the total number of episodes they appear in).
This presentation makes it more obvious that high-profile guest characters (Deangelo, Charles, Jan, Robert California) tend to be the focus when they are around.
It also points out that Creed has very few lines in each episode. When he does speak, it's superb.

![lines_per_episode_stdev](/images/the-office-lines-per-episode-std.png)
The standard deviation (stdev) of the previous plot shows the inconsistency of some characters' presence.
Michael has a huge stdev, likely because he dominated the first seven seasons, but only had two lines in the final two seasons.
Holly and Deangelo waver significantly in their amounts of dialogue in each episode.
Long-term secondary characters like Kevin, Oscar, Toby, Meredith, Phyllis, Creed, and Stanley are relatively consistent in their reserved roles.

