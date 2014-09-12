---
layout: post
title:  "基于Scrapy的京东爬虫"
category: python
tags: python scrapy
---

出于兴趣爱好原来就做过爬虫,从C到python,再到scrapy,一开始也是因为做爬虫才慢慢了解python,才知道有这么优雅的语言,废话不多说了.看一下是怎么爬取的.

前期工作做了很多,学习了twisted,scrapy.京东的网页价格,评论是动态,怎么爬?google了一大堆什么,又抓包,发现关于评论的包不能正常的抓取,可能是设的cookies认证,没深入了解这方面的知识,发觉碰到一块难题.

就在昨天,偶尔浏览到了京东的wap页面,我勒个大曹啊,wap页面都是静态的,而且和网页端的内容是一致更新的,终于送了一口气,三下五除二用了两个多小时写完了这个爬虫,今天早上正好到实验室开始抓取,抽空写个blog.

项目地址:https://github.com/taizilongxu/scrapy_jingdong

###思路

同学只要价格,商品名和评论数,所以还算简单,最后要加个商品ID,方便去重

####第一个页面

京东有一个商品全部分类的页面如下 http://wap.jd.com/category/all.html

![](https://raw.githubusercontent.com/taizilongxu/taizilongxu.github.io/master/img/2014-09-07 09:17:58 的屏幕截图.png)

其中红色的和后面的更多指向的是一个页面,只要抓取一个就可以到下一个页面了,进入第一个邪恶的服饰内衣看看.

```python
    def parse(self, response):
        '获取全部分类商品'
        req = []
        for sel in response.xpath('/html/body/div[5]/div[2]/a'):
            name = sel.xpath('text()').extract()
            href = sel.xpath('@href').extract()
            for i in href:
                if 'category' in i:
                    url = "http://wap.jd.com" + i
                    # print url
                    r = Request(url, callback=self.parse_category)
                    req.append(r)
        return req
```

####第二个页面

![](https://raw.githubusercontent.com/taizilongxu/taizilongxu.github.io/master/img/2014-09-07 09:20:42 的屏幕截图.png)

这个页面就是服饰内衣的页面了,我们抓取每个页面的蓝色字体的小分类,保存进req

```python
    def parse_category(self,response):
        '获取分类页'
        req = []
        for sel in response.xpath('/html/body/div[5]/div/a'):
            href = sel.xpath('@href').extract()
            for i in href:
                url = "http://wap.jd.com" + i
                # print url
                r = Request(url, callback=self.parse_list)
                req.append(r)
        return req
```

####第三个页面

![](https://raw.githubusercontent.com/taizilongxu/taizilongxu.github.io/master/img/2014-09-07 09:23:32 的屏幕截图.png)

这个就是列表页面了,我们沿着这个页面可以抓取所有商品的url,要说的就是要把下一页也放到parse_list里进行循环

```python
    def parse_list(self,response):
        '分别获得商品的地址和下一页地址'
        req = []

        '下一页地址'
        next_list = response.xpath('/html/body/div[21]/a[1]/@href').extract()
        if next_list:
            url = "http://wap.jd.com" + next_list[0]
            r = Request(url, callback=self.parse_list)
            req.append(r)
            
        '商品地址'
        for sel in response.xpath('/html/body/div[contains(@class, "pmc")]/div[1]/a'):
            href = sel.xpath('@href').extract()
            for i in href:
                url = "http://wap.jd.com" + i
                # print url
                r = Request(url, callback=self.parse_product)
                req.append(r)
        return req
```

####第四个页面

![](https://raw.githubusercontent.com/taizilongxu/taizilongxu.github.io/master/img/2014-09-07 09:25:36 的屏幕截图.png)

在这里抓取title,price和id,id就是 http://wap.jd.com/product/1268172347.html 最后的数字.

把上面网址中的product改成comments,就可以抓取评论页了

```python
    def parse_product(self,response):
        '商品页获取title,price,product_id'
        url = re.sub('product','comments',response.url)
        r = Request(url,callback=self.parse_comments)

        title = response.xpath('//title/text()').extract()[0][:-6]
        price = response.xpath('/html/body/div[4]/div[4]/font/text()').extract()[0]
        product_id = response.url.split('/')[-1][:-5]


        item = TutorialItem()
        item['title'] = title
        item['price'] = price
        item['product_id'] = product_id
        r.meta['item'] = item
        print title,price,product_id
        return r
```


####第五个页面

![](https://raw.githubusercontent.com/taizilongxu/taizilongxu.github.io/master/img/2014-09-07 09:29:07 的屏幕截图.png)

这个页面也没什么好说的,把好中差评论加一起就是商品总数了,然后返回item([如何爬取属性在不同页面的item呢？](http://scrapy-chs.readthedocs.org/zh_CN/latest/topics/request-response.html#topics-request-response-ref-request-userlogin)scrapy里面介绍的很详细了

```python
    def parse_comments(self,response):
        '获取商品comment'
        comment_5 = response.xpath('/html/body/div[4]/div[2]/a[1]/font[2]/text()').extract()
        comment_3 = response.xpath('/html/body/div[4]/div[2]/a[2]/font/text()').extract()
        comment_1 = response.xpath('/html/body/div[4]/div[2]/a[3]/font/text()').extract()
        comment = comment_5 + comment_3 + comment_1
        print comment
        totle_comment = sum([int(i.strip()) for i in comment])
        item = response.meta['item']
        item['comment'] = totle_comment
        print totle_comment
        return item
```

至此整个爬虫完毕,剩下就是运行了,正在等待结果


