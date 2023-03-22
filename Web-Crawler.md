---
title: "Web Crawler"
author: "Alvin, Lin"
date: "2023-03-21"
date-format: full
format:
   html:
     code-fold: show
     theme: flatly
     embed-resources: true
toc: true
toc-depth: 3
toc-location: left
execute:
  warning: false 
  keep-md: true
---


::: {.cell}

:::


# Python Web Crawler

::: {.cell .column-page}

```{.python .cell-code}
# Get url from PTT movies
import urllib.request as req
url = "https://www.ptt.cc/bbs/movie/index.html"
# Request with headers information
request = req.Request(url, headers = {
  # Headers should be a dictionary
  "User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/110.0"
})
with req.urlopen(request) as response:
  data = response.read().decode("utf-8")
```
:::

::: {.cell .column-page}

```{.python .cell-code}
# header
import bs4
root = bs4.BeautifulSoup(data,"html.parser")
titles = root.find_all("div", class_= "title")
for title in titles:
  if title.a != None:
    print(title.a.string)
```

::: {.cell-output .cell-output-stdout}
```
[新聞] 沙贊戲外槓上黑亞當 證實巨石不讓他客串
Re: [討論] 進擊的鼓手-嚴厲的老師是錯的嗎? (雷)
[新聞] 電視插播致中國5000人遭非法逮捕 紀錄片還原真相
Re: [討論] 進擊的鼓手-嚴厲的老師是錯的嗎? (雷)
[情報] 《戰爭機器》改編電影編劇確定
[公告] 電影板板規 2022/12/5
```
:::
:::


## Cookies

::: {.cell .column-page}

```{.python .cell-code}
# Get url from PTT Gossiping
import urllib.request as req
def getData(url):

    # Request with headers information
    request = req.Request(url, headers = {
      # Headers should be a dictionary
      ## Add cookie in the header
      "cookie":"over18=1",
      "User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/110.0"
    })
    with req.urlopen(request) as response:
      data = response.read().decode("utf-8")
    
    import bs4
    root = bs4.BeautifulSoup(data,"html.parser")
    titles = root.find_all("div", class_= "title")
    for title in titles:
      if title.a != None:
        print(title.a.string)
        
    nextLink = root.find("a", string = "‹ 上頁")
    return nextLink["href"]
```
:::

::: {.cell .column-page}

```{.python .cell-code}
pageURL = "https://www.ptt.cc/bbs/Gossiping/index.html"
count = 0
while count < 3:
  pageURL = "https://www.ptt.cc" + getData(pageURL)
  count +=1
```

::: {.cell-output .cell-output-stdout}
```
[問卦] 慟 哇操 我把同事的髮卷捏碎了 ==
Re: [爆卦] 被民眾抓包不開單 台南警：違停有惹到你嗎
[新聞] 猴痘疫苗今開打！本土已爆10例「分布8縣
Re: [新聞] 關稅、運費農委會全吸收 陳吉仲：已
[問卦] 什麼時候才吃的到茶葉蛋
[新聞] 確保不缺電 總統：已做好三大努力
[問卦] 電子菸不能抽要用什麼替代
Re: [問卦] 人類是怎麼確定同性戀不是疾病？
[公告] 八卦板板規(2023.03.01)
[協尋] 3/3 16：50-17：20大寮區新厝路和新三
[協尋] 台2濱海事故
[協尋] 3/15 7:44高雄大寮行車記錄器(更新地點)
[公告] 代PO 政黑進板圖徵選 5/23截止(每推100P
[問卦] 萊爾富的三角飯糰是不是超商中最好吃的?
[問卦] 遇到蜥蜴怎麼抓
[新聞] 侯友宜民調滑鐵盧　前宋楚瑜文膽曝關鍵
Re: [新聞] 悚！桃園男子深夜鬧區墜樓屍首分離 頭顱
[問卦] 學日語的人會變多嗎？
[問卦] 為何警察抓賣春不抓違停
Re: [新聞] 名嘴批能源政策錯誤 台電：核能大國電價
[問卦] 麥當勞新的1+1主打省錢？
[問卦] 剛學會轉移大法 要轉移什麼
[問卦] 小智一行人會有事嗎？
Re: [新聞] 被徵召選2024「義無反顧」？侯友宜：
Re: [問卦] 俄羅斯被西方制裁沒什麼用?
[新聞] 關稅、運費農委會全吸收 陳吉仲：已有2家
[新聞] 【富尪神隱逼搬家】昔為愛淡出演藝圈！
[問卦] 迪士尼跟摩天輪是怎麼樣的構想
Re: [爆卦] 大谷翔平17歲預言自己27歲拿下WBC MVP
[問卦] 戰女f*f跟邦邦p*p  哪團比較慘？？
Re: [新聞] 進口雞蛋再擴大！陳吉仲：4、5月各進口30
[公告] 八卦板板規(2023.03.01)
[協尋] 3/3 16：50-17：20大寮區新厝路和新三
[協尋] 台2濱海事故
[協尋] 3/15 7:44高雄大寮行車記錄器(更新地點)
[公告] 代PO 政黑進板圖徵選 5/23截止(每推100P
[問卦] 現在公務員很會唬爛很會包裝？
Re: [問卦] 驚！不婚不生老了會很慘？
[新聞] 惡師版《黑暗榮耀》！6師逼死高中生　搜
Re: [新聞] 高虹安查棒球場 挖「上級施壓」趕工開幕
[問卦] 目前日本社會對日本隊贏球的反應是如何
Re: [問卦] 如果台海開戰 鄉民真的不想上戰場嗎
Re: [新聞] 學創人員以有男老師為由禁學生穿緊身褲 
[問卦] 阿姨的VR很有看頭
Re: [新聞] 就是拜登下令的! 美國名記者再爆料 炸北
[問卦] 彰化市有啥好吃的 在線等
[問卦] 酒店都是像華燈初上演的那樣嗎？
[新聞] 調查稱「中國人幸福感最高」 被嘲「不敢不
[問卦] 轉帳一天限3萬 為何不像信用卡給額度?
[問卦] 半躺平選路易莎/星巴克打工?
[問卦] 大谷翔平能夠完美的演出楊過這個腳色嗎?
Re: [問卦] 如果台海開戰 鄉民真的不想上戰場嗎
[問卦] 迴轉壽司完全沒有位子？
[問卦] 棒球是不是很無聊？
[新聞] 連綠粉都倒戈！盧秀燕「神級民調」創94％
[問卦] 不接受徵招才關幾年划算吧？
```
:::
:::


