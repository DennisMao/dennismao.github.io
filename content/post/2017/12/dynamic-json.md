+++
title = "使用Go语言处理动态JSON类型"
date = 2017-12-14T00:22:45+08:00
draft = false
slug = ""
categories = ["golang"]
tags = ["json"]
toc = true
+++

## 背景
在项目中有遇到JSON格式的数据流，对接的数据源其内容格式会有变化，但是内部会有字段表示其内容的类型，比如以下结构:
```
type DataJson struct {
	JsonType string `json:"json_type"`
	Data interface{} `json:"data"`
}
```
其中DataJson是固定每次数据流返回的JSON数据格式，但是其中的Data有以下几种可能：字符串、Int64数值、结构体对象。  
这种情况需要根据Json去进行判断。

## 生成动态类型
在Go语言中通过使用interface{}类型，我们可以很方便地把各种数据类型都使用同一个数据对象中进行数据传递。
动态类型生成方式很简单:
```
type DataJson struct {
      	JsonType string `json:"json_type"`
	Data interface{} `json:"data"`
}

func main(){
	//定义两个JSON数据对象
	var jsData1,jsData2 DataJson
	//String数据类型对象
	jsData1.JsonType = "string"
	jsData1.Data = "hello json"
	
	//Int64数据类型对象
	jsData2.JsonType = "int64"
	jsData2.Data = 123456789	
}
```

## 解析动态类型
### 方法一 整体二次反序列化
核心原理是通过新建一个对象，将能已知的数据对象先解析并获取到目标数据类型后再进行二次解析。
```
//目标类型
type DataJson struct {
        JsonType string `json:"json_type"`
        Data interface{} `json:"data"`	
}
//中间类型
type DataJsonType struct {
	JsonType string `json:"json_type"`
}

//字符串类型
type DataJsonTypeString struct {
	Data string `json:"data"`
}

func main(){
	//原始JSON数据
	var rawData = `
	{
	"type": "string",
	"data": "dynamite"
	}
	`

	buffer := []byte(rawData)
	//创建中间类型对象
	var md DataJsonType 
	err := json.Unmarshal(buffer,&md)
	if err != nil {
		log.Fatalln(err)
	}
	switch md.Type {
		case "string":
			var strData struct {
				DataJsonType
				DataJsonTypeString
			}
			err := json.Unmarshal(buffer,&strData)
			if err != nil {
				log.Fatalln(err)	
			}
			log.Println("data:",strData.Data)
		default :
			log.Fatalln("error: unsupported type")
	}
}
```


### 方法二 针对未知对象反序列化
核心原理是使用 *json.RawMessage，将未知类型的数据先保存到预开辟的内存空间中，等到确定类型后再进行二次解析
由于二次反序列化时只针对未知数据类型的数据进行处理，对比方法一将会更高效和节省内存消耗。
```go
package main

import (
	"encoding/json"
	"fmt"
	"log"
)

func main() {
	//基本结构
	type Color struct {
		Space string
		Point json.RawMessage // delay parsing until we know the color space
	}
	//节点结构1
	type RGB struct {
		R uint8
		G uint8
		B uint8
	}
	//节点结构2
	type YCbCr struct {
		Y  uint8
		Cb int8
		Cr int8
	}
	//模拟JSON序列化后数据
	var j = []byte(`[
		{"Space": "YCbCr", "Point": {"Y": 255, "Cb": 0, "Cr": -10}},
		{"Space": "RGB",   "Point": {"R": 98, "G": 218, "B": 255}}
	]`)
	//定义基本结构
	var colors []Color
	err := json.Unmarshal(j, &colors)
	if err != nil {
		log.Fatalln("error:", err)
	}
	//通过space变量来判断节点的目标类型
	for _, c := range colors {
		var dst interface{}
		switch c.Space {
		case "RGB":
			dst = new(RGB)
		case "YCbCr":
			dst = new(YCbCr)
		dafult :
			log.Fatalln("error: unsupported type")
		}
		//二次序列化
		err := json.Unmarshal(c.Point, dst)
		if err != nil {
			log.Fatalln("error:", err)
		}
		fmt.Println(c.Space, dst)
	}
}
```
