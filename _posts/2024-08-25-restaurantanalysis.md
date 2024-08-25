---
layout: single
title: "Project 4 - 시카고 맛집 데이터 분석"
categories: Data Analysis
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## 시카고 맛집 데이터 분석
### 1. 개요
- https://www.chicagomag.com/chicago-magazine/november-2012/best-sandwiches-chicago/
- chicago magazine the 50 best sandwiches

#### 목표
50개 페이지에서 각 가게의 정보 가져오기
- 가게이름
- 대표메뉴
- 대표메뉴의 가격
- 가게주소

### 2. 메인페이지
```py
from urllib.request import Request, urlopen
from fake_useragent import UserAgent
from bs4 import BeautifulSoup
import ssl

context = ssl._create_unverified_context()

url_base = "https://www.chicagomag.com/"
url_sub = "chicago-magazine/november-2012/best-sandwiches-chicago/"
url = url_base + url_sub

ua = UserAgent()
req = Request(url, headers={"User-Agent": ua.ie})
html = urlopen(req, context=context)
soup = BeautifulSoup(html, "html.parser")
print(soup.prettify())

=>

<!DOCTYPE html>
<html lang="en-US">
 <head>
  <meta charset="utf-8"/>
  <meta content="IE=edge" http-equiv="X-UA-Compatible">
   <link href="https://gmpg.org/xfn/11" rel="profile"/>
   <script src="https://cmp.osano.com/16A1AnRt2Fn8i1unj/f15ebf08-7008-40fe-
...
  </script>
 </body>
</html>
```
```py
soup.find_all("div", class_="sammy"), len(soup.find_all("div", class_="sammy"))
# soup.select(".sammy"), len(soup.select(".sammy"))

=>

([<div class="sammy" style="position: relative;">
  <div class="sammyRank">1</div>
  <div class="sammyListing"><a href="/Chicago-Magazine/November-2012/Best-Sandwiches-in-Chicago-Old-Oak-Tap-BLT/"><b>BLT</b><br/>
  Old Oak Tap<br/>
  <em>Read more</em> </a></div>
  </div>,
  <div class="sammy" style="position: relative;">
  <div class="sammyRank">2</div>
  <div class="sammyListing"><a href="/Chicago-Magazine/November-2012/Best-
...
  <div class="sammyListing"><a href="https://www.chicagomag.com/Chicago-Magazine/November-2012/Best-Sandwiches-in-Chicago-Phoebes-Bakery-The-Gatsby/"><b>The Gatsby</b><br/>
  Phoebe’s Bakery<br/>
  <em>Read more</em> </a></div>
  </div>],
 50)
```
```py
tmp_one = soup.find_all("div", "sammy")[0]
type(tmp_one)

=>

bs4.element.Tag
```
```py
tmp_one.find(class_="sammyRank").get_text()
# tmp_one.select_one(".sammyRank").text

=>

'1'
```
```py
tmp_one.find("div", {"class":"sammyListing"}).text
# tmp_one.select_one(".sammyListing").get_text()

=>

'BLT\nOld Oak Tap\nRead more '
```
```py
tmp_one.find("a")["href"]
# tmp_one.select_one("a").get("href")

=>

'/Chicago-Magazine/November-2012/Best-Sandwiches-in-Chicago-Old-Oak-Tap-BLT/'
```
```py
import re

tmp_string = tmp_one.find(class_="sammyListing").get_text()
re.split(("\n|\r\n"), tmp_string)

=>

['BLT', 'Old Oak Tap', 'Read more ']
```
```py
print(re.split(("\n|\r\n"), tmp_string)[0]) # menu
print(re.split(("\n|\r\n"), tmp_string)[1]) # cafe

=>

BLT
Old Oak Tap
```
#### DataFrame
```py
from urllib.parse import urljoin

url_base = "http://www.chicagomag.com"

# 필요한 내용을 담을 빈 리스트
# 리스트로 하나씩 컬럼을 만들고, DataFrame으로 합칠 예정
rank = []
main_menu = []
cafe_name = []
url_add = []

list_soup = soup.find_all("div", "sammy") # soup.select(".sammy")

for item in list_soup:
    rank.append(item.find(class_="sammyRank").get_text())
    tmp_string = item.find(class_="sammyListing").get_text()
    main_menu.append(re.split(("\n|\r\n"), tmp_string)[0])
    cafe_name.append(re.split(("\n|\r\n"), tmp_string)[1])
    url_add.append(urljoin(url_base, item.find("a")["href"]))
    
import pandas as pd

data = {
    "Rank": rank,
    "Menu": main_menu,
    "Cafe": cafe_name,
    "URL": url_add,
}

df = pd.DataFrame(data)
df.tail(2)
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Rank</th>
      <th>Menu</th>
      <th>Cafe</th>
      <th>URL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>48</th>
      <td>49</td>
      <td>Le Végétarien</td>
      <td>Toni Patisserie</td>
      <td>https://www.chicagomag.com/Chicago-Magazine/No...</td>
    </tr>
    <tr>
      <th>49</th>
      <td>50</td>
      <td>The Gatsby</td>
      <td>Phoebe’s Bakery</td>
      <td>https://www.chicagomag.com/Chicago-Magazine/No...</td>
    </tr>
  </tbody>
</table>

---

#### 컬럼 순서 변경
```py
df = pd.DataFrame(data, columns=["Rank", "Cafe", "Menu", "URL"])
df.tail()
```
<div>
<style scoped="">
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Rank</th>
      <th>Cafe</th>
      <th>Menu</th>
      <th>URL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>45</th>
      <td>46</td>
      <td>Chickpea</td>
      <td>Kufta</td>
      <td>https://www.chicagomag.com/Chicago-Magazine/No...</td>
    </tr>
    <tr>
      <th>46</th>
      <td>47</td>
      <td>The Goddess and Grocer</td>
      <td>Debbie’s Egg Salad</td>
      <td>https://www.chicagomag.com/Chicago-Magazine/No...</td>
    </tr>
    <tr>
      <th>47</th>
      <td>48</td>
      <td>Zenwich</td>
      <td>Beef Curry</td>
      <td>https://www.chicagomag.com/Chicago-Magazine/No...</td>
    </tr>
    <tr>
      <th>48</th>
      <td>49</td>
      <td>Toni Patisserie</td>
      <td>Le Végétarien</td>
      <td>https://www.chicagomag.com/Chicago-Magazine/No...</td>
    </tr>
    <tr>
      <th>49</th>
      <td>50</td>
      <td>Phoebe’s Bakery</td>
      <td>The Gatsby</td>
      <td>https://www.chicagomag.com/Chicago-Magazine/No...</td>
    </tr>
  </tbody>
</table>
</div>

---

#### 데이터 저장
```py
df.to_csv(
    "../data/03. best_sandwiches_list_chicago.csv", sep=",", encoding="utf-8"
)
```
---

### 3. 하위페이지

#### requirements
```py
import pandas as pd
from urllib.request import urlopen, Request
from fake_useragent import UserAgent
from bs4 import BeautifulSoup
```

---

```py
df = pd.read_csv("../data/03. best_sandwiches_list_chicago.csv", index_col=0)
df.tail()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Rank</th>
      <th>Cafe</th>
      <th>Menu</th>
      <th>URL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>45</th>
      <td>46</td>
      <td>Chickpea</td>
      <td>Kufta</td>
      <td>https://www.chicagomag.com/Chicago-Magazine/No...</td>
    </tr>
    <tr>
      <th>46</th>
      <td>47</td>
      <td>The Goddess and Grocer</td>
      <td>Debbie’s Egg Salad</td>
      <td>https://www.chicagomag.com/Chicago-Magazine/No...</td>
    </tr>
    <tr>
      <th>47</th>
      <td>48</td>
      <td>Zenwich</td>
      <td>Beef Curry</td>
      <td>https://www.chicagomag.com/Chicago-Magazine/No...</td>
    </tr>
    <tr>
      <th>48</th>
      <td>49</td>
      <td>Toni Patisserie</td>
      <td>Le Végétarien</td>
      <td>https://www.chicagomag.com/Chicago-Magazine/No...</td>
    </tr>
    <tr>
      <th>49</th>
      <td>50</td>
      <td>Phoebe’s Bakery</td>
      <td>The Gatsby</td>
      <td>https://www.chicagomag.com/Chicago-Magazine/No...</td>
    </tr>
  </tbody>
