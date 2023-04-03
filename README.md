# Twitter Analysis: The Data Cleaning Challenge

<img src="https://github.com/Ebuka456/Twitter-Analysis-The-Data-Cleaning-Challenge/blob/main/Data%20Cleaning%20Challenge/istockphoto-1473433691-612x612.jpg" alt="Alt text" style= "width: 900px; height: 450px"/>

## Introduction
The #datacleaningchallenge is an twitter event aimed at promoting best practices in data cleaning. The challenge encourages participants to share their experiences, tips, and tricks in data cleaning by working on a dirty data. It also served as a medium for enthusiast to get a feel of what data cleaning is all about while receiving mentorship from the organizers. The online event launched on the 9th of March 2023 via a twitter space and the event lasted for the whole or March.

This project is a Python-based analysis of tweets related to the #datacleaningchallenge. The Data Visualization was done using Power BI. I wrote a Medium post explaining my insights in more details. Check it out [HERE](https://medium.com/@okonkwoebuka456/the-data-cleaning-challenge-a-twitter-data-analysis-project-c25ae4a32dd3)

## Problem Statement 
The Organizers of the #datacleaningchallenge that took place in March are looking to start another challenge in April. I aim to provide insights on the data gotten from the challenge, how people perceive data cleaning, the most talked about tools which could give a hint on the tools the participants used and the strategies on how to make the next challenge even bigger.

## Data Sourcing 
I gathered my data by using python Snscrape library. I scraped my twitter data using the #DataCleaningChallenge hashtagg from 1st of March to 31st of March 2023. The function I used to scrape the data is shown beloow

```python
# to define a function that scrapes tweet from tweet

def scrape_hashtag_tweets(hashtag, start_date, end_date):
    """
    Scrapes all tweets containing a certain hashtag within a specified time frame,
    ignoring case sensitivity.
    Args:
        hashtag (str): the hashtag to scrape, without the "#" symbol
        start_date (str): the start date in "YYYY-MM-DD" format
        end_date (str): the end date in "YYYY-MM-DD" format
    """
    # Convert the start and end dates to datetime objects
    start_dt = dt.datetime.strptime(start_date, "%Y-%m-%d")
    end_dt = dt.datetime.strptime(end_date, "%Y-%m-%d")

    # Create a list to store the scraped tweets
    tweets = []

    # Iterate over all tweets containing the specified hashtag
    for tweet in sntwitter.TwitterSearchScraper(f"#{hashtag} since:{start_date} until:{end_date}").get_items():
        # Ignore tweets that don't match the hashtag (ignoring case sensitivity)
        if hashtag.lower() not in tweet.content.lower():
            continue

       # Add the relevant information about the tweet to the list
        tweets.append({
            "id": tweet.id, #
            "content": tweet.content,
            "timestamp": tweet.date,
            "username": tweet.user.username,
            "userdisplayname": tweet.user.displayname,
            "userlocation": tweet.user.location,
            "retweetCount": tweet.retweetCount,
            "likeCount": tweet.likeCount,
            "language": tweet.lang,
            "source": tweet.source
        })

    return tweets
  return go(f, seed, [])
}
```
In total, I scraped 922 tweets and 11 columns. 

## Data Cleaning and Preprocessing
Data Cleaning and Preprocessing was carried out using python. The few steps I took to achieve this are
- Ensuring no duplicate tweet id
- Checking for null values and handling them if exists
- Ensuring the data types are consistent
- Extracting the content from the `source` column which appeared in html tag format

```python
# python function extract the content between the HTML tags using str.extract()
def extract_html_tags(df, column_name):
       
    content = df[column_name].str.extract(r'>(.*?)<')
    
    return content
# apply the extract_html_tags function to the 'source' column of the tweets dataframe
tweets['platform'] = extract_html_tags(tweets, 'source')
```
- Presnece of three letter language code in the `language` column. Language code should not exceed two letters except from Undefined (und)
- I also extracted the date (yyyy/mm/dd) from the `timestamp` column.

## Data Analysis and Visualization
The cleaned data set was Explored using python and then visualized using Power BI. There is no data model except the relationship between my Calendat table and my cleaned dataset. The Report I made is shown below

