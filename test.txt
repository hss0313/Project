#!/usr/bin/env python
# coding: utf-8

# In[111]:


import re
import pandas as pd
import glob
from collections import Counter
import matplotlib
import matplotlib.pyplot as plt
from matplotlib import font_manager, rc

#파일 불러오기
file = 'Mining.csv'
df = pd.read_csv(file, sep=',',header = 0, 
                 engine = 'python',encoding = "cp949")

#data frame을 언론사별로 분류
df_cho = df[(df['media']=='조선일보')]
df_jung = df[(df['media']=='중앙일보')]
df_dong = df[(df['media']=='동아일보')]
df_kyeong = df[(df['media']=='경향일보')]
df_han = df[(df['media']=='한겨레')]


#4분기 정규식
q1 = re.compile('2020\-12\-19-2021\-03\-18')
q2 = re.compile('2020\-03\-19-2021\-06\-18')
q3 = re.compile('2020\-06\-19-2021\-09\-18')
q4 = re.compile('2020\-09\-19-2021\-12\-18')


#언론사별 전체 키워드 병합용
ms_cho = ''
ms_jung = ''
ms_dong = ''
ms_kyeong = ''
ms_han = ''

#언론사별 전체 키워드
kw_cho = []
kw_jung = []
kw_dong = []
kw_kyeong = []
kw_han = []

media_name = ['조선일보','동아일보', '중앙일보', '한겨레', '경향일보']

#조선일보-----------------------------------------------------------------------------------
for item in df_cho['title']:
    ms_cho = ms_cho + re.sub(r'[^\w]',' ',item)
    
kw_cho = ms_cho.split()
cnt_cho = Counter(kw_cho)

#출현 빈도 상위 30개
sorted_cho = dict()
for tag,counts in cnt_cho.most_common(30):
    sorted_cho[tag] =counts

#조선일보 분기별
ms_cho_q1 = ''
ms_cho_q2 = ''
ms_cho_q3 = ''
ms_cho_q4 = ''
kw_cho_q1 = []
kw_cho_q2 = []
kw_cho_q3 = []
kw_cho_q4 = []

#분기별 분류
for i in df_cho.index:
    t = df_cho['date'][i].split("-")
    if (t[0] == "2020") | (t[1] == "[01-02]"):
        ms_cho_q1 = ms_cho_q1 + re.sub(r'[^\w]',' ',df_cho['title'][i])
    elif t[1] == "03":
        if t[2] == "[01-18]":
            ms_cho_q1 = ms_cho_q1 + re.sub(r'[^\w]',' ',df_cho['title'][i])
        else:
            ms_cho_q2 = ms_cho_q2 + re.sub(r'[^\w]',' ',df_cho['title'][i])    
    elif t[1] == "[04-05]":
        ms_cho_q2 = ms_cho_q2 + re.sub(r'[^\w]',' ',df_cho['title'][i])    
    elif t[1] == "06":
        if t[2] == "[01-18]":
            ms_cho_q2 = ms_cho_q2 + re.sub(r'[^\w]',' ',df_cho['title'][i])
        else:
            ms_cho_q3 = ms_cho_q3 + re.sub(r'[^\w]',' ',df_cho['title'][i])    
    elif t[1] == "[07-08]":
        ms_cho_q3 = ms_cho_q3 + re.sub(r'[^\w]',' ',df_cho['title'][i])    
    elif t[1] == "09":
        if t[2] == "[01-18]":
            ms_cho_q3 = ms_cho_q3 + re.sub(r'[^\w]',' ',df_cho['title'][i])
    else: 
        ms_cho_q4 = ms_cho_q4 + re.sub(r'[^\w]',' ',df_cho['title'][i])    

        
