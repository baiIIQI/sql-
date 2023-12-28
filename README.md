# sql-
![597bcddff03b9bedeb3697953872ee2](https://github.com/baiIIQI/sql-/assets/155067886/90774c6b-91f8-417b-91c6-dea4abacf42a)
![dfc471ff7d9d96dbf0f27402b8a2678](https://github.com/baiIIQI/sql-/assets/155067886/9ce6810c-a846-423e-bef5-da332fdccb00)
1.上面两张图片，是我在一开始跟着学的资料，然后您看一下他的优化对吗
2.我在CSDN上看到like 的模糊优化，大致也是着个思路，这是网站https://blog.csdn.net/Martin_chen2/article/details/121407417

1.我遇到like and like 如何优化的问题
尝试：我先按着上面两个资料共同的方法，试了试，然后发现这个方法好像不对

您帮我看看我的尝试哪里出了问题，谢谢

以下是我的尝试
1.创建数据https://blog.csdn.net/cocoa_geforce/article/details/116234804#:~:text=MySQL%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AA100%E4%B8%87%E6%95%B0%E6%8D%AE%E9%87%8F%E7%9A%84%E8%A1%A8%E5%88%9B%E5%BB%BA%E8%A1%A8CREATE%20TABLE%20%60person%60%20%28%60id%60%20bigint%20%2820%29%20NOT%20NULL,NOT%20NULL%2C%20PRIMARY%20KEY%20%28%60id%60%29%29%20ENGINE%3DInnoDB%20DEFAULT%20CHARSET%3Dutf8mb4%3B%E5%88%9B%E5%BB%BA%E5%AD%98%E5%82%A8%E8%BF%87%E7%A8%8B_db2%E5%88%9B%E5%BB%BA%E4%B8%80%E5%BC%A0%E4%B8%80%E7%99%BE%E4%B8%87%E8%A1%A8，按照这个网站建立的
2尝试结果是下面图片：
![b6c807c47492c862da637ac8b074c70](https://github.com/baiIIQI/sql-/assets/155067886/10f00824-d210-4118-bbe4-239b36f52f76)
刚才的四个语句是我在id加上主键和复合索引（name,score）之后再执行的语句
这是四个语句的explain
![f8868c2f6f68b31f4d5a8fac74c738c](https://github.com/baiIIQI/sql-/assets/155067886/e4b59b03-da8f-45f5-97a0-c6932f9d8812)
然后我想问：第三个语句select name,score from person where id in (select id from person where locate('2',name)>0 and locate(2, score)>0) |
复合索引在底层是在索引也就是name,score的下面是主键的id 值，然后where 过滤出
id用主键索引底层直接在主键的id下面找出对应数据

第二个语句select name,score from person where locate('2',name)>0 and locate(2, score)>0 是直接用复合索引找出对应的主键id值，再回表去用id值去找数据

分析对吗，为什么第三个语句要慢很多 谢谢


