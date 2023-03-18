---
title: "Web Crawler"
author: "Alvin, Lin"
date: "2023-03-17"
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


# Web Crawler

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
[閒聊] 湯姆克魯斯看了《閃電俠》後表示很喜歡
[新聞]《捍衛任務4》上映前要角過世享壽60歲
[新聞] 真人版《小美人魚》上映前再爆話題熱度 
[討論] 王子也找黑人演能拯救小美人魚嗎？
Re: [討論] 沙贊 眾神之怒 爛番茄開盤
[新聞] 《侏儸紀公園》山姆尼爾驚傳罹淋巴癌3期
[新聞] 近日風靡韓國的日本動畫 — 韓國電影就
[好雷]《流水落花》，淡淡交會過，各不留下印，
[問片] 類似血觀音或灼人秘密的電影
[好雷] 鈴芽之旅 台灣配音版
[請益] 沙贊佛萊迪的身世？
[討論] 沙贊有比黑亞當好看嗎?
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
[問卦] 不講國語要什麼稱呼肥宅？
[問卦] 為何日韓妹子願意來台灣定居拍視頻啊?
[問卦] 台電員工支不支持漲電價？
[問卦] 俄烏都打一年了美國在幹嘛啊
[問卦] 之後會有醫生宣導每天不用喝太多水嗎
[問卦] 甄嬛的隊友在自卑殺小
[新聞] 在日本藏壽司拍攝舔醬油瓶口影片 19歲少
[問卦] 訂披薩app顯示的流程準確嗎？
[問卦] 笑死人 江澤民被通緝都沒事 更何況普丁?
[爆卦] 獨行俠絕殺湖人
[問卦] 226超鐵是什麼體驗！？？
[新聞] 318太陽花學運9週年　民進黨：秉持初衷
[問卦] ㄧ個人住網速多少才夠
[新聞] （圖輯）BLACKPINK開唱！　半夜排隊「買
[問卦] 去球賽狂看啦啦隊，球隊老闆會哭嗎？
[問卦] 朋友拿6000賭今年會有大停電 該賭嗎?
[公告] 八卦板板規(2023.03.01)
[協尋] 3/3 16：50-17：20大寮區新厝路和新三
[爆卦] 在韓國發現疑似柬埔寨詐騙手法(歡迎記者
[協尋] 台2濱海事故
[協尋] 3/15 7:44高雄大寮行車記錄器(更新地點)
[問卦] 職棒啦啦隊一個月薪水多少？
[問卦] 太陽花學運滿九年了  島嶼天光了嗎
[問卦]歐巴桑插隊
[問卦] 為啥雞排比雞腿還貴ㄚ
[問卦] 不認識Blackpink的人大概幾歲？
Re: [新聞] 白宮：美國不支持陸的烏俄和平計畫
[問卦] 不講台語的買吻仔魚粥都怎麼唸？
[問卦] 停核能漲電價的人是不是腦袋不好
[問卦] 說一個啦啦隊女孩名字你會想到誰
[問卦] 藏壽司的盤子最後都去哪裡了?
[問卦] 不講國語的都怎麼約肛?
[新聞] 朱立倫、侯友宜關係被炒作？ 親近議員
[問卦] 肥宅改專攻螃蟹了 老闆該哭了吧
Re: [問卦] 為什麼戴資穎在八卦版總是被酸？
[問卦] 台灣停電若要負責，城市優先順序是?
Re: [問卦] 台灣價值具體來說是什麼意思
[問卦] 牙醫說他一週只工作兩天？
Re: [新聞] 涉嫌犯下戰爭罪 國際刑事法院下令逮捕普京
[問卦] 為什麼中國未成年女生 喜歡拍懷孕給人看?
[問卦] 爲什麼經典賽不全部在美國場地就好？
[問卦] Dcard熱門：女友喝醉後 被網友上了..
[問卦] 李多慧來台當藝人也是海放吧
[問卦] 當年投反核的人，現在後悔嗎
[問卦] 4/14是啦啦隊的終局之戰嗎？
[問卦] 漫威電影是不是快不行了？
[問卦] 波波最大問題就是連當地都不承認吧
[問卦] 700度那租屋族用電怎麼辦
[問卦] 勾起你心中的華
Re: [新聞] 《小美人魚》最新劇照又惹議！鄉民愣：想
[問卦] 倪福德跟陳偉殷誰比較有天分?
Re: [新聞] 蔡政府第3度調電價 漲幅仍低於馬政府2008
[問卦] 漲價潮是不是最後一波要結束了??
Re: [問卦] 為什麼戴資穎在八卦版總是被酸？
[問卦] 光明會,新世界秩序失敗?
[問卦] 穩聊能幹嘛？
Re: [問卦] 國動找林襄上直播能挽救人氣嗎
[問卦] 現在牙醫 怎六日幾乎都沒在看?
[問卦] 你們是站組的嗎
[問卦] 不講台語的買蚵仔煎都怎麼唸
Re: [問卦] 台灣價值具體來說是什麼意思
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
Composability in LLM Apps
Why are women encouraged to go into UX?
Here’s Everything that Happened at the #BlackTwitterSummit
Don’t Let Google Manage Your Passwords
The TRUTH About Smartphone Reviews
Why Holding onto Failed Artwork May be Holding You Back
Creative Writers in the Know Use the Power of Congeries
Need Help With Your Writing Journey? Here’s What To Do
Don’t Shy Away From Tricky Topics
The New AI Art Generators Can Teach You a Thing or Two About Writing
Fundraising Too Early Is the Worst Decision You’ll Make as a Startup Founder
How can we help?
The Ultimate Guide to Y-Combinator Applications
Business Strategy on a Page
The Rise and Fall of the Dot-Com Foosball Table
What the F*** is a ‘Regular Black Woman?’
The Time I Got Cursed Out By Rapper Talib Kweli
Things Aren’t Black and White
The Black Things We Fear
Ancestor Veneration in the Akan Culture of Ghana
Big Business can’t stop its illegal, fantastically lucrative gossiping
Learning from Silicon Valley Bank’s apologists
5 Lessons for Investors from Silicon Valley Bank’s Collapse
Come Fly the Competitive Skies
Mindfulness Meditation Has Great, Research-Backed Benefits
More Young Folks Have Colorectal Cancer. Why?
Do You Think You Might Ever Consider Medically Assisted Dying?
What’s the Minimum Amount of Movement You Need to Offset Your Sedentary Lifestyle?
What Every Parent Needs To Know About Teen Substance Use
Why and How to Get More Skillful at Office Politics
Doing Cal Newport’s Digital Declutter Was Like Finding Gold Under My House
Back to Basics, Part Tres: Logistic Regression
The Biggest Issue with Long-Term Productivity
My Creative Writing Setup
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
