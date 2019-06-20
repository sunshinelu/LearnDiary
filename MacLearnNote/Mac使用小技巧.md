# Mac使用小技巧


1. excel for mac 中替换多余的回车或换行符

As line-break is CHAR(13), you could try something like this (assuming your text is in A1 and you'll put edited text in B1)

    B1: =SUBSTITUTE(A1,CHAR(13),"")
or

    B1: =SUBSTITUTE(A1,CHAR(13)," ")