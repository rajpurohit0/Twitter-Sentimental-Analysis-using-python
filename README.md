# Twitter-Sentimental-Analysis-using-python
from textblob import TextBlob
import sys, tweepy
import matplotlib.pyplot as plt


def percentage(part, whole):
    return 100 * float(part)/float(whole)


consumerKey = "nWxslJqngvEjSQxPiEoFbNYiC"
consumerSecret = "vROCTMKIDNjbo2iyJw2M0Z39T4O6sQizhW3KCMAc5io1qBQeTE"
accessToken = "1149722534931128320-VJLXVHfCwvwQxeQDl1HoJxuixzvC6U"
accessTokenSecret ="mKW5qXzBFZyolWtYtgF7XQT6c0TTKetHKZohiPr8uqto6"


auth = tweepy .OAuthHandler(consumerKey,consumerSecret)
auth.set_access_token(accessToken,accessTokenSecret)
api = tweepy.API(auth)


searchTerm = input("enter keyword/hashteag to search about: ")
noOfSearchTerms = int(input("enter how many tweets to analyze: "))

tweets =tweepy.Cursor(api.search, q=searchTerm).items(noOfSearchTerms)


positive = 0
negative = 0
neutral = 0
polarity = 0


for tweet in tweets:
    #print(tweet.text)
    analysis = TextBlob(tweet.text)
    polarity += analysis.sentiment.polarity

    if(analysis.sentiment.polarity == 0 ):
        neutral += 1
    elif(analysis.sentiment.polarity < 0.00 ):
        negative += 1
    elif(analysis.sentiment.polarity > 0.00 ):
        positive += 1


positive = percentage(positive, noOfSearchTerms)
negative = percentage(negative, noOfSearchTerms)
neutral = percentage(neutral, noOfSearchTerms)
polarity = percentage(polarity, noOfSearchTerms)


positive = format(positive, '.2f')
neutral = format(neutral, '.2f')
negative =format(negative, '.2f')


print("how people are reaching on " + searchTerm + "analyzing" + str(noOfSearchTerms) + "tweets.")

if(polarity == 0):
    print("Neutral")
elif(polarity < 0.00):
    print("Negative")
elif(polarity > 0.00):
    print("Positiv")


labels = ['positive [' + str(positive) + '%]', 'Neutral [' + str(neutral) + '%]', 'Negative[' + str(negative) + ' %]']


sizes = [positive, neutral, negative]
colors = ['yellowgreen', 'gold', 'red']

patches, texts = plt.pie(sizes, colors=colors, startangle=90)

plt.legend(patches, labels, loc="best")
plt.title('how people are reacting on '+searchTerm+' by analyzing '+str(noOfSearchTerms)+' Tweets.')

plt.axis('equal')

plt.tight_layout()
plt.show()
