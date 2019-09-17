#Processo de conexão com a API e tratamento de texto postado por usuários do Twitter.

import tweepy #Biblioteca para conexão com API do Twitter.
#Processo de autenticação na API do Twitter.
consumer_key='Your Key'
consumer_secret='your Key'
access_token='Your token'
access_token_secret='Your token'

auth=tweepy.OAuthHandler(consumer_key,consumer_secret)
auth.set_access_token(access_token,access_token_secret)
api = tweepy.API(auth)

#Usuário do Twitter
usuario = api.get_user('acmneto_')

#Bibliotecas para pré-processamento
from nltk.corpus import stopwords 
import re
#Instância STOPWORDS
stopwords = set(stopwords.words("portuguese"))

#Criação de nova lista de palavras irrelevantes 
nova_stopwords = ['1','2','3','4','5','6','7','8','9','0','q','w','e','r','t','y','u','i','o','p','a','s','d','f','g','h','j','k','l','z','x','v','b','n','m','da','de','com','é','então','ja','já','sao','ai','vao','so','acho','até','daqui','dessa','desse','dessas','desses','assim','ia','tão','devem','fica','ficou','la','lá','ate','até','desde','só','pra','há','ha','hà','são','só','já','deixou','aí','sobre','que','durante','vai','dia','ainda','estão','deu','dar','para','r','o','e','após','sr','sra','tudo','q','tão','sendo','sem','me','as','os','isso','mas','quase','estar','ta','tá','ai','vão','lá','vá','tô']

#União de lista padrão do StopWords com a nova lista
nova_stopwords_list = stopwords.union(nova_stopwords)

#Instancias para remoção de padrões irrelevantes
nolink = r"http\S+"
caracters = r"[^@#_a-záéíóúàèìòùâêîôûãõçA-ZÁÉÍÓÚÀÈÌÒÙÂÊÎÔÛÃÕÇ0-9 ]"
nospaces = r'\s+'

#Criação do arquivo onde serão armazenados os dados transformados
saida= open('C:/users/onlyone/desktop/prefeito/prefeito.txt',mode='w', encoding='UTF-8')

#Laço para executar a instrução em toodas as postagens
for page in tweepy.Cursor(api.user_timeline, screen_name=usuario.screen_name, count=200,tweet_mode="extended",include_rts = True).pages(20):
        for tweet in page:
                #PRÉ-PROCESSAMENTO DOS DADOS
                step1= (tweet.retweeted_status.full_text if tweet.full_text.startswith("RT") else tweet.full_text).lower()
                step2= re.sub(caracters, u' ',(re.sub(nolink, u' ',(step1))))
                step3= [palavra for palavra in step2.split(' ') if not palavra in nova_stopwords_list]
                step4= u' '.join(step3)
                step5= re.sub(nospaces, ' ',(step4))
                saida.write(step5)      
saida.close()



from nltk.tokenize import TweetTokenizer
from nltk import FreqDist #Biblioteca para contagem de padrões frequêntes


tweet_tokenizer = TweetTokenizer()
with open ('C:/users/onlyone/desktop/prefeito/prefeito.txt',mode='r',encoding='UTF-8') as dados_tratados:
        
        mining1=str(dados_tratados.readlines())
        mining2= re.sub(caracters, u'',mining1)
        mining3= tweet_tokenizer.tokenize(str(mining2))
        mining4= FreqDist(mining3)

dados_tratados.close()

#Impressão de padrões frequêntes
print(mining4)
print(mining4)
print(mining4.max())
print(mining4.most_common())

#Plotagem Gráfico 1
mining4.plot(60, cumulative=False,title= "Gráfico de Padrões Frequêntes - Prefeito de Salvador")


#Plotagem núvem de palavras

from PIL import Image
from nltk import FreqDist
import numpy as np
from wordcloud import WordCloud
import matplotlib.pyplot as plt
 
filter_words = dict([(elemento, qtd) for elemento, qtd in mining4.most_common() if len(elemento) > 1])
template = np.array(Image.open('C:/Users/Onlyone/Desktop/nuvem.png'))
nuvem = WordCloud(width=1366, height=720, background_color="white",max_font_size=140, max_words=100, mask= template).generate_from_frequencies(filter_words)
plt.imshow(nuvem)
plt.title('Núvem de padrões Frequêntes - Prefeito de Salvador')
plt.axis("off")
plt.show()

#Plotagem Gráfico 2
import matplotlib.pyplot as plt

filter_words = dict([(elemento, qtd) for elemento, qtd in mining4.most_common() if len(elemento) > 3])

x = (list(filter_words.keys())[:20])
y = (list(filter_words.values())[:20])

plt.plot(x, y,linestyle="--")
plt.scatter(x, y, color="green",marker="^")
plt.title("Avaliação de Padrões Frequentes - Prefeito de salvador", color="red")
plt.xlabel("Frequência")
plt.ylabel("Padrões")
plt.show()

#Plotagem Gráfico 3
import matplotlib.pyplot as plt

x1 = (list(filter_words.keys())[:15])
y1 = (list(filter_words.values())[:15])
#Eixos
plt.title("Graphic of bar 2")
plt.xlabel("Eixo X")
plt.ylabel("Eixo Y")
plt.barh(x1, y1, label = "Grupo 1", color = 'red')
plt.legend()
plt.show()


#Conulta de ponstagens
public_tweets = api.home_timeline(count=3)
for public in public_tweets:
        print('Usuário: ',public.user.screen_name)
        screen=(public.user.screen_name)
        print('Origem: ',public.source)
        print('id_publicacão: ',str(public.id))
        print('Data_publicação: ',public.created_at)
        print('Texto_Publicado: ',public.text)
        usuario = api.get_user(str(screen))
        print("===Info. do usuário===")
        print('Usuário: ',usuario.name,'Local: ', usuario.location)
        print('Seguindo: ',str(usuario.friends_count),'Seguidores: ',str(usuario.followers_count),'Tweets: '+str(usuario.statuses_count),'Curtidas: '+str(usuario.favourites_count))
        print("==================================")
       
print('Esta seção mostrará meus próprios detalhes da conta do Twitter')

minha_conta = api.me()
print('Meu nome: '+ minha_conta.name)
print('Minha localização: '+ minha_conta.location)
print('Seguindo: '+ str(minha_conta.friends_count))
print('Seguidores:' + str(minha_conta.followers_count))

print('===')
print('Esta seção mostrará meus recentes retweents')

me_retweeted = api.retweets_of_me()

for tweet in me_retweeted:
        print(tweet.created_at, tweet.text)

print('================================================')
print('Esta seção mostrará informações do usuário para qualquer usuário do twitter')

usuario = api.get_user('jairbolsonaro')
print('Nome: ' + usuario.name)
print('Localização: ' + usuario.location)
print('Seguindo: ' + str(usuario.friends_count))
print('Seguidores: '+ str(usuario.followers_count))

print('================================================')
print('This section will show the most recent tweets of any user')
