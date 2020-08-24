# Ai-Chatbot
#making of an intelligent ai chatbot using machine learning with python
#Import the libraries
from newspaper import Article
import random
import string
import nltk
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.metrics.pairwise import cosine_similarity 
import numpy as np
import  warnings
warnings.filterwarnings('ignore')
#Download the punkt package
nltk.download('punkt',quiet=True)
#Get the article
article=Article('https://www.robotics.org/blog-article.cfm/How-Artificial-Intelligence-is-Used-in-Today-s-Robots/117#:~:text=Artificial%20intelligence%20(AI)%20and%20robotics,capabilities%20in%20previously%20rigid%20applications.')
article.download()
article.parse()
article.nlp()
corpus=article.text
#Print the article text
print(corpus)
#Tokenization
text = corpus
sentence_list = nltk.sent_tokenize(text) #A list of sentences
#Print the list of sentences
print(sentence_list)
# Function to return a random greeting response to a user's greeting
def greeting_response(text):
  text = text.lower()


#Bots greeting response
  bot_greetings = ['hey','hi','hello','namaste']
#Users greeting
  user_greetings = ['hi','hey','hello','Wassup','namaste','greetings']



  for word in text.split():
     if word in user_greetings:
           return random.choice(bot_greetings)

def index_sort(list_var):
    length = len(list_var)
    list_index = list(range(0,length))

    x = list_var
    for  i in range(length):
        for  j in range(length):
          if x[list_index[i]] > x[list_index[j]]:
            #Swap
            temp = list_index[i]
            list_index[i] = list_index[j]
            list_index[j] = temp

    return list_index
#Create the bots response
def bot_response(user_input):
     user_input = user_input.lower()
     sentence_list.append(user_input)
     bot_response=''
     cm = CountVectorizer().fit_transform(sentence_list)
     similarity_scores = cosine_similarity(cm[-1],cm)
     similarity_scores_list = similarity_scores.flatten()
     index = index_sort(similarity_scores_list) 
     index = index[1:]
     response_flag = 0

     j = 0
     for i in range(len(index)):
        if similarity_scores_list[index[i]] > 0.0:
           bot_response = bot_response+' '+sentence_list[index[i]]
           response_flag = 1
           j = j+1
        if j>2:
           break
        if response_flag ==0:
           bot_response = bot_response+' '+"I apologize,I don't understand."
           sentence_list.remove(user_input)

         return bot_response   
#Start the  chat
    print('Ai BOT:Hey,I am Chatbot created by Aarti.And i thank her to make me.I will talk to you if she want.And if you want to exit type Bye')

exit_list =['exit','see you later','bye','see you soon','break','quit']
    while(True):
       user_input = input()
       if user_input.lower() in exit_list:
          print('Ai BOT: Chat with you later !')
          break
       else:
         if greeting_response(user_input)!= None:
            print('Ai BOT: '+greeting_response(user_input))
         else:
            print('Ai BOT: '+bot_response(user_input))