kw_cho_q1 = ms_cho_q1.split()
kw_cho_q2 = ms_cho_q2.split()
kw_cho_q3 = ms_cho_q3.split()
kw_cho_q4 = ms_cho_q4.split()
cnt_cho_q1 = Counter(kw_cho_q1)
cnt_cho_q2 = Counter(kw_cho_q2)
cnt_cho_q3 = Counter(kw_cho_q3)
cnt_cho_q4 = Counter(kw_cho_q4)


#출현 횟수 상위 50개
sorted_cho_q1 = dict()
sorted_cho_q2 = dict()
sorted_cho_q3 = dict()
sorted_cho_q4 = dict()
for tag,counts in cnt_cho_q1.most_common(50):
    sorted_cho_q1[tag] =counts
for tag,counts in cnt_cho_q2.most_common(50):
    sorted_cho_q2[tag] =counts
for tag,counts in cnt_cho_q3.most_common(50):
    sorted_cho_q3[tag] =counts
for tag,counts in cnt_cho_q4.most_common(50):
    sorted_cho_q4[tag] =counts
    
    
#중앙일보-----------------------------------------------------------------------------------
for item in df_jung['title']:
    ms_jung = ms_jung + re.sub(r'[^\w]',' ',item)
kw_jung = ms_jung.split()
cnt_jung = Counter(kw_jung)
#출현 횟수 상위 50개
sorted_jung = dict()
for tag,counts in cnt_jung.most_common(30):
    sorted_jung[tag] =counts
    
#중앙일보 분기별
ms_jung_q1 = ''
ms_jung_q2 = ''
ms_jung_q3 = ''
ms_jung_q4 = ''
kw_jung_q1 = []
kw_jung_q2 = []
kw_jung_q3 = []
kw_jung_q4 = []

#분기별 분류
for i in df_jung.index:
    t = df_jung['date'][i].split("-")
    if (t[0] == "2020") | (t[1] == "[01-02]"):
        ms_jung_q1 = ms_jung_q1 + re.sub(r'[^\w]',' ',df_jung['title'][i])
    elif t[1] == "03":
        if t[2] == "[01-18]":
            ms_jung_q1 = ms_jung_q1 + re.sub(r'[^\w]',' ',df_jung['title'][i])
        else:
            ms_jung_q2 = ms_jung_q2 + re.sub(r'[^\w]',' ',df_jung['title'][i])
    elif t[1] == "[04-05]":
        ms_jung_q2 = ms_jung_q2 + re.sub(r'[^\w]',' ',df_jung['title'][i])
    elif t[1] == "06":
        if t[2] == "[01-18]":
            ms_jung_q2 = ms_jung_q2 + re.sub(r'[^\w]',' ',df_jung['title'][i])
        else:
            ms_jung_q3 = ms_jung_q3 + re.sub(r'[^\w]',' ',df_jung['title'][i]) 
    elif t[1] == "[07-08]":
        ms_jung_q3 = ms_jung_q3 + re.sub(r'[^\w]',' ',df_jung['title'][i]) 
    elif t[1] == "09":
        if t[2] == "[01-18]":
            ms_jung_q3 = ms_jung_q3 + re.sub(r'[^\w]',' ',df_jung['title'][i])
    else: 
        ms_jung_q4 = ms_jung_q4 + re.sub(r'[^\w]',' ',df_jung['title'][i]) 

kw_jung_q1 = ms_jung_q1.split()
kw_jung_q2 = ms_jung_q2.split()
kw_jung_q3 = ms_jung_q3.split()
kw_jung_q4 = ms_jung_q4.split()
cnt_jung_q1 = Counter(kw_jung_q1)
cnt_jung_q2 = Counter(kw_jung_q2)
cnt_jung_q3 = Counter(kw_jung_q3)
cnt_jung_q4 = Counter(kw_jung_q4)


#출현 횟수 상위 50개
sorted_jung_q1 = dict()
sorted_jung_q2 = dict()
sorted_jung_q3 = dict()
sorted_jung_q4 = dict()
for tag,counts in cnt_jung_q1.most_common(50):
    sorted_jung_q1[tag] =counts
