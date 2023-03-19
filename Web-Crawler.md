---
title: "Web Crawler"
author: "Alvin, Lin"
date: "2023-03-18"
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
[新聞] LEXUS短影片競賽 啟動
[新聞]《星際異攻隊3》內部試映成績優秀！
[新聞] 丹佐華盛頓驚喜參演《神鬼戰士 2》！
Re: [討論] 為什麼楊紫瓊沒拿過金馬獎最佳女主角
Re: [新聞] 真人版《小美人魚》上映前再爆話題熱
[新聞] 瑪利歐導演：碧姬公主將不是遇難少女！
[新聞] 《沙贊2》成為DC最差，爛番茄降至54%
[請益] 關於黑的教育一些疑問
[討論] 基努李維上今夜秀宣傳....
[討論] WW1984反派麥斯
[問片] 某一部關於天堂地獄、心境轉折的電影
[偏負雷] 沙讚2 
[討論] 佈局(看不見的客人) 一問
[討論] 看過十二夜的心理有留下陰影嗎？？
[好雷] 沙贊2
[請益] 請問電影評價站，有哪裡還沒被行銷攻佔
[討論] 鈴芽之旅的缺點是觀眾觀影素養不足？
Re: [討論] 新警察故事拍續集 成龍監製 謝霆鋒執導
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
[問卦] 為什麼同款小瓶礦泉水比較貴？
[問卦] 誰會成為台灣的艾蓮。葉卡？
[問卦] 調漲基本工資 物價也跟漲  有啥意義？
[問卦] 演唱會前一天先潛入有可能嗎
[問卦] 一度電8元大概在世界是哪個水準
[問卦] 台灣用路人心理素質都很差？
[問卦] 工作交接，女同事做得亂七八糟還嗆人?
[問卦] 八年級生是不是已經算老人了？
[問卦] 音樂課才藝表演大家都表演什麼?
[問卦] 日系阿宅懂日文的占幾成啊？
Re: [問卦] 幹你娘 重機騎士其實很勇敢
[問卦] 為什麼軍武版沒看到烏軍營長撤職消息？
Re: [問卦] 這七年你覺得最低能的事情是啥？
[問卦] 請問嘉義人，IKEA嘉義店484涼了？
[問卦] 高速行駛中，要怎麼撞後車？
[問卦] 女友不溝通都怎麼解決？
Re: [問卦] 陸戰儀隊操演跟一般儀隊有什麼不一樣？
[問卦] 漲電價通膨 罵的比 漲基本工資通膨 的少
Re: [問卦] 幹你娘塔綠班開始帶囤蛋風向了嗎？
[問卦] 現在還有人care肺炎嗎
[公告] 八卦板板規(2023.03.01)
[協尋] 3/3 16：50-17：20大寮區新厝路和新三
[協尋] 台2濱海事故
[協尋] 3/15 7:44高雄大寮行車記錄器(更新地點)
[公告] 代PO 政黑進板圖徵選 5/23截止(每推100P
Re: [問卦] 2024的總統會是屎缺嗎？
[問卦] 這七年你覺得最低能的事情是啥？
Re: [問卦] 2024的總統會是屎缺嗎？
[問卦]  有木有馬特洪峰的八卦？
[問卦] 為什麼台灣媽祖信仰那麼興盛？
[問卦] 基隆港務局空中花園是不是看港景最好veiw
[問卦] 官方語言改英文是不是會人才流失?
[問卦] 這是一個黑女巫用武漢肺炎欺負公主的故事
[問卦] 什麼叫做台灣化？
[問卦]用心良苦民進黨讓台灣再次偉大
Re: [新聞] 歐洲不滿中國干預 北京形象如墜落氣球不
[問卦] 結婚交往後跟其他人有點來往不好嗎
[問卦] 聽韓團粉絲懂韓文的占幾成啊？
Re: [問卦]  有木有馬特洪峰的八卦？
[問卦] 希特勒算是好人還是壞人？
[新聞] BLACKPINK演唱會全程被擋！網爆「賈永婕
[問卦] 五月天阿信怎麼了？
Re: [問卦] 2024的總統會是屎缺嗎？
[問卦] =.= 高雄好熱歐...
[新聞] 離婚只為「4人行」！2夫妻同居換換愛　床
[問卦] 大家願意為美食排多久的隊？
[問卦] 想追的女生想看black pink你會買嗎？
[問卦] 大家花了多久才接受君白離開的事實？
[問卦] 陸戰儀隊操演跟一般儀隊有什麼不一樣？
[問卦] 館長什麼時候要開半套店？
[問卦] 馬份演員 演自己本人是演啥劇情？
[問卦] 什麼時候才要立法禁止7-11印廢紙
[問卦] 欸！ 俄國女高音順利在日本演出！
[問卦] 重機騎士車禍後在想什麼？
[新聞] 私藏大谷翔平一舉一動筆記本　達比修有
[新聞] 煉金術？倫敦金屬交易所 54噸鎳礦變成石
[問卦] 從統粉變椅粉了怎麼辦==
[新聞] 美麗島公布2024最新民調 吳子嘉分析：侯
[問卦] 女孩最受不了肥宅屌臭還是腳臭
[新聞] 學生宿舍也漲！台大太子學舍9月調價
Re: [問卦] 鄉民都怎麼戒酒的？
[新聞] 台中人孔蓋害「雷殘」 先生手骨折、她懷
Re: [問卦] 欸幹！警察說這樣不算肇事逃逸？？！
Re: [問卦] 血小板500cc值多少錢？
[問卦] 小時候的文具店為何消失了
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
Here’s Everything That Happened At The #BlackTwitterSummit
Don’t Let Google Manage Your Passwords
The TRUTH About Smartphone Reviews
UX lessons from a pottery professor about lean product design
Why Holding onto Failed Artwork May be Holding You Back
Creative Writers in the Know Use the Power of Congeries
Need Help With Your Writing Journey? Here’s What To Do
Don’t Shy Away From Tricky Topics
Fundraising Too Early Is the Worst Decision You’ll Make as a Startup Founder
How can we help?
The Ultimate Guide to Y-Combinator Applications
Business Strategy on a Page
The Rise and Fall of the Dot-Com Foosball Table
Bartender Refuses to Throw Out 2 Men Kissing at the Bar, Tosses Out Complaining Homophobe Instead
Maegan Hall, Black Men, Historical Flashbacks…and a Strange Demand for Black Women’s Support
What the F*** is a ‘Regular Black Woman?’
The Time I Got Cursed Out By Rapper Talib Kweli
Things Aren’t Black and White
Who Will Make Money from the Generative AI Gold Rush? Part I
Venture Catastrophists
Big Business can’t stop its illegal, fantastically lucrative gossiping
Learning from Silicon Valley Bank’s apologists
Research Shows Fast Poops Are Better
Six Strange & Scary Sleep Sabotagers
Mindfulness Meditation Has Great, Research-Backed Benefits
More Young Folks Have Colorectal Cancer. Why?
Do You Think You Might Ever Consider Medically Assisted Dying?
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


## Request Data(Request Payload Request Body)

::: {.cell .column-page}

```{.python .cell-code}
# Get url from PTT movies
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
Live Drawing The 95th Oscars
How to Survive the Stupid Spring Forward of Daylight Saving Time
I Asked Leading Covid Scientists — Off the Record — About the Virus’s Origins and the ‘Lab Leak’…
Angela Bassett Is Allowed to be Disappointed
From Performance Reviews to Awards: The Power of Articulating Impact
‘Women’s history is women’s right’
How ‘Should’ Makes Us Stupid — And How to Get Smart Again
It’s International Women’s Day, and Quantum has a long way to go
How Devon Price Redefined ‘Lazy’ and Turned His Medium Essay Into a Book
The art of quitting — lessons from my Ph.D. journey
```
:::
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
<div id="cnipjysnbf" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#cnipjysnbf .gt_table {
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

#cnipjysnbf .gt_heading {
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

#cnipjysnbf .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}

#cnipjysnbf .gt_title {
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

#cnipjysnbf .gt_subtitle {
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

#cnipjysnbf .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#cnipjysnbf .gt_col_headings {
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

#cnipjysnbf .gt_col_heading {
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

#cnipjysnbf .gt_column_spanner_outer {
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

#cnipjysnbf .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#cnipjysnbf .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#cnipjysnbf .gt_column_spanner {
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

#cnipjysnbf .gt_group_heading {
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

#cnipjysnbf .gt_empty_group_heading {
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

#cnipjysnbf .gt_from_md > :first-child {
  margin-top: 0;
}