</table>

---

```py
df["URL"][0]

=>

'http://www.chicagomag.com/Chicago-Magazine/November-2012/Best-Sandwiches-in-Chicago-Old-Oak-Tap-BLT/'

```

---

```py
req = Request(df["URL"][0], headers={"user-agent":ua.ie})
html = urlopen(req, context=context).read()
soup_tmp = BeautifulSoup(html, "html.parser")
soup_tmp.find("p", "addy") # soup_find.select_one(".addy")

=>

<p class="addy">
<em>$10. 2109 W. Chicago Ave., 773-772-0406, <a href="http://www.theoldoaktap.com/">theoldoaktap.com</a></em></p>
```

---

#### regular expression
```py
price_tmp = soup_tmp.find("p", "addy").text
price_tmp

=>

'\n$10. 2109 W. Chicago Ave., 773-772-0406, theoldoaktap.com'
```

---

```py
import re
re.split(".,", price_tmp)

=>

['\n$10. 2109 W. Chicago Ave', ' 773-772-040', ' theoldoaktap.com']
```

---

```py
price_tmp = re.split(".,", price_tmp)[0]
price_tmp

=>

'\n$10. 2109 W. Chicago Ave'
```

---

```py
tmp = re.search("\$\d+\.(\d+)?", price_tmp).group()
price_tmp[len(tmp) + 2:]

=>

'2109 W. Chicago Ave'
```

