+++
title = "Go爬虫初探"
date = 2017-09-26T23:29:00+08:00
draft = false
slug = ""
categories = ["使用手记","golang"]
tags = ["网络爬虫","正则"]
toc = true
+++

## 背景
前些天用Python实践了一下爬虫功能，强大的requests库、re库、beautifulsoup库和scrapy框架以及其附属的分布式数据库配套库，开发起来十分高效。对于复杂的爬虫规则和验证规则，Python的相关生态也能提供很好的解决方案。回到golang，也想尝试是否能用go语言也构建一个基本爬虫，看实现上会有多大差异。

## 目标功能
获取 **http://razil.cc** 首页上的网站名称元素内容。  
目标内容为**“后端小筑”**  

## 基本结构
由于是基本的定向爬虫，类比Python的结构是  
requests库 + re库  
即基本请求后处理转码，再通过正则获得需要的数据  
在go实现的结构如下:  

+   [net/http]  基本请求   
+   [mahonia] 第三方库实现编码转换  
+   [regexp]  正则匹配数据  

## 代码实现

```go
package main

import (
	"errors"
	"fmt"
	"io/ioutil"
	"net/http"
	"regexp"

	encodingConvert "github.com/DennisMao/mahonia"
)

func SimpleScrapy(url string) (string, error) {
	//http请求
	resp, err := http.Get(url)
	if err != nil {
		return "", err
	}

	//获取响应数据
	if resp.StatusCode != 200 {
		return "", errors.New("resp statuscode = :" + fmt.Sprint(resp.StatusCode))
	}

	respBody, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		return "", nil
	}

	//编码转换
	content := encodingConvert.NewEncoder("utf-8").ConvertString(string(respBody))
	//正则
	re, _ := regexp.Compile(`\<a.+a\>`)

	txts := re.FindAllString(content, 10) //获取到所有 <a>标签
	if len(txts) == 0 {
		return "", nil
	}

	reClass, _ := regexp.Compile(`class=\"site-header-title\"`) //筛选 <a>标签中 class="site-header-title" 的标签
	for _, txt := range txts {
		if reClass.MatchString(txt) {
			//返回数据
			reTitle, _ := regexp.Compile(`\>.+\<`)  //获取元素中的内容
			titleRaw := reTitle.FindString(txt)
			return titleRaw[1 : len(titleRaw)-1], nil
		}
	}
	return "", nil
}

func main() {
	url := "http://razil.cc"
	title, err := SimpleScrapy(url)
	if err != nil {
		fmt.Println(err.Error())
	}
	fmt.Println("网站Title = ", title)
}
```
 

打印输出:
![simpleScrapy输出结果](http://ovfxonyuf.bkt.clouddn.com/2017-10-gospider-1.png)


## 总结
通过初探，可以了解使用Go基本库可以轻松进行爬虫的编写。与Python相比,Go语言没有那么多丰富的库供我们调用，但是通过对Python相关库的理解，我们也可以自己用Go来“翻译”相关的库。另外由于语言异常处理的风格不同，Go这块的代码里会稍多一些。  
项目地址: [https://github.com/DennisMao/gospider](https://github.com/DennisMao/gospider)