#cnipjysnbf .gt_from_md > :last-child {
  margin-bottom: 0;
}

#cnipjysnbf .gt_row {
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

#cnipjysnbf .gt_stub {
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

#cnipjysnbf .gt_stub_row_group {
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

#cnipjysnbf .gt_row_group_first td {
  border-top-width: 2px;
}

#cnipjysnbf .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#cnipjysnbf .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#cnipjysnbf .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#cnipjysnbf .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#cnipjysnbf .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#cnipjysnbf .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#cnipjysnbf .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#cnipjysnbf .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#cnipjysnbf .gt_footnotes {
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

#cnipjysnbf .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-left: 4px;
  padding-right: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#cnipjysnbf .gt_sourcenotes {
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

#cnipjysnbf .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#cnipjysnbf .gt_left {
  text-align: left;
}

#cnipjysnbf .gt_center {
  text-align: center;
}

#cnipjysnbf .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#cnipjysnbf .gt_font_normal {
  font-weight: normal;
}

#cnipjysnbf .gt_font_bold {
  font-weight: bold;
}

#cnipjysnbf .gt_font_italic {
  font-style: italic;
}

#cnipjysnbf .gt_super {
  font-size: 65%;
}

#cnipjysnbf .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 75%;
  vertical-align: 0.4em;
}