for tag,counts in cnt_jung_q2.most_common(50):
    sorted_jung_q2[tag] =counts
for tag,counts in cnt_jung_q3.most_common(50):
    sorted_jung_q3[tag] =counts
for tag,counts in cnt_jung_q4.most_common(50):
    sorted_jung_q4[tag] =counts
    
    
#동아일보-----------------------------------------------------------------------------------
for item in df_dong['title']:
    ms_dong = ms_dong + re.sub(r'[^\w]',' ',item)
kw_dong = ms_dong.split()
cnt_dong = Counter(kw_dong)
#출현 횟수 상위 50개
sorted_dong = dict()
for tag,counts in cnt_dong.most_common(30):
    sorted_dong[tag] =counts
    
    
#동아일보 분기별
ms_dong_q1 = ''
ms_dong_q2 = ''
ms_dong_q3 = ''
ms_dong_q4 = ''
kw_dong_q1 = []
kw_dong_q2 = []
kw_dong_q3 = []
kw_dong_q4 = []

#분기별 분류
for i in df_dong.index:
    t = df_dong['date'][i].split("-")
    if (t[0] == "2020") | (t[1] == "[01-02]"):
        ms_dong_q1 = ms_dong_q1 + re.sub(r'[^\w]',' ',df_dong['title'][i])
    elif t[1] == "03":
        if t[2] == "[01-18]":
            ms_dong_q1 = ms_dong_q1 + re.sub(r'[^\w]',' ',df_dong['title'][i])
        else:
            ms_dong_q2 = ms_dong_q2 + re.sub(r'[^\w]',' ',df_dong['title'][i]) 
    elif t[1] == "[04-05]":
        ms_dong_q1 = ms_dong_q1 + re.sub(r'[^\w]',' ',df_dong['title'][i])
    elif t[1] == "06":
        if t[2] == "[01-18]":
            ms_dong_q2 = ms_dong_q2 + re.sub(r'[^\w]',' ',df_dong['title'][i])
        else:
            ms_dong_q3 = ms_dong_q3 + re.sub(r'[^\w]',' ',df_dong['title'][i]) 
    elif t[1] == "[07-08]":
        ms_dong_q3 = ms_dong_q3 + re.sub(r'[^\w]',' ',df_dong['title'][i]) 
    elif t[1] == "09":
        if t[2] == "[01-18]":
            ms_dong_q3 = ms_dong_q3 + re.sub(r'[^\w]',' ',df_dong['title'][i])
    else:
        ms_dong_q4 = ms_dong_q4 + re.sub(r'[^\w]',' ',df_dong['title'][i]) 

        
kw_dong_q1 = ms_dong_q1.split()
kw_dong_q2 = ms_dong_q2.split()
kw_dong_q3 = ms_dong_q3.split()
kw_dong_q4 = ms_dong_q4.split()
cnt_dong_q1 = Counter(kw_dong_q1)
cnt_dong_q2 = Counter(kw_dong_q2)
cnt_dong_q3 = Counter(kw_dong_q3)
cnt_dong_q4 = Counter(kw_dong_q4)


#출현 횟수 상위 50개
sorted_dong_q1 = dict()
sorted_dong_q2 = dict()
sorted_dong_q3 = dict()
sorted_dong_q4 = dict()
for tag,counts in cnt_dong_q1.most_common(50):
    sorted_dong_q1[tag] =counts
for tag,counts in cnt_dong_q2.most_common(50):
    sorted_dong_q2[tag] =counts
for tag,counts in cnt_dong_q3.most_common(50):
    sorted_dong_q3[tag] =counts
for tag,counts in cnt_dong_q4.most_common(50):
    sorted_dong_q4[tag] =counts
    
#경향일보-----------------------------------------------------------------------------------
for item in df_kyeong['title']:
    ms_kyeong = ms_kyeong + re.sub(r'[^\w]',' ',item)