## AJAX(Asynchronous JavaScript and XML) (XHR)

::: {.cell .column-page}

```{.python .cell-code}
# Get url from PTT movies
import urllib.request as req
url = "https://medium.com/_/api/home-feed"
# Request with headers information
request = req.Request(url, headers = {
  # Headers should be a dictionary
  "User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/110.0"
})
with req.urlopen(request) as response:
  data = response.read().decode("utf-8")
```
:::

::: {.cell .column-page}

```{.python .cell-code}
# Json Format
import json
data = data.replace("])}while(1);</x>","")
data = json.loads(data) 

posts = data["payload"]["references"]["Post"]
for key in posts:
  post = posts[key]
  print(post["title"])
```

::: {.cell-output .cell-output-stdout}
```
What Will Humans Do In An Artificially Intelligent World?
Battle of the iPhone contenders: S23 Ultra vs Pixel 7 Pro
Trickle-Down Apple Silicon
Maybe Zoom Parties Weren’t So Bad
Composability in LLM Apps
Could A.I. Art Be The Future Of Book Covers?
It’s Always Convenient to Tell Writers They Aren’t Good Enough
10 Trends That Will Shape the Online Writing Industry in 2023
UX lessons from a pottery professor about lean product design
Why Holding onto Failed Artwork May be Holding You Back
Fundraising Too Early Is the Worst Decision You’ll Make as a Startup Founder
How can we help?
The Ultimate Guide to Y-Combinator Applications
Business Strategy on a Page
The Rise and Fall of the Dot-Com Foosball Table
New Expressions of Selfhood Favour Growth-Based Futures
Accepting My Ugliness
Bethany Mandel, Wokeness, Hard Core Lies and Perverted Right Wing Politics!
My Travels With Bob
Why Does a Black Woman's Freedom Infuriate So Many People
Why Meta’s embrace of a ‘flat’ management structure may not lead to the innovation Zuckerberg hopes…
4D Chess in Card Payments
What It Means to “Trust the System”
SVB’s investors will get $2b in public bailout money
Who Will Make Money from the Generative AI Gold Rush? Part I
Run to reconnect with your body
My doctor says they have to lower my long-term opioid medicine. “What can I do? What do I say?”
IVF Failed Me. The System Shouldn’t Have, Too.
Transform Your Life: How I Corrected My Posture and Fixed My Neck Hump in One Year — A Personal…
This sweetener has an unexpected impact on immunity
How To Keep Productivity And Motivation During The War?
Note to Self: Perfectionism and the Right to Rest
Why and How to Get More Skillful at Office Politics
Doing Cal Newport’s Digital Declutter Was Like Finding Gold Under My House
Back to Basics, Part Tres: Logistic Regression
Employee Expectations Are Changing, and Leaders Must Listen
Your Employer Should Be Funding Your Commute
Camille Perri: How I Paid the Bills While I Wrote the Book
Xhenet Aliu: How I Paid the Bills While I Wrote the Book
The Moral Case for Universal Basic Income
Why Basic Jobs Are Better Than Basic Incomes
Let’s Talk About How to Create a Content Strategy
Let’s Talk About SWOT Analysis
How the Internet Made Us Believe in a Flat Earth
The Newest Face of Diet Culture Is the Instagram Butt
How Retail Companies Fail at Checkout
Healthcare Design Is About More Than Aesthetics
```
:::
:::


## Request Data(Request Payload Request Body)

::: {.cell .column-page}

