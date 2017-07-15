layout: post
title:  "Regex Expression"
date:   2017-07-15
---




最近想要從youtube下載一個俄劇[《Не родись красивой》](https://www.youtube.com/playlist?list=PLHtqAN7MRo3jhIL0gaVNLtl2r7IJBM3N9)，總共有200個視頻，只要找到所有的鏈接，在vps上用youtube-dl下載一下，bypy upload到百度雲就行了，但是一個一個去複製粘貼鏈接太麻煩了。

視頻鏈接是有規律的，所有視頻鏈接基本都是這樣的

```
https://www.youtube.com/watch?v=n-e7vFWQiuk&amp;index=190
https://www.youtube.com/watch?v=Y1vfffLsAUM&amp;index=191
https://www.youtube.com/watch?v=GSAK9yGjRUs&amp;index=192
https://www.youtube.com/watch?v=DoLuw-Is-m0&amp;index=193
https://www.youtube.com/watch?v=AppQtK5f24k&amp;index=194
https://www.youtube.com/watch?v=_ep-5irLjb4&amp;index=195
https://www.youtube.com/watch?v=X7E3J5KHS8U&amp;index=196
https://www.youtube.com/watch?v=jdJedQVZs0I&amp;index=197
https://www.youtube.com/watch?v=aMdUNAy_L6Y&amp;index=198
https://www.youtube.com/watch?v=auMi6aw92zI&amp;index=199
https://www.youtube.com/watch?v=8pQlTRFR8zk&amp;index=200

```

我把那個網頁保存下來，可以在[此處](http://explorerlxz.github.io/images/regex-expression/index.html)找到，重命名爲index.html，之後輸入這條命令

```
cat index.html |  grep -o 'https://www.youtube.com/watch.*index=[[:digit:]]\{1,3\}'
```

相關的鏈接就都出來了，十分方便，這次不知爲何有重複的鏈接，使用一下uniq可以消除重複行。


```
cat index.html |  grep -o 'https://www.youtube.com/watch.*index=[[:digit:]]\{1,3\}' | uniq > download-link.txt
```