---

#### 가격, 주소 추가
```py
from tqdm import tqdm

price = []
address = []

for idx, row in tqdm(df.iterrows()):
    req = Request(row["URL"], headers={"user-agent":ua.ie})
    html = urlopen(req, context=context).read()
    soup_tmp = BeautifulSoup(html, "lxml")
    
    gettings = soup_tmp.find("p", "addy").get_text()
    price_tmp = re.split(".,", gettings)[0]
    tmp = re.search("\$\d+\.(\d+)?", price_tmp).group()

    price.append(tmp)
    address.append(price_tmp[len(tmp) + 2:])
```

---

```py
df["Price"] = price
df["Address"] = address
df = df.loc[:, ["Rank", "Cafe", "Menu", "Price", "Address"]]
df.set_index("Rank", inplace=True)
df.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Cafe</th>
      <th>Menu</th>
      <th>Price</th>
      <th>Address</th>
    </tr>
    <tr>
      <th>Rank</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Old Oak Tap</td>
      <td>BLT</td>
      <td>$10.</td>
      <td>2109 W. Chicago Ave</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Au Cheval</td>
      <td>Fried Bologna</td>
      <td>$9.</td>
      <td>800 W. Randolph St</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Xoco</td>
      <td>Woodland Mushroom</td>
      <td>$9.50</td>
      <td>445 N. Clark St</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Al’s Deli</td>
      <td>Roast Beef</td>
      <td>$9.40</td>
      <td>914 Noyes St</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Publican Quality Meats</td>
      <td>PB&amp;L</td>
      <td>$10.</td>
      <td>825 W. Fulton Mkt</td>
    </tr>
  </tbody>
</table>

---

## 지도 시각화

#### requirements
```py
import folium
import pandas as pd
import numpy as np
import googlemaps
from tqdm import tqdm
```

---

```py
gmaps_key = ""
gmaps = googlemaps.Client(key=gmaps_key)