#cnipjysnbf .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#cnipjysnbf .gt_indent_1 {
  text-indent: 5px;
}

#cnipjysnbf .gt_indent_2 {
  text-indent: 10px;
}

#cnipjysnbf .gt_indent_3 {
  text-indent: 15px;
}

#cnipjysnbf .gt_indent_4 {
  text-indent: 20px;
}

#cnipjysnbf .gt_indent_5 {
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
moveis_url <- read_html("https://www.ptt.cc/bbs/movie/index.html")
```
:::

::: {.cell .column-page}

```{.r .cell-code}
moveis_url |> 
           html_elements("div") |> 
           html_elements(".title") |> 
           html_text2()
```

::: {.cell-output .cell-output-stdout}
```
 [1] "[新聞] LEXUS短影片競賽 啟動"                    
 [2] "[新聞]《星際異攻隊3》內部試映成績優秀！"        
 [3] "[新聞] 丹佐華盛頓驚喜參演《神鬼戰士 2》！"      
 [4] "Re: [討論] 為什麼楊紫瓊沒拿過金馬獎最佳女主角"  
 [5] "Re: [新聞] 真人版《小美人魚》上映前再爆話題熱"  
 [6] "[新聞] 瑪利歐導演：碧姬公主將不是遇難少女！"    
 [7] "[新聞] 《沙贊2》成為DC最差，爛番茄降至54%"      
 [8] "[請益] 關於黑的教育一些疑問"                    
 [9] "[討論] 基努李維上今夜秀宣傳...."                
[10] "[討論] WW1984反派麥斯"                          
[11] "[問片] 某一部關於天堂地獄、心境轉折的電影"      
[12] "[偏負雷] 沙讚2"                                 
[13] "[討論] 佈局(看不見的客人) 一問"                 
[14] "[討論] 看過十二夜的心理有留下陰影嗎？？"        
[15] "[好雷] 沙贊2"                                   
[16] "[請益] 請問電影評價站，有哪裡還沒被行銷攻佔"    
[17] "[討論] 鈴芽之旅的缺點是觀眾觀影素養不足？"      
[18] "Re: [討論] 新警察故事拍續集 成龍監製 謝霆鋒執導"
[19] "[公告] 電影板板規 2022/12/5"                    
```
:::
:::