```{.python .cell-code}
# Get url from Medium
import urllib.request as req
import json
url = "https://medium.com/_/graphql"
# Request with headers information
requestData = {"operationName":"CuratedHomeFeedModuleQuery","variables":{"paging":{"from":"10","limit":10}},"query":"query CuratedHomeFeedModuleQuery($paging: PagingOptions!) {\n  staffPicksFeed(input: {paging: $paging}) {\n    items {\n      ...CuratedHomeFeedItems_homeFeedItems\n      __typename\n    }\n    pagingInfo {\n      next {\n        page\n        limit\n        from\n        __typename\n      }\n      __typename\n    }\n    __typename\n  }\n}\n\nfragment CuratedHomeFeedItems_homeFeedItems on HomeFeedItem {\n  __typename\n  post {\n    id\n    title\n    ...HomeFeedItem_post\n    __typename\n  }\n}\n\nfragment HomeFeedItem_post on Post {\n  __typename\n  id\n  title\n  firstPublishedAt\n  mediumUrl\n  collection {\n    id\n    name\n    domain\n    logo {\n      id\n      __typename\n    }\n    __typename\n  }\n  creator {\n    id\n    name\n    username\n    imageId\n    mediumMemberAt\n    __typename\n  }\n  previewImage {\n    id\n    __typename\n  }\n  previewContent {\n    subtitle\n    __typename\n  }\n  readingTime\n  tags {\n    ...TopicPill_tag\n    __typename\n  }\n  ...BookmarkButton_post\n  ...OverflowMenuButtonWithNegativeSignal_post\n  ...PostPresentationTracker_post\n  ...PostPreviewAvatar_post\n  ...Star_post\n}\n\nfragment TopicPill_tag on Tag {\n  __typename\n  id\n  displayTitle\n  normalizedTagSlug\n}\n\nfragment BookmarkButton_post on Post {\n  visibility\n  ...SusiClickable_post\n  ...AddToCatalogBookmarkButton_post\n  __typename\n  id\n}\n\nfragment SusiClickable_post on Post {\n  id\n  mediumUrl\n  ...SusiContainer_post\n  __typename\n}\n\nfragment SusiContainer_post on Post {\n  id\n  __typename\n}\n\nfragment AddToCatalogBookmarkButton_post on Post {\n  ...AddToCatalogBase_post\n  __typename\n  id\n}\n\nfragment AddToCatalogBase_post on Post {\n  id\n  __typename\n}\n\nfragment OverflowMenuButtonWithNegativeSignal_post on Post {\n  id\n  ...OverflowMenuWithNegativeSignal_post\n  __typename\n}\n\nfragment OverflowMenuWithNegativeSignal_post on Post {\n  id\n  creator {\n    id\n    __typename\n  }\n  collection {\n    id\n    __typename\n  }\n  ...OverflowMenuItemUndoClaps_post\n  __typename\n}\n\nfragment OverflowMenuItemUndoClaps_post on Post {\n  id\n  clapCount\n  ...ClapMutation_post\n  __typename\n}\n\nfragment ClapMutation_post on Post {\n  __typename\n  id\n  clapCount\n  ...MultiVoteCount_post\n}\n\nfragment MultiVoteCount_post on Post {\n  id\n  ...PostVotersNetwork_post\n  __typename\n}\n\nfragment PostVotersNetwork_post on Post {\n  id\n  voterCount\n  recommenders {\n    name\n    __typename\n  }\n  __typename\n}\n\nfragment PostPresentationTracker_post on Post {\n  id\n  visibility\n  previewContent {\n    isFullContent\n    __typename\n  }\n  collection {\n    id\n    slug\n    __typename\n  }\n  __typename\n}\n\nfragment PostPreviewAvatar_post on Post {\n  __typename\n  id\n  collection {\n    id\n    name\n    ...CollectionAvatar_collection\n    __typename\n  }\n  creator {\n    id\n    username\n    name\n    ...UserAvatar_user\n    ...userUrl_user\n    ...useIsVerifiedBookAuthor_user\n    __typename\n  }\n}\n\nfragment CollectionAvatar_collection on Collection {\n  name\n  avatar {\n    id\n    __typename\n  }\n  ...collectionUrl_collection\n  __typename\n  id\n}\n\nfragment collectionUrl_collection on Collection {\n  id\n  domain\n  slug\n  __typename\n}\n\nfragment UserAvatar_user on User {\n  __typename\n  id\n  imageId\n  mediumMemberAt\n  name\n  username\n  ...userUrl_user\n}\n\nfragment userUrl_user on User {\n  __typename\n  id\n  customDomainState {\n    live {\n      domain\n      __typename\n    }\n    __typename\n  }\n  hasSubdomain\n  username\n}\n\nfragment useIsVerifiedBookAuthor_user on User {\n  verifications {\n    isBookAuthor\n    __typename\n  }\n  __typename\n  id\n}\n\nfragment Star_post on Post {\n  id\n  creator {\n    id\n    __typename\n  }\n  __typename\n}\n"}
request = req.Request(url, 
                      headers = {
                          "Content-Type":"application/json",
                        # Headers should be a dictionary
                          "User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/110.0"},
                        # request Data
                     data = json.dumps(requestData).encode("utf-8"))
with req.urlopen(request) as response:
  result = response.read().decode("utf-8")
  # String to Dictionary with json loads
result = json.loads(result)
# List contains all title
items = result["data"]["staffPicksFeed"]["items"]
for item in items:
  print(item["post"]["title"])
# print(result["data"]["staffPicksFeed"]["items"][0]["post"]["title"])
```

