# -zhihu-crawl-

知乎爬虫，爬取一个问题下所有的回答。

原 repo： https://github.com/visionshao/-zhihu-crawl- 

原文： https://zhuanlan.zhihu.com/p/78552777 

感谢原作者 visionshao 发现并实现的优秀爬取方法，关于原理请参见原文。

这里仅记录使用方法，作为备用。

## 尝试使用

直接执行 `python3 main.py`，如果你已经装好了代码所需要的包，即可在命令行中得到问题 [神经网络为什么可以（理论上）拟合任何函数？]( https://www.zhihu.com/question/268384579 ) 的结果。

![image-20200523145437349](C:\Users\15781\AppData\Roaming\Typora\typora-user-images\image-20200523145437349.png)

## 使用方法

以问题 [你的择偶标准是怎样的？]( https://www.zhihu.com/question/275359100) 为例，我们可以爬取到这个问题下面回答中的文字。

0. 环境需求

   - chrome 浏览器

   - python，并装有代码所需的包

1. 进入问题页面

   ![image-20200523143306815](C:\Users\15781\AppData\Roaming\Typora\typora-user-images\image-20200523143306815.png)

2. ctrl+shift+i 进入网页检查，进入 NetWork 选项卡，进入 XHR 选项卡。

   ![image-20200523143840945](C:\Users\15781\AppData\Roaming\Typora\typora-user-images\image-20200523143840945.png)

3. 向下滑动网页（可以直接拖到最下面），直到开始加载新答案。右侧会出现许多 XHR 对象。

   ![image-20200523144209548](C:\Users\15781\AppData\Roaming\Typora\typora-user-images\image-20200523144209548.png)

4. 从 XHR 中选中第一个 Name 前缀为 answer?include=data 的对象，在右侧进入 Preview 选项卡，点开 paging 项，复制 previous 的值。

   ![image-20200523144534071](C:\Users\15781\AppData\Roaming\Typora\typora-user-images\image-20200523144534071.png)

5. 比如这里得到的值是

    ```
   https://www.zhihu.com/api/v4/questions/275359100/answers?include=data%5B%2A%5D.is_normal%2Cadmin_closed_comment%2Creward_info%2Cis_collapsed%2Cannotation_action%2Cannotation_detail%2Ccollapse_reason%2Cis_sticky%2Ccollapsed_by%2Csuggest_edit%2Ccomment_count%2Ccan_comment%2Ccontent%2Ceditable_content%2Cvoteup_count%2Creshipment_settings%2Ccomment_permission%2Ccreated_time%2Cupdated_time%2Creview_info%2Crelevant_info%2Cquestion%2Cexcerpt%2Crelationship.is_authorized%2Cis_author%2Cvoting%2Cis_thanked%2Cis_nothelp%2Cis_labeled%2Cis_recognized%2Cpaid_info%2Cpaid_info_content%3Bdata%5B%2A%5D.mark_infos%5B%2A%5D.url%3Bdata%5B%2A%5D.author.follower_count%2Cbadge%5B%2A%5D.topics&limit=5&offset=0&platform=desktop&sort_by=default 
    ```

   用这个值替换 main.py 代码中 start_url 的值。

6. 执行这个 python 文件即可开始爬取得到内容。

   ![image-20200523145125363](C:\Users\15781\AppData\Roaming\Typora\typora-user-images\image-20200523145125363.png)

## 注意点

1. 对于回答极多的问题，建议将输出重定向到文件中

   ```shell
   python3 main.py >> result.txt
   ```

2. 这个爬虫只会得到回答中的文字并忽略换行，最后得到的结果是一个回答一行。可以用于关键词查找或数据分析。（择偶标准问题中排行第一的答案通过爬虫与数据分析得到了 1.5 万的赞。）（如果是只想看图片的绅士可以找找别的爬虫）

3. 有时候排行前几的回答会出现乱码，不过不会影响之后的答案。

4. 对于回答极多的问题或多次重复爬取，爬取过程中可能会被知乎限制（需要输入验证码），可以考虑添加代理或过一段时间再爬。

5. 再次感谢原作者 visionshao 发现并实现的优秀爬取方法，关于原理请参见最上方的原文链接。