kw_kyeong = ms_kyeong.split()
cnt_kyeong = Counter(kw_kyeong)
#출현 횟수 상위 50개
sorted_kyeong = dict()
for tag,counts in cnt_kyeong.most_common(30):
    sorted_kyeong[tag] =counts
    
    
#경향일보 분기별
ms_kyeong_q1 = ''
ms_kyeong_q2 = ''
ms_kyeong_q3 = ''
ms_kyeong_q4 = ''
kw_kyeong_q1 = []
kw_kyeong_q2 = []
kw_kyeong_q3 = []
kw_kyeong_q4 = []


#분기별 분류
for i in df_kyeong.index:
    t = df_kyeong['date'][i].split("-")
    if (t[0] == "2020") | (t[1] == "[01-02]"):
        ms_kyeong_q1 = ms_kyeong_q1 + re.sub(r'[^\w]',' ',df_kyeong['title'][i])
    elif t[1] == "03":
        if t[2] == "[01-18]":
            ms_kyeong_q1 = ms_kyeong_q1 + re.sub(r'[^\w]',' ',df_kyeong['title'][i])
        else:
            ms_kyeong_q2 = ms_kyeong_q2 + re.sub(r'[^\w]',' ',df_kyeong['title'][i])
    elif t[1] == "[04-05]":
        ms_kyeong_q2 = ms_kyeong_q2 + re.sub(r'[^\w]',' ',df_kyeong['title'][i])
    elif t[1] == "06":
        if t[2] == "[01-18]":
            ms_kyeong_q2 = ms_kyeong_q2 + re.sub(r'[^\w]',' ',df_kyeong['title'][i])
        else:
            ms_kyeong_q3 = ms_kyeong_q3 + re.sub(r'[^\w]',' ',df_kyeong['title'][i])
    elif t[1] == "[07-08]":
        ms_kyeong_q3 = ms_kyeong_q3 + re.sub(r'[^\w]',' ',df_kyeong['title'][i])
    elif t[1] == "09":
        if t[2] == "[01-18]":
            ms_kyeong_q3 = ms_kyeong_q3 + re.sub(r'[^\w]',' ',df_kyeong['title'][i])
    else:
        ms_kyeong_q4 = ms_kyeong_q4 + re.sub(r'[^\w]',' ',df_kyeong['title'][i])
        
kw_kyeong_q1 = ms_kyeong_q1.split()
kw_kyeong_q2 = ms_kyeong_q2.split()
kw_kyeong_q3 = ms_kyeong_q3.split()
kw_kyeong_q4 = ms_kyeong_q4.split()
cnt_kyeong_q1 = Counter(kw_kyeong_q1)
cnt_kyeong_q2 = Counter(kw_kyeong_q2)
cnt_kyeong_q3 = Counter(kw_kyeong_q3)
cnt_kyeong_q4 = Counter(kw_kyeong_q4)


#출현 횟수 상위 50개
sorted_kyeong_q1 = dict()
sorted_kyeong_q2 = dict()
sorted_kyeong_q3 = dict()
sorted_kyeong_q4 = dict()
for tag,counts in cnt_kyeong_q1.most_common(50):
    sorted_kyeong_q1[tag] =counts
for tag,counts in cnt_kyeong_q2.most_common(50):
    sorted_kyeong_q2[tag] =counts
for tag,counts in cnt_kyeong_q3.most_common(50):
    sorted_kyeong_q3[tag] =counts
for tag,counts in cnt_kyeong_q4.most_common(50):
    sorted_kyeong_q4[tag] =counts
    
#한겨례일보-----------------------------------------------------------------------------------
for item in df_han['title']:
    ms_han = ms_han + re.sub(r'[^\w]',' ',item)
kw_han = ms_han.split()
cnt_han = Counter(kw_han)
#출현 횟수 상위 50개
sorted_han = dict()
for tag,counts in cnt_han.most_common(30):
    sorted_han[tag] =counts
    
    