::: {.cell-output .cell-output-stdout}
```
Note to Self: Perfectionism and the Right to Rest
Five Steps For How I Build Brands
The Curse of Perfect
A Simple Strategy for Boosting Happiness and Health: Spend Time in Nature
What’s Going On? The Silicon Valley Bank Collapse vs. the 2008 Financial Crisis
What We’re Reading: We asked for your stories and you delivered
College Basketball’s NET Rankings Explained
Discovering Creativity: On Your Unique Blob and the Threat of Normalcy
No Atheists In Foxholes. Or Libertarians In Bank Runs
You Don’t Have to Diet Until You Die (or, Some Thoughts on Weight Watchers)
```
:::
:::


## Selenium

::: {.cell .column-page}

```{.python .cell-code}
# import Selenium module
from selenium import webdriver
from msedge.selenium_tools import Edge
# Path to Driver
driver = Edge(executable_path = "msedgedriver.exe")
# Maximize window
driver.maximize_window()
# Connect to Google
driver.get("https://www.google.com/")
driver.save_screenshot("screenshot.png")
driver.close()
```
:::


## PTT Stock

::: {.cell .column-page}

```{.python .cell-code}
from selenium import webdriver
from selenium.webdriver.common.by import By
from msedge.selenium_tools import Edge
# Path to Driver
driver = Edge(executable_path = "msedgedriver.exe")
# Connect PTT Stock
driver.get("https://www.ptt.cc/bbs/Stock/index.html")
# Html page source
# print(driver.page_source)
# Class Attribute
tags = driver.find_elements(By.CLASS_NAME,"title")
for tag in tags:
  print(tag.text)
# Get the previous titles
link = driver.find_element(By.LINK_TEXT,"‹ 上頁")
# Simulate clicking
link.click()
tags = driver.find_elements(By.CLASS_NAME,"title")
for tag in tags:
  print(tag.text)
driver.close()
```
:::


## LinkedIn

::: {.cell .column-page}

```{.python .cell-code}
from selenium import webdriver
from selenium.webdriver.common.by import By
from msedge.selenium_tools import Edge
import time
# Path to Driver
driver = Edge(executable_path = "msedgedriver.exe")
# Connect to LinkedIn
driver.get("https://www.linkedin.com/jobs/search?src=go-pa&trk=sem-ga_campid.18853522261_asid.146084015209_crid.633923221414_kw.linkedin_d.c_tid.kwd-296170574619_n.g_mt.e_geo.9031971&mcid=6994434350142418944&cid=&gclid=EAIaIQobChMI94CEo9Lu_QIVFRatBh0z1gK4EAAYASABEgJ1J_D_BwE&gclsrc=aw.ds&position=1&pageNum=0")
# Scroll Websites to the most bottom. Simulate with JavaScript
n = 0
while n < 3:
  driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
  # Wait for 3 seconds
  time.sleep(3)
  n+=1
# Job Titles
titleTags = driver.find_elements(By.CLASS_NAME, "base-search-card__title")
# For Loop
for titleTag in titleTags:
  print(titleTag.text)
driver.close()
```
:::


## LeetCode

::: {.cell .column-page}

```{.python .cell-code}
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By
from msedge.selenium_tools import Edge
import time
# Path to Driver
driver = Edge(executable_path = "msedgedriver.exe")
# Connect to LeetCode
driver.get("https://leetcode.com/accounts/login/")
# Enter Username and password
usernameInput = driver.find_element(By.ID, "id_login")
passwordInput = driver.find_element(By.ID, "id_password")
# Send information
usernameInput.send_keys("billy2003live@gmail.com")
passwordInput.send_keys("Me@0972716677")
signinBtn = driver.find_element(By.ID,"signin_btn")
signinBtn.send_keys(Keys.ENTER)
# Wait for 5 seconds
time.sleep(5)
# Access to information
driver.get("https://leetcode.com/problemset/all/")
time.sleep(3)
statElement = driver.find_element(By.CSS_SELECTOR,"[data-difficulty=TOTAL]")
# print(statElement.text)
columns = statElement.text.split("\n")
print(columns[1])
driver.close()
```
:::


# R Web Crawler

## Star Wars films

::: {.cell .column-page}

```{.r .cell-code}
html <- read_html("https://rvest.tidyverse.org/articles/starwars.html")
```
:::

::: {.cell .column-page}

