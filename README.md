import requests
from bs4 import*
import pandas as pd

lista_noticias=[]

url = 'https://g1.globo.com/busca/?q='
search = input("Digite: ")

response = requests.get(url + search)
site = BeautifulSoup(response.text, 'html.parser')

itens = site.findAll('li', attrs={'class': 'widget widget--card widget--info'})

for noticia in itens:
    tittle = noticia.find('div', attrs={'class': 'widget--info__title product-color'})
    
    subtittle = noticia.find('p', attrs={'class': 'widget--info__description'})

    link = noticia.find('a', attrs={'class': 'widget--info__media widget--info__media--video'})
    if (link):
        lista_noticias.append([tittle.text, subtittle.text, link['href']])
    else:
        lista_noticias.append([tittle.text, subtittle.text])
news=pd.DataFrame(lista_noticias, columns=['Titulos', 'Subtítulo', 'Links'])
news.to_excel('Noticias pedidas.xlsx', index=False)
