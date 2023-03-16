---
title: "Python Numpy"
author: "Alvin, Lin"
date: "2023-03-15"
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

::: {.cell .column-page}

```{.python .cell-code}
# Get url from PTT movies
import urllib.request as req
url = "https://www.ptt.cc/bbs/movie/index.html"
# Request with headers information
request = req.Request(url, headers = {
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
[新聞] DC 總裁岡恩正式宣佈親自執導新版《超人
[討論] Guillermo上節目說JJ欠他7塊錢
[好雷] 瘋狂富作用 探討權力的荒謬喜劇
[討論] 《沙贊2》我懷疑華納在下一盤很大的棋
[新聞] 基努李維盛讚湯姆克魯斯「來自另一個宇宙
[公告] 電影板板規 2022/12/5
```
:::
:::
