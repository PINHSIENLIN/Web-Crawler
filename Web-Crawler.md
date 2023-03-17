---
title: "Web Crawler"
author: "Alvin, Lin"
date: "2023-03-16"
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
[  好雷] 比悲傷更悲傷的故事
[好雷] 《若愛重來》，二次機會該給愛情，還是留
[情報] 夢工廠動畫《變身少女露比》
[新聞] 劉德華將挑戰做導演　成龍吳京主演：我覺得我會紅了
[討論] iTunes特價 臥虎藏龍4K版/汪汪隊立大功
[情報]鈴芽之旅國語配音版3/17上映
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
[問卦] 有沒有餐餐自助餐能多省的八卦？
[問卦] 高雄發經典賽祭品有人會來嗎
Re: [問卦] 在歐美人眼中，「亞洲」根本就是貶義詞吧
Re: [新聞] 電價估漲11% 逾8成住宅用電凍漲
[問卦] 輪胎磨平後當熱熔胎用
Re: [新聞] 快訊／震撼彈！　林昶佐宣布不競選2024
[問卦] 台灣經濟起飛2003~2023已經20年
[新聞] YG出動豪華專機！BLACKPINK準備空降高雄
[新聞] 賠償黑奴後代 舊金山研議每人發500萬、
[新聞] 「全原運在台北」台南代表隊竟住桃園 穎
Re: [問卦] 我是處男為什麼我不能捐血？
[問卦] 通膨很要緊嗎？
Re: [新聞] 我曾外援1「非邦交國」8億？郭正亮嚇
Re: [問卦] 40歲以上失業會很難找工作嗎？
[問卦] 現在網頁廣告也太多了吧？
[問卦] 安西這麼雙標,湘北其他隊員不會不爽嗎
Re: [問卦] 欠民間180萬我還不了
[公告] 八卦板板規(2023.03.01)
[協尋] 3/3 16：50-17：20大寮區新厝路和新三
[爆卦] 在韓國發現疑似柬埔寨詐騙手法(歡迎記者
[協尋] 台2濱海事故
[協尋] 3/15 7:44高雄大寮行車記錄器(更新地點)
[問卦] 今年值癸屬黑色，明年值甲屬什麼顏色？
[新聞] 電價估漲11% 逾8成住宅用電凍漲
Re: [新聞] 國民黨選策會名單惹議 朱立倫：所有的錯
[問卦] 還有什麼事情是不健康的?
Re: [新聞] 彰化輕航機墜毀2死 18歲日本旅人體驗飛
[問卦] 為什麼廁所要挖洞啊？
[問卦] 會講客語的客家人怎麼看不會客語的客家
[問卦] 一人說一個覺得line很爛的地方?
[問卦] 股市噴三小？
[新聞] 印度神童預言再命中 台灣也要小心9件事
[問卦] 欸 跑跑卡丁車飄移怎麼都配到一樣的人
[問卦] 湯麵內附的蛋都換成鳥蛋了嗎
[問卦] 一句話証明你當過兵
[問卦] 開放台灣人遺失物警察火速找到送達
[問卦] 去澎湖要吃哪家海鮮才不會當盤子
[問卦] 玉子燒一份這樣大小很盤嗎?
[問卦] 沖繩為台灣開始駐軍抵抗中國
[問卦] 大家期待2025非核家園嗎?
[新聞] 高雄巨蛋旅展明天登場 海外自由行7688元
[問卦] 物價上漲484基本工資調漲害的
Re: [新聞] 「已淪民進黨沙包！」趙少康籲解散選策會
[問卦] 手搖杯改成半糖，算減糖嗎？
[問卦] 鄭家純屬「雞」犯太歲 有辦法化解嗎?
[問卦] 2024他是不是穩了？
Re: [問卦] 普丁是怎麼做到不閃尿???
Re: [新聞] 少吃蛋顧健康？心臟科醫師：雞蛋吃太多
Re: [問卦] 該推廣「洗澡不健康」了吧
[問卦] 220便當配菜出現電話線，你OK嗎？
[問卦] 政府真的很在乎人民健康
[問卦] 為什麼廁所門要留這麼大的縫？
Re: [問卦] 你有多久沒流過我慢汁了??????????????
Re: [新聞] 外媒稱關閉核二廠今夏恐大停電 陳建仁：
[問卦] 除了醫師以外還有什麼工作是神聖的？
Re: [新聞] 我曾外援1「非邦交國」8億？郭正亮嚇壞：
[問卦] 員工旅遊去韓國但行程排的很爛
Re: [新聞] 少吃蛋顧健康？心臟科醫師：雞蛋吃太多
Re: [問卦] 欠民間180萬我還不了
[問卦] 正常人上戰場多久以後會壞掉
[問卦] 怎麼分辨約砲訊息的真假
```
:::
:::
