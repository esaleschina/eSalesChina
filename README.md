#备注

所有 API 都返回 json 格式的数据。

  例如产品的类别，一共有 4层，那么 PHP 中表示产品类别的数组形式为：

	$catelog = array(
		"Mobile Phone" => array(
			"iPhone" => array(
				"iPhone 4",
				"iPhone 5"
			),
			"Nokia" => array()
		),
		"NoteBook" => array(),
		"Cars" => array(),
	);
	
	那么应该返回的数据是：
	
	echo json_encode($catelog);


#登录
用户使用 eSalesChina 账号登录

	POST http://www.esaleschina.com/api/sign-in.php

	- parame username string 用户名
	- parame password string 密码

返回Json格式的数据

	{
		"status": 1 ,      // 登录成功：1，登录失败：0
		"error_msg": ""    // 失败的提示信息，无则为""
		"msg": {
			"Id": "1",   //用户id
			"Email": "xx" //用户名
			"Level": "1/2" //级别 Buyer=1 Reseller=2
			"FirstName": "Alex" //名字
			"LastName": "Ke" //姓
			"Check": "0/1" //是否审核 目前不用做判别 以后拓展
		}
	}

#注册

点击注册，直接在手机浏览器打开注册的页面，如果需要在手机端做注册功能，需要补充 API.

#退出登录
用户退出 eSalesChina 账号登录

	GET http://www.esales.com/xxx
	
返回Json格式的数据

	{
		"status": 1 ,      // 退出成功：1，退出失败：0
		"error_msg": ""    // 失败的提示信息，无则为""
	}


#获取 HOT 产品列表

> 用户点击底部菜单“HOT”,查看 HOT 产品列表

	GET http://www.esales.com/xxx

	- parame page string 页码

返回Json格式的数据

	{
		"status": 1 ,      // 获取成功：1，获取失败：0
		"error_msg": ""    // 例如一共就10页，客户端提交请求11页，则返回错误提示信息
		"msg": [
			{
				product_id: 	"1",
				product_title:	"iPhone 5",
				product_description: "xxxx"
				product_price:	"$230.00",
				product_special_offer:	"$110.00", //如果没有特价，就留空
				product_save: "20.00 %", //如果没有，就留空
				product_shipping: "DHL/EMS FREE SHIPPING"
			},
			{},{},{}...
		]
	}

#获取所有 catelog 列表

> 用户点击底部菜单“catelog”,查看 所有分类

	GET http://www.esaleschina.com/api/category.php
	分类图片:http://usa.esaleschina.com/categories/{Id}/{Image}-s.jpg	


返回Json格式的数据

	{
		"status": 1 ,      // 获取成功：1，获取失败：0
		"error_msg": ""    // 错误提示信息
		"msg": [
			{
				Id: 	"10",
				Name:	"Mobile Phone",
				Domain: "photo",
				Image: "mobile-phone",
				SubCategory:[
					{
						Id: 	"111",
						Name:	"iPhone",
						Domain: "photo",
						Image: "iphone"
						SubCategory:[
							{
								Id: 	"1111",
								Name:	"iPhone 4",
								Domain: "photo"
								Image: "iphone-4"
							},
							{
								Id: 	"112",
								Name:	"iPhone 5",
								Domain: "photo"
								Image: "iphone-5"
							},
							{},
							{}...
						]
					},
					{
						Id: 	"112",
						Name:	"iPhone2",
						Domain: "photo2",
						Image: "iphone2",
						SubCategory:{....}
					},
					{},
					{}...
				]
			},
			{
				Id: 	"11",
				Name:	"Memery Cards",
				Domain: "photo",
				Image: "memory-cards",
				SubCategory: {...}
			},
			{},
			{},
			{}...
		]
	}


#查看一个分类的产品

> 用户点击某个分类，查看该分类的 产品列表

	GET http://www.esales.com/xxx

	- parame catelog_id string 该分类的 id
	- parame page string 页码
	- 产品图片:http://{Domain}.esaleschina.com/{Id}/{Image}-s.jpg
	
返回Json格式的数据

	{
		"status": 1 ,      // 获取成功：1，获取失败：0
		"error_msg": ""    // 例如一共就10页，客户端提交请求11页，则返回错误提示信息
		"msg": [
			{
				product_id: 	"1",
				product_title:	"iPhone 5",
				product_description: "xxxx"
				product_price:	"$230.00",
				product_special_offer:	"$110.00", //如果没有特价，就留空
				product_save: "20.00 %", //如果没有，就留空
				product_shipping: "DHL/EMS FREE SHIPPING"
			},
			{},{},{}...
		]
	}
	
#搜索

> 用户点击搜索，返回查询结果列表

	GET http://www.esales.com/xxx

	- parame key_word string 搜索的关键词
	- parame page string 页码
	
返回Json格式的数据

	{
		"status": 1 ,      // 获取成功：1，获取失败：0
		"error_msg": ""    // 例如一共就10页，客户端提交请求11页，则返回错误提示信息
		"msg": [
			{
				product_id: 	"1",
				product_title:	"iPhone 5",
				product_description: "xxxx"
				product_price:	"$230.00",
				product_special_offer:	"$110.00", //如果没有特价，就留空
				product_save: "20.00 %", //如果没有，就留空
				product_shipping: "DHL/EMS FREE SHIPPING"
			},
			{},{},{}...
		]
	}
