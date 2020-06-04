```python
import requests
import pandas as pd
import jieba
import wordcloud
import snownlp
import numpy as np
from matplotlib import pyplot as plt
from wordcloud import WordCloud, STOPWORDS
from snownlp import SnowNLP
from PIL import Image
from bs4 import BeautifulSoup

url= 'https://comment.bilibili.com/186803402.xml'
request = requests.get(url)
request.encoding = 'utf-8'
useless_words = [['不是','这个','就是','我们','会来','一起','各位','自己','我要','啊啊啊']]
soup = BeautifulSoup(request.text, 'lxml') 
results = soup.find_all('d')
comments = [comment.text for comment in results]
comments_clean  = [comment.replace(' ','') for comment in comments]
set(comments_clean)

for i in comments:
  with open(r'C:\Users\Victor-sos\Desktop\Design\1.txt','a',encoding = 'utf-8')as fp:
       fp.write(i + '\n')

comments_clean = [element for element in comments_clean if element not in useless_words]
danmustr = ''.join(element for element in comments_clean)
words = list(jieba.cut(danmustr))
fnl_words = [word for word in words if len(word)>1]
    
wc = wordcloud.WordCloud(width=1000, font_path=r'C:\Users\Victor-sos\Desktop\Design\simhei.ttf',height=800)
wc.generate(' '.join(fnl_words))

plt.imshow(wc)
wc.to_file(r"C:\Users\Victor-sos\Desktop\Design\danmu.png")
img = Image.open(r'C:\Users\Victor-sos\Desktop\Design\1.jpg')
resized = np.array(img)

WC = wordcloud.WordCloud(
    background_color='white',
    width=1000,
    height=800,
    scale=32,
    mask=resized,
    font_path=r'C:\Users\Victor-sos\Desktop\Design\simhei.ttf'
)

WC.generate_from_text(' '.join(fnl_words))
plt.imshow(WC, interpolation="bilinear")
plt.axis('off')
plt.figure()
plt.show()
WC.to_file(r'C:\Users\Victor-sos\Desktop\Design\danmu1.png')

fp=open(r"C:\Users\Victor-sos\Desktop\Design\1.txt","r",encoding='utf-8')
lines=fp.readlines()
k=0
m=0
for line in lines:
    try:
        s=SnowNLP(line)
        k=k+s.sentiments
        m=m+1
    except:
        print("")
print(round(k/m,3))
```