lat = []
lng = []

for idx, row in tqdm(df.iterrows()):
    if not row["Address"] == "Multiple location":
        target_name = row["Address"] + ", " + "Chicago"
        # print(target_name)
        gmaps_output = gmaps.geocode(target_name)
        location_output = gmaps_output[0].get("geometry")
        lat.append(location_output["location"]["lat"])
        lng.append(location_output["location"]["lng"])
        # location_output = gmaps_output[0]
    else:
        lat.append(np.nan)
        lng.append(np.nan)
        
df.tail()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Cafe</th>
      <th>Menu</th>
      <th>Price</th>
      <th>Address</th>
    </tr>
    <tr>
      <th>Rank</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>46</th>
      <td>Chickpea</td>
      <td>Kufta</td>
      <td>$8.</td>
      <td>2018 W. Chicago Ave</td>
    </tr>
    <tr>
      <th>47</th>
      <td>The Goddess and Grocer</td>
      <td>Debbie’s Egg Salad</td>
      <td>$6.50</td>
      <td>25 E. Delaware Pl</td>
    </tr>
    <tr>
      <th>48</th>
      <td>Zenwich</td>
      <td>Beef Curry</td>
      <td>$7.50</td>
      <td>416 N. York St</td>
    </tr>
    <tr>
      <th>49</th>
      <td>Toni Patisserie</td>
      <td>Le Végétarien</td>
      <td>$8.75</td>
      <td>65 E. Washington St</td>
    </tr>
    <tr>
      <th>50</th>
      <td>Phoebe’s Bakery</td>
      <td>The Gatsby</td>
      <td>$6.85</td>
      <td>3351 N. Broadwa</td>
    </tr>
  </tbody>
</table>

---

```py
df["lat"] = lat
df["lng"] = lng
df.tail()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Cafe</th>
      <th>Menu</th>
      <th>Price</th>
      <th>Address</th>
      <th>lat</th>
      <th>lng</th>
    </tr>
    <tr>
      <th>Rank</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>46</th>
      <td>Chickpea</td>
      <td>Kufta</td>
      <td>$8.</td>
      <td>2018 W. Chicago Ave</td>
      <td>41.896113</td>
      <td>-87.677857</td>
    </tr>
    <tr>
      <th>47</th>
      <td>The Goddess and Grocer</td>
      <td>Debbie’s Egg Salad</td>
      <td>$6.50</td>
      <td>25 E. Delaware Pl</td>
      <td>41.898979</td>
      <td>-87.627393</td>
    </tr>
    <tr>
      <th>48</th>
      <td>Zenwich</td>
      <td>Beef Curry</td>
      <td>$7.50</td>
      <td>416 N. York St</td>
      <td>41.910583</td>
      <td>-87.940488</td>
    </tr>
    <tr>
      <th>49</th>
      <td>Toni Patisserie</td>
      <td>Le Végétarien</td>
      <td>$8.75</td>
      <td>65 E. Washington St</td>
      <td>41.883106</td>
      <td>-87.625438</td>
    </tr>
    <tr>
      <th>50</th>
      <td>Phoebe’s Bakery</td>
      <td>The Gatsby</td>
      <td>$6.85</td>
      <td>3351 N. Broadwa</td>
      <td>41.943163</td>
      <td>-87.644507</td>
    </tr>
  </tbody>
</table>

---

```py
mapping = folium.Map(location=[41.8781136, -87.6297982], zoom_start=11)

for idx, row in df.iterrows():
    if not row["Address"] == "Multiple location":
        folium.Marker(
            location=[row["lat"], row["lng"]],
            popup=row["Cafe"],
            tooltip=row["Menu"],
            icon=folium.Icon(
                icon="coffee",
                prefix="fa"
            )
        ).add_to(mapping)

mapping
```
![](https://velog.velcdn.com/images/yy2hi/post/332208ca-53bb-4955-9e4a-8fa0b9c56f07/image.png)