```{.r .cell-code}
tibble(
  Title = html |> 
          html_elements("section") |> 
          html_element("h2") |> 
          html_text2(),
  `Released Date` = html |> 
                     html_elements("section") |> 
                     html_element("p") |> 
                     html_text2() |> 
                     str_remove("Released: ") |> 
                     parse_date(),
  Director = html |> 
             html_elements("section") |> 
             html_element(".director") |> 
             html_text2(),
  Intro = html |> 
          html_elements("section") |> 
          html_element(".crawl") |> 
          html_text2()
) |> 
  gt() |> 
  cols_align(columns = everything(),align = "center")
```

::: {.cell-output-display}

```{=html}
<div id="ajrtpfqfpv" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#ajrtpfqfpv .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#ajrtpfqfpv .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#ajrtpfqfpv .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}

#ajrtpfqfpv .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#ajrtpfqfpv .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#ajrtpfqfpv .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#ajrtpfqfpv .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#ajrtpfqfpv .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#ajrtpfqfpv .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#ajrtpfqfpv .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#ajrtpfqfpv .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#ajrtpfqfpv .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#ajrtpfqfpv .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  text-align: left;
}

#ajrtpfqfpv .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#ajrtpfqfpv .gt_from_md > :first-child {
  margin-top: 0;
}

#ajrtpfqfpv .gt_from_md > :last-child {
  margin-bottom: 0;
}

#ajrtpfqfpv .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#ajrtpfqfpv .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#ajrtpfqfpv .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#ajrtpfqfpv .gt_row_group_first td {
  border-top-width: 2px;
}

#ajrtpfqfpv .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#ajrtpfqfpv .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#ajrtpfqfpv .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#ajrtpfqfpv .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#ajrtpfqfpv .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#ajrtpfqfpv .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#ajrtpfqfpv .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#ajrtpfqfpv .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#ajrtpfqfpv .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#ajrtpfqfpv .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-left: 4px;
  padding-right: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#ajrtpfqfpv .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#ajrtpfqfpv .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#ajrtpfqfpv .gt_left {
  text-align: left;
}

#ajrtpfqfpv .gt_center {
  text-align: center;
}

#ajrtpfqfpv .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#ajrtpfqfpv .gt_font_normal {
  font-weight: normal;
}

#ajrtpfqfpv .gt_font_bold {
  font-weight: bold;
}

#ajrtpfqfpv .gt_font_italic {
  font-style: italic;
}

#ajrtpfqfpv .gt_super {
  font-size: 65%;
}

#ajrtpfqfpv .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 75%;
  vertical-align: 0.4em;
}

#ajrtpfqfpv .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#ajrtpfqfpv .gt_indent_1 {
  text-indent: 5px;
}

#ajrtpfqfpv .gt_indent_2 {
  text-indent: 10px;
}

#ajrtpfqfpv .gt_indent_3 {
  text-indent: 15px;
}

#ajrtpfqfpv .gt_indent_4 {
  text-indent: 20px;
}

#ajrtpfqfpv .gt_indent_5 {
  text-indent: 25px;
}
</style>
<table class="gt_table">
  
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="Title">Title</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="Released Date">Released Date</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="Director">Director</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1" scope="col" id="Intro">Intro</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td headers="Title" class="gt_row gt_center">The Phantom Menace</td>
<td headers="Released Date" class="gt_row gt_center">1999-05-19</td>
<td headers="Director" class="gt_row gt_center">George Lucas</td>
<td headers="Intro" class="gt_row gt_center">Turmoil has engulfed the Galactic Republic. The taxation of trade routes to outlying star systems is in dispute.

Hoping to resolve the matter with a blockade of deadly battleships, the greedy Trade Federation has stopped all shipping to the small planet of Naboo.

While the Congress of the Republic endlessly debates this alarming chain of events, the Supreme Chancellor has secretly dispatched two Jedi Knights, the guardians of peace and justice in the galaxy, to settle the conflict….</td></tr>
    <tr><td headers="Title" class="gt_row gt_center">Attack of the Clones</td>
<td headers="Released Date" class="gt_row gt_center">2002-05-16</td>
<td headers="Director" class="gt_row gt_center">George Lucas</td>
<td headers="Intro" class="gt_row gt_center">There is unrest in the Galactic Senate. Several thousand solar systems have declared their intentions to leave the Republic.

This separatist movement, under the leadership of the mysterious Count Dooku, has made it difficult for the limited number of Jedi Knights to maintain peace and order in the galaxy.

Senator Amidala, the former Queen of Naboo, is returning to the Galactic Senate to vote on the critical issue of creating an ARMY OF THE REPUBLIC to assist the overwhelmed Jedi….</td></tr>
    <tr><td headers="Title" class="gt_row gt_center">Revenge of the Sith</td>
<td headers="Released Date" class="gt_row gt_center">2005-05-19</td>
<td headers="Director" class="gt_row gt_center">George Lucas</td>
<td headers="Intro" class="gt_row gt_center">War! The Republic is crumbling under attacks by the ruthless Sith Lord, Count Dooku. There are heroes on both sides. Evil is everywhere.

In a stunning move, the fiendish droid leader, General Grievous, has swept into the Republic capital and kidnapped Chancellor Palpatine, leader of the Galactic Senate.

As the Separatist Droid Army attempts to flee the besieged capital with their valuable hostage, two Jedi Knights lead a desperate mission to rescue the captive Chancellor….</td></tr>
    <tr><td headers="Title" class="gt_row gt_center">A New Hope</td>
<td headers="Released Date" class="gt_row gt_center">1977-05-25</td>
<td headers="Director" class="gt_row gt_center">George Lucas</td>
<td headers="Intro" class="gt_row gt_center">It is a period of civil war. Rebel spaceships, striking from a hidden base, have won their first victory against the evil Galactic Empire.

During the battle, Rebel spies managed to steal secret plans to the Empire’s ultimate weapon, the DEATH STAR, an armored space station with enough power to destroy an entire planet.

Pursued by the Empire’s sinister agents, Princess Leia races home aboard her starship, custodian of the stolen plans that can save her people and restore freedom to the galaxy….</td></tr>
    <tr><td headers="Title" class="gt_row gt_center">The Empire Strikes Back</td>
<td headers="Released Date" class="gt_row gt_center">1980-05-17</td>
<td headers="Director" class="gt_row gt_center">Irvin Kershner</td>
<td headers="Intro" class="gt_row gt_center">It is a dark time for the Rebellion. Although the Death Star has been destroyed, Imperial troops have driven the Rebel forces from their hidden base and pursued them across the galaxy.

Evading the dreaded Imperial Starfleet, a group of freedom fighters led by Luke Skywalker has established a new secret base on the remote ice world of Hoth.

The evil lord Darth Vader, obsessed with finding young Skywalker, has dispatched thousands of remote probes into the far reaches of space….</td></tr>
    <tr><td headers="Title" class="gt_row gt_center">Return of the Jedi</td>
<td headers="Released Date" class="gt_row gt_center">1983-05-25</td>
<td headers="Director" class="gt_row gt_center">Richard Marquand</td>
<td headers="Intro" class="gt_row gt_center">Luke Skywalker has returned to his home planet of Tatooine in an attempt to rescue his friend Han Solo from the clutches of the vile gangster Jabba the Hutt.

Little does Luke know that the GALACTIC EMPIRE has secretly begun construction on a new armored space station even more powerful than the first dreaded Death Star.

When completed, this ultimate weapon will spell certain doom for the small band of rebels struggling to restore freedom to the galaxy…</td></tr>
    <tr><td headers="Title" class="gt_row gt_center">The Force Awakens</td>
<td headers="Released Date" class="gt_row gt_center">2015-12-11</td>
<td headers="Director" class="gt_row gt_center">J. J. Abrams</td>
<td headers="Intro" class="gt_row gt_center">Luke Skywalker has vanished. In his absence, the sinister FIRST ORDER has risen from the ashes of the Empire and will not rest until Skywalker, the last Jedi, has been destroyed. With the support of the REPUBLIC, General Leia Organa leads a brave RESISTANCE. She is desperate to find her brother Luke and gain his help in restoring peace and justice to the galaxy. Leia has sent her most daring pilot on a secret mission to Jakku, where an old ally has discovered a clue to Luke’s whereabouts….</td></tr>
  </tbody>
  
  
</table>
</div>
```