#한겨례일보 분기별
ms_han_q1 = ''
ms_han_q2 = ''
ms_han_q3 = ''
ms_han_q4 = ''
kw_han_q1 = []
kw_han_q2 = []
kw_han_q3 = []
kw_han_q4 = []

#분기별 분류
for i in df_han.index:
    t = df_han['date'][i].split("-")
    if (t[0] == "2020") | (t[1] == "[01-02]"):
        ms_han_q1 = ms_han_q1 + re.sub(r'[^\w]',' ',df_han['title'][i])
    elif t[1] == "03":
        if t[2] == "[01-18]":
            ms_han_q1 = ms_han_q1 + re.sub(r'[^\w]',' ',df_han['title'][i])
        else:
            ms_han_q2 = ms_han_q2 + re.sub(r'[^\w]',' ',df_han['title'][i])    
    elif t[1] == "[04-05]":
        ms_han_q2 = ms_han_q2 + re.sub(r'[^\w]',' ',df_han['title'][i])
    elif t[1] == "06":
        if t[2] == "[01-18]":
            ms_han_q2 = ms_han_q2 + re.sub(r'[^\w]',' ',df_han['title'][i])
        else:
            ms_han_q3 = ms_han_q3 + re.sub(r'[^\w]',' ',df_han['title'][i])    
    elif t[1] == "[07-08]":
        ms_han_q3 = ms_han_q3 + re.sub(r'[^\w]',' ',df_han['title'][i])    
    elif t[1] == "09":
        if t[2] == "[01-18]":
            ms_han_q3 = ms_han_q3 + re.sub(r'[^\w]',' ',df_han['title'][i])
    else:
        ms_han_q4 = ms_han_q4 + re.sub(r'[^\w]',' ',df_han['title'][i])    
        

kw_han_q1 = ms_han_q1.split()
kw_han_q2 = ms_han_q2.split()
kw_han_q3 = ms_han_q3.split()
kw_han_q4 = ms_han_q4.split()
cnt_han_q1 = Counter(kw_han_q1)
cnt_han_q2 = Counter(kw_han_q2)
cnt_han_q3 = Counter(kw_han_q3)
cnt_han_q4 = Counter(kw_han_q4)


#출현 횟수 상위 50개
sorted_han_q1 = dict()
sorted_han_q2 = dict()
sorted_han_q3 = dict()
sorted_han_q4 = dict()
for tag,counts in cnt_han_q1.most_common(50):
    sorted_han_q1[tag] =counts
for tag,counts in cnt_han_q2.most_common(50):
    sorted_han_q2[tag] =counts
for tag,counts in cnt_han_q3.most_common(50):
    sorted_han_q3[tag] =counts
for tag,counts in cnt_han_q4.most_common(50):
    sorted_han_q4[tag] =counts
    
#그래프 준비-----------------------------------------------------------------------------------
font_path = "c:/Windows/fonts/malgun.ttf"
font_name = font_manager.FontProperties(fname=font_path).get_name()
matplotlib.rc('font',family = font_name)
colors = ['red', 'blue', 'green', 'blueviolet', 'orange']
All_q1 = []
All_q1.append(sorted_cho_q1)
All_q1.append(sorted_jung_q1)
All_q1.append(sorted_dong_q1)
All_q1.append(sorted_han_q1)
All_q1.append(sorted_kyeong_q1)
All_q2 = []
All_q2.append(sorted_cho_q2)
All_q2.append(sorted_jung_q2)
All_q2.append(sorted_dong_q2)
All_q2.append(sorted_han_q2)
All_q2.append(sorted_kyeong_q2)
All_q3 = []
All_q3.append(sorted_cho_q3)
All_q3.append(sorted_jung_q3)
All_q3.append(sorted_dong_q3)
All_q3.append(sorted_han_q3)
All_q3.append(sorted_kyeong_q3)
All_q4 = []
All_q4.append(sorted_cho_q4)
All_q4.append(sorted_jung_q4)
All_q4.append(sorted_dong_q4)
All_q4.append(sorted_han_q4)
All_q4.append(sorted_kyeong_q4)
All_Year = []
All_Year.append(sorted_cho)
All_Year.append(sorted_jung)
All_Year.append(sorted_dong)
All_Year.append(sorted_han)
All_Year.append(sorted_kyeong)


