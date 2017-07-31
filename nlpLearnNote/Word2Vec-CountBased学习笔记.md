# word2vec: Count Based

> 参考资料：
>
> word2vec 正篇（上）：Count Based
<https://www.youtube.com/watch?v=mC9IwulM7D0>
>
> word2vec 正篇（中）：Skip Gram
<https://www.youtube.com/watch?v=EHqXB6P1PzA&t>


滑窗大小为5，以每个词分别为滑窗的中心词，去count在这个滑窗中每个词的个数。

以“I love you so much”为例：

![CountBased01](https://raw.githubusercontent.com/sunshinelu/LearnDiary/master/images/NLP/NLP.Word2Vec_CountBased_01.png)

新增一句话“I love her”之后，则变为：

![CountBased02](https://raw.githubusercontent.com/sunshinelu/LearnDiary/master/images/NLP/NLP.Word2Vec_CountBased_02.png)


如果滑窗大小为3，那么一个滑窗就要训练两次（如果滑窗为5，一个滑窗就要训练5次？？），我们有5个单词的话就要训练10次。

使用`One-Hot Encoding`对“I love you so much”表示结果为：

![SkipGram01](https://raw.githubusercontent.com/sunshinelu/LearnDiary/master/images/NLP/NLP.Word2Vec_SkipGram_01.png)

使用神经网络的方法预测每个单词附近的词。

`I`的输入为`[1,0,0,0,0]`，输出结果为`love`向量`[0，1，0，0，0]`


`love`的输入为`[0，1，0，0，0]`，输出为`I`（`[1,0,0,0,0]`）和`you`（`[0,0,1,0,0]`）

![SkipGram02](https://raw.githubusercontent.com/sunshinelu/LearnDiary/master/images/NLP/NLP.Word2Vec_SkipGram_02.png)