:::
:::


## PTT Movies

::: {.cell .column-page}

```{.r .cell-code}
moveis_html <- read_html("https://www.ptt.cc/bbs/movie/index.html")
```
:::

::: {.cell .column-page}

```{.r .cell-code}
moveis_html |> 
           html_elements("div") |> 
           html_elements(".title") |> 
           html_text2()
```

::: {.cell-output .cell-output-stdout}
```
[1] "[新聞] 沙贊戲外槓上黑亞當 證實巨石不讓他客串"        
[2] "Re: [討論] 進擊的鼓手-嚴厲的老師是錯的嗎? (雷)"      
[3] "[新聞] 電視插播致中國5000人遭非法逮捕 紀錄片還原真相"
[4] "Re: [討論] 進擊的鼓手-嚴厲的老師是錯的嗎? (雷)"      
[5] "[情報] 《戰爭機器》改編電影編劇確定"                 
[6] "[公告] 電影板板規 2022/12/5"                         
```
:::
:::


##  IMDB top films

::: {.cell .column-page}

```{.r .cell-code}
imdb_url <- read_html("https://www.imdb.com/chart/top")
```
:::

::: {.cell .column-page}

```{.r .cell-code}
imdb_url |> 
         html_element("table") |> 
         html_table() |> 
         select(
             rank_title_year = `Rank & Title`,
             rating = `IMDb Rating`
                ) |> 
         mutate(
            rank_title_year = str_replace_all(rank_title_year, "\n +", " ")
               ) |> 
        separate_wider_regex(
            rank_title_year,
              patterns = c(
               rank = "\\d+", "\\. ",
               title = ".+", " +\\(",
               year = "\\d+", "\\)")
         ) |> 
         reactable(resizable = T,filterable = T,defaultColDef = colDef(align = "center"))
```

