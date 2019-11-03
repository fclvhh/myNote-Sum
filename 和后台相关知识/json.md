# 什么是JSON?

JSON是一种轻量级的数据交换格式 , 是不依赖于任何语言的 「文本格式」

- 参考连接:
  - [JSON是什么](http://json.org/)
  - [JSON知乎  参考质量回答](https://www.zhihu.com/question/34468150)



# JSON基于两种结构：

1. 对象  
   1. key, value   
2. 数据
   1. [value ]
3. 本质
   1. hash 数据结构

# JSON序列化和反序列化

- 序列化
  - 把「数据结构或者对象 」 转化成 其他格式的文本的过程  就称为序列化
  - 序列化的反向过程就称为 「反序列化」
- JSON的序列化函数 和反序列化函数
  - 序列化函数
    - JSON.parse()
    - 把「存储了JSON 文本格式的字符串」解析成 对象 或者数据结构
  - 反序列化函函数
    - JSON.stringify()
    - 把js对象 转换成 JSON格式文本的 字符串



# JSON和`js`的不同

- JSON是一门新的语言(抄袭了js)
- JSON中的基本类型,没有undefinde和symbol,也没有object中的function
  - 因为他就不是编程语言
- string必须是双引号
- key必须是string
- value 可是是任意类型
- `{string:value,}`



