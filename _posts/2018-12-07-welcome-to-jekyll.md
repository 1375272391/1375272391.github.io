---
layout: post
title: Jekyll预置文档-译版
subtitle: 一个很棒的静态站点生成器.
author: Jeffrey
categories: jekyll
banner:
  video: https://vjs.zencdn.net/v/oceans.mp4
  loop: true
  volume: 0.8
  start_at: 8.5
  image: https://bit.ly/3xTmdUP
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold; text-decoration: underline"
  subheading_style: "color: gold"
tags: jekyll theme yat
sidebar: []
---

您可以在“_posts”目录中找到这篇文章. 编辑它并重新构建网站以查看您的更改. 您可以通过多种不同的方式重新构建网站, 但最常见的方法还是运行`jekyll serve`, 它会启动 Web 服务器并在文件更新时自动重新生成您的网站.

若要添加新帖子,只需在 `_posts` 目录中添加一个文件,该文件遵循约定`YYYY-MM-DD-名称-其他.ext`,并包含必要的前言.查看这篇文章的源文件,了解它是如何工作的.

## 第一节

Jekyll 还为代码片段提供了强大的支持:

{% highlight ruby %}
def print_hi(name)
puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

## 第二节

查看 [Jekyll docs][jekyll-docs] 以获取有关如何充分利用 Jekyll 的更多信息. 将所有错误/功能请求提交到 [Jekyll’s GitHub repo][jekyll-gh]. 如果你有问题，随时可以在 [Jekyll Talk][jekyll-talk] 上提问.

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]: https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

$ a \* b = c ^ b $

$ 2^{\frac{n-1}{3}} $

$ \int_a^b f(x)\,dx. $

```cpp
#include <iostream>
using namespace std;

int main() {
  cout << "Hello World!";
  return 0;
}
// prints 'Hi, Tom' to STDOUT.
```

```python
class Person:
  def __init__(self, name, age):
    self.name = name
    self.age = age

p1 = Person("John", 36)

print(p1.name)
print(p1.age)
```