<img src="https://github.com/Ebuka456/Twitter-Analysis-The-Data-Cleaning-Challenge/blob/main/Data%20Cleaning%20Challenge/Twitter%20Analysis%20Report_page-0001.jpg" alt="Alt text" style= "width: 600px; height: 1000px"/>

I carried out sentiment analysis which would tell how people perceived the data cleaning challenge. Positive sentiment could indicate that users are finding the challenge to be engaging, informative or useful, while negative sentiment may suggest that users are not enjoying the challenge or having difficulty with it.

![Sentiment](https://github.com/Ebuka456/Twitter-Analysis-The-Data-Cleaning-Challenge/blob/main/Data%20Cleaning%20Challenge/sentiment.png)

I then created a word cloud to visualize the qualitative data in order find out the most commonly used words. The word cloud was done using [Word Art](https://wordart.com/)

![Word Cloud](https://github.com/Ebuka456/Twitter-Analysis-The-Data-Cleaning-Challenge/blob/main/Data%20Cleaning%20Challenge/word%20cloud.png)

For the data cleaning challenge, Participants were allowed to use the popular data analytics tools such as Power BI, Python, Excel, SQL, R and Tableau. The number of times data cleaning tools are mentioned in tweets can provide you with valuable insights into which tools are popular among Twitter users, preferred by the users for carrying data cleaning tasks and and trending tools in the data cleaning community

![tools](https://github.com/Ebuka456/Twitter-Analysis-The-Data-Cleaning-Challenge/blob/main/Data%20Cleaning%20Challenge/tools.png)

### The insights I got from the Analysis and visualization are documented in my [GitHub Repository](https://github.com/Ebuka456/Twitter-Analysis-The-Data-Cleaning-Challenge/blob/main/Data%20Cleaning%20Challenge/%23DataCleaningChallenge%20Twitter%20Analysis.ipynb) and my [Medium Post](https://medium.com/@okonkwoebuka456/the-data-cleaning-challenge-a-twitter-data-analysis-project-c25ae4a32dd3)

## Recommendations

Before I make recommendations, I would like to congratulate the Organizers and the speakers for a job well done. On the 11th of March 2023, #DataCleaningChallenge was one of the trending topics in Nigeria.

![Trending](https://github.com/Ebuka456/Twitter-Analysis-The-Data-Cleaning-Challenge/blob/main/Data%20Cleaning%20Challenge/Trending.jpg)

Some recommendations I can make are
- For wider reach and more online presence, I recommend that the organizers encourages the participants to write their experience, the things they learnt and the struggles they faced when participating in subsequent challenges organized. This way it gives the Organizers the chance to help them and improve on the sentiments from the participants.
- Looking at the most talked about tool, we can see that the users have preference tending towards Excel, SQL and Python for data cleaning. For subsequent data cleaning challenge, I recommend teaching sessions be held on how to effectively use these three tools and take their skillset to the next level. This should be done before commencement of subsequent challenges so that the participants would not be stuck when the challenges begin.
- Concerning tools that are the least frequently talked about, it could be related to the fact that the users find them difficult to use. So to help individuals get familiar with more data analytics tool, The organizers could fix session to train all the participants on one tool at a time, maybe Power BI since it has similarities to Excel.
- I analyzed for the top mentioned Twitter handles and the most common hashtags. For future challenges and to foster collaboration, I would recommend the Organizers collaborate with anyone from the names below. This could help for wider reach and bring in more users to partake in the challenge.

![handles](https://github.com/Ebuka456/Twitter-Analysis-The-Data-Cleaning-Challenge/blob/main/Data%20Cleaning%20Challenge/Top%20handles.png)

- I lastly recommend that a broadcasting strategy is put in place to make this challenge known to more users. For example, a flier could be made as a means of making the challenge look more official. You can even plead with Data Influencers to spread the word and encourage those who participated in the previous challenge to spread the word as well.

Generally, I would say the challenge was a success and it did well in terms of social media outreach but there is room for improvement. 

For subsequent challenges, I recommend a target to be set on the number of users tweeting about the challenge. A total of 502 twitter users were discovered for this challenge, 1000 could be the target for the next challenge.