# In[102]:



for index in range(5):
    plt.figure(figsize = (12,5))
    plt_title = "1분기 "+media_name[index]+" 뉴스 제목 키워드 상위 50개"
    plt.title(plt_title,fontsize=15)
    plt.xlabel('키워드')
    plt.ylabel('빈도수')
    plt.grid(True)
    sorted_Keys = sorted(All_q1[index], key=All_q1[index].get,reverse=True)
    sorted_Values = sorted(All_q1[index].values(), reverse=True)
    plt.bar(range(len(All_q1[index])), sorted_Values,align = 'center', color=colors[index])
    plt.xticks(range(len(All_q1[index])),list(sorted_Keys),rotation='75')
    
    plt.show()


# In[103]:



for index in range(5):
    plt.figure(figsize = (12,5))
    plt_title = "2분기 "+media_name[index]+" 뉴스 제목 키워드 상위 50개"
    plt.title(plt_title,fontsize=15)
    plt.xlabel('키워드')
    plt.ylabel('빈도수')
    plt.grid(True)
    sorted_Keys = sorted(All_q2[index], key=All_q2[index].get,reverse=True)
    sorted_Values = sorted(All_q2[index].values(), reverse=True)
    plt.bar(range(len(All_q2[index])), sorted_Values,align = 'center', color=colors[index])
    plt.xticks(range(len(All_q2[index])),list(sorted_Keys),rotation='75')
    
    plt.show()

# In[106]:



for index in range(5):
    plt.figure(figsize = (12,5))
    plt_title = "3분기 "+media_name[index]+" 뉴스 제목 키워드 상위 50개"
    plt.title(plt_title,fontsize=15)
    plt.xlabel('키워드')
    plt.ylabel('빈도수')
    plt.grid(True)
    sorted_Keys = sorted(All_q3[index], key=All_q3[index].get,reverse=True)
    sorted_Values = sorted(All_q3[index].values(), reverse=True)
    plt.bar(range(len(All_q3[index])), sorted_Values,align = 'center', color=colors[index])
    plt.xticks(range(len(All_q3[index])),list(sorted_Keys),rotation='75')
    
    plt.show()

# In[107]:



for index in range(5):
    plt.figure(figsize = (12,5))
    plt_title = "4분기 "+media_name[index]+" 뉴스 제목 키워드 상위 50개"
    plt.title(plt_title,fontsize=15)
    plt.xlabel('키워드')
    plt.ylabel('빈도수')
    plt.grid(True)
    sorted_Keys = sorted(All_q4[index], key=All_q4[index].get,reverse=True)
    sorted_Values = sorted(All_q4[index].values(), reverse=True)
    plt.bar(range(len(All_q4[index])), sorted_Values,align = 'center', color=colors[index])
    plt.xticks(range(len(All_q4[index])),list(sorted_Keys),rotation='75')
    
    plt.show()

# In[109]:



for index in range(5):
    plt.figure(figsize = (12,5))
    plt_title = "최근1년 "+media_name[index]+" 뉴스 제목 키워드 상위 30개"
    plt.title(plt_title,fontsize=15)
    plt.xlabel('키워드')
    plt.ylabel('빈도수')
    plt.grid(True)
    sorted_Keys = sorted(All_Year[index], key=All_Year[index].get,reverse=True)
    sorted_Values = sorted(All_Year[index].values(), reverse=True)
    plt.bar(range(len(All_Year[index])), sorted_Values,align = 'center', color=colors[index])
    plt.xticks(range(len(All_Year[index])),list(sorted_Keys),rotation='75')
    
    plt.show()