::: {.cell-output-display}

```{=html}
<div class="reactable html-widget html-fill-item-overflow-hidden html-fill-item" id="htmlwidget-b5c1fefcf891e5755beb" style="width:auto;height:auto;"></div>
<script type="application/json" data-for="htmlwidget-b5c1fefcf891e5755beb">{"x":{"tag":{"name":"Reactable","attribs":{"data":{"rank":["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30","31","32","33","34","35","36","37","38","39","40","41","42","43","44","45","46","47","48","49","50","51","52","53","54","55","56","57","58","59","60","61","62","63","64","65","66","67","68","69","70","71","72","73","74","75","76","77","78","79","80","81","82","83","84","85","86","87","88","89","90","91","92","93","94","95","96","97","98","99","100","101","102","103","104","105","106","107","108","109","110","111","112","113","114","115","116","117","118","119","120","121","122","123","124","125","126","127","128","129","130","131","132","133","134","135","136","137","138","139","140","141","142","143","144","145","146","147","148","149","150","151","152","153","154","155","156","157","158","159","160","161","162","163","164","165","166","167","168","169","170","171","172","173","174","175","176","177","178","179","180","181","182","183","184","185","186","187","188","189","190","191","192","193","194","195","196","197","198","199","200","201","202","203","204","205","206","207","208","209","210","211","212","213","214","215","216","217","218","219","220","221","222","223","224","225","226","227","228","229","230","231","232","233","234","235","236","237","238","239","240","241","242","243","244","245","246","247","248","249","250"],"title":["The Shawshank Redemption","The Godfather","The Dark Knight","The Godfather Part II","12 Angry Men","Schindler's List","The Lord of the Rings: The Return of the King","Pulp Fiction","The Lord of the Rings: The Fellowship of the Ring","The Good, the Bad and the Ugly","Forrest Gump","Fight Club","The Lord of the Rings: The Two Towers","Inception","Star Wars: Episode V - The Empire Strikes Back","The Matrix","Goodfellas","One Flew Over the Cuckoo's Nest","Se7en","Seven Samurai","It's a Wonderful Life","The Silence of the Lambs","Saving Private Ryan","City of God","Interstellar","Life Is Beautiful","The Green Mile","Star Wars","Terminator 2: Judgment Day","Back to the Future","Spirited Away","The Pianist","Psycho","Parasite","Léon: The Professional","The Lion King","Gladiator","American History X","The Departed","The Prestige","The Usual Suspects","Whiplash","Casablanca","Grave of the Fireflies","Harakiri","The Intouchables","Modern Times","Once Upon a Time in the West","Cinema Paradiso","Rear Window","Alien","City Lights","Apocalypse Now","Memento","Django Unchained","Raiders of the Lost Ark","WALL·E","The Lives of Others","Sunset Blvd.","Paths of Glory","The Shining","The Great Dictator","Avengers: Infinity War","Witness for the Prosecution","Aliens","Spider-Man: Into the Spider-Verse","American Beauty","Dr. Strangelove or: How I Learned to Stop Worrying and Love the Bomb","The Dark Knight Rises","Oldboy","Inglourious Basterds","Amadeus","Coco","Toy Story","Joker","Braveheart","Das Boot","Avengers: Endgame","Princess Mononoke","Once Upon a Time in America","Good Will Hunting","Your Name.","Singin' in the Rain","3 Idiots","Requiem for a Dream","High and Low","Toy Story 3","Capernaum","Star Wars: Episode VI - Return of the Jedi","Eternal Sunshine of the Spotless Mind","2001: A Space Odyssey","Come and See","Reservoir Dogs","The Hunt","Citizen Kane","M","Lawrence of Arabia","North by Northwest","Ikiru","Vertigo","The Apartment","Amélie","A Clockwork Orange","Double Indemnity","Full Metal Jacket","Scarface","Hamilton","Incendies","Heat","To Kill a Mockingbird","Up","The Sting","A Separation","Metropolis","Taxi Driver","L.A. Confidential","Die Hard","Top Gun: Maverick","Snatch","Indiana Jones and the Last Crusade","Bicycle Thieves","Like Stars on Earth","1917","Downfall","Dangal","For a Few Dollars More","Batman Begins","The Kid","Some Like It Hot","The Father","All About Eve","The Wolf of Wall Street","Green Book","Judgment at Nuremberg","Ran","Casino","The Truman Show","There Will Be Blood","Pan's Labyrinth","Unforgiven","The Sixth Sense","Shutter Island","A Beautiful Mind","Jurassic Park","Yojimbo","The Treasure of the Sierra Madre","Monty Python and the Holy Grail","No Country for Old Men","The Great Escape","Kill Bill: Vol. 1","Rashomon","Spider-Man: No Way Home","The Thing","Finding Nemo","The Elephant Man","Chinatown","Raging Bull","V for Vendetta","Gone with the Wind","Lock, Stock and Two Smoking Barrels","Dial M for Murder","Inside Out","The Secret in Their Eyes","Howl's Moving Castle","Three Billboards Outside Ebbing, Missouri","Trainspotting","The Bridge on the River Kwai","Prisoners","Warrior","Fargo","Gran Torino","My Neighbor Totoro","Catch Me If You Can","Million Dollar Baby","Children of Heaven","Blade Runner","The Gold Rush","Klaus","Before Sunrise","Harry Potter and the Deathly Hallows: Part 2","12 Years a Slave","On the Waterfront","Ben-Hur","The Grand Budapest Hotel","Gone Girl","Wild Strawberries","The General","Barry Lyndon","In the Name of the Father","The Third Man","The Deer Hunter","Hacksaw Ridge","The Wages of Fear","Memories of Murder","Sherlock Jr.","Wild Tales","Mr. Smith Goes to Washington","Mad Max: Fury Road","Mary and Max","The Seventh Seal","How to Train Your Dragon","Monsters, Inc.","Jaws","Dead Poets Society","Room","The Big Lebowski","Tokyo Story","The Passion of Joan of Arc","Ford v Ferrari","Hotel Rwanda","Rocky","Platoon","Ratatouille","Spotlight","Logan","The Terminator","Stand by Me","Rush","Network","Before Sunset","Into the Wild","The Wizard of Oz","The Best Years of Our Lives","Groundhog Day","The Exorcist","Pather Panchali","The Incredibles","To Be or Not to Be","La haine","Pirates of the Caribbean: The Curse of the Black Pearl","The Battle of Algiers","The Grapes of Wrath","Hachi: A Dog's Tale","Jai Bhim","My Father and My Son","Amores Perros","Rebecca","Cool Hand Luke","The Handmaiden","The 400 Blows","The Sound of Music","It Happened One Night","Persona","Life of Brian","The Iron Giant","The Help","Dersu Uzala","Aladdin","Dances with Wolves","Gandhi"],"year":["1994","1972","2008","1974","1957","1993","2003","1994","2001","1966","1994","1999","2002","2010","1980","1999","1990","1975","1995","1954","1946","1991","1998","2002","2014","1997","1999","1977","1991","1985","2001","2002","1960","2019","1994","1994","2000","1998","2006","2006","1995","2014","1942","1988","1962","2011","1936","1968","1988","1954","1979","1931","1979","2000","2012","1981","2008","2006","1950","1957","1980","1940","2018","1957","1986","2018","1999","1964","2012","2003","2009","1984","2017","1995","2019","1995","1981","2019","1997","1984","1997","2016","1952","2009","2000","1963","2010","2018","1983","2004","1968","1985","1992","2012","1941","1931","1962","1959","1952","1958","1960","2001","1971","1944","1987","1983","2020","2010","1995","1962","2009","1973","2011","1927","1976","1997","1988","2022","2000","1989","1948","2007","2019","2004","2016","1965","2005","1921","1959","2020","1950","2013","2018","1961","1985","1995","1998","2007","2006","1992","1999","2010","2001","1993","1961","1948","1975","2007","1963","2003","1950","2021","1982","2003","1980","1974","1980","2005","1939","1998","1954","2015","2009","2004","2017","1996","1957","2013","2011","1996","2008","1988","2002","2004","1997","1982","1925","2019","1995","2011","2013","1954","1959","2014","2014","1957","1926","1975","1993","1949","1978","2016","1953","2003","1924","2014","1939","2015","2009","1957","2010","2001","1975","1989","2015","1998","1953","1928","2019","2004","1976","1986","2007","2015","2017","1984","1986","2013","1976","2004","2007","1939","1946","1993","1973","1955","2004","1942","1995","2003","1966","1940","2009","2021","2005","2000","1940","1967","2016","1959","1965","1934","1966","1979","1999","2011","1975","1992","1990","1982"],"rating":[9.2,9.2,9,9,9,8.9,8.9,8.8,8.8,8.8,8.8,8.7,8.7,8.7,8.7,8.7,8.7,8.6,8.6,8.6,8.6,8.6,8.6,8.6,8.6,8.6,8.6,8.5,8.5,8.5,8.5,8.5,8.5,8.5,8.5,8.5,8.5,8.5,8.5,8.5,8.5,8.5,8.5,8.5,8.5,8.5,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.4,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.3,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.2,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8.1,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8,8]},"columns":[{"id":"rank","name":"rank","type":"character","align":"center"},{"id":"title","name":"title","type":"character","align":"center"},{"id":"year","name":"year","type":"character","align":"center"},{"id":"rating","name":"rating","type":"numeric","align":"center"}],"resizable":true,"filterable":true,"dataKey":"218c3434cb566ce63a4b1931ac0fc47d"},"children":[]},"class":"reactR_markup"},"evals":[],"jsHooks":[]}</script>
```

:::
:::

::: {.cell}

```{.r .cell-code}
knitr::knit_exit()
```
:::




