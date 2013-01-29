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

	GET http://www.esaleschina.com/api/sign-out.php
	
返回Json格式的数据

	{
		"status": 1 ,      // 退出成功：1，退出失败：0
		"error_msg": ""    // 失败的提示信息，无则为""
	}


#获取 Specails 产品列表

> 用户点击底部菜单“Specails”,查看 Specails 产品列表

	GET http://www.esaleschina.com/api/specails.php
	产品图片:http://{Domain}.esaleschina.com/{HtmlPath}/{Image}

返回Json格式的数据

	{
		"status": 1 ,      // 获取成功：1，获取失败：0
		"error_msg": ""    // 例如一共就10页，客户端提交请求11页，则返回错误提示信息
		"msg": [
			{
				Id: 	"1",
				NameAlias:	"iPhone-5", //html静态页文件名
				Name: "xxxx" //产品名称
				Image:	"free-shipping-iphone-5-case-iphone-5-skin-colorful-translucent-18242-m.jpg",
				UnitPrice: "$230.0", //单价
				UnitPriceDiscount: "$110.00", //如果没有特价，就留空				
				SaveRate: "20%" //优惠比率
				Domain: "photo" //图片域名
				HtmlPath: "202" //产品静态html存放路径 
				DHL_EMS_FreeShipping: "1" //0:Free Shipping 1:DHL/EMS Free Shipping
			},
			{},{},{}...
		]
	}

#获取所有 Catelogs 列表

> 用户点击底部菜单“Catelogs”,查看 所有分类

	GET http://www.esaleschina.com/api/category.php
	- parame par_id //父类Id 0时返回第一层及第二层 依次类推
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

	GET http://www.esaleschina.com/api/search-item-by-category.php

	- parame catelog_id string 该分类的 id
	- parame s string 排序条件 best-match / lowest-price / highest-price / recently-listed
	- parame page string 页码
	- 产品图片:http://{Domain}.esaleschina.com/{Id}/{Image}-s.jpg
	
返回Json格式的数据

	{
		"status": 1 ,      // 获取成功：1，获取失败：0
		"error_msg": ""    // 例如一共就10页，客户端提交请求11页，则返回错误提示信息
		"msg": [
			{
				Id: 	"1",
				NameAlias:	"iPhone-5", //html静态页文件名
				Name: "xxxx" //产品名称
				Image:	"free-shipping-iphone-5-case-iphone-5-skin-colorful-translucent-18242-m.jpg",
				UnitPrice: "$230.0", //单价
				UnitPriceDiscount: "$110.00", //如果没有特价，就留空				
				SaveRate: "20%" //优惠比率
				Domain: "photo" //图片域名
				HtmlPath: "202" //产品静态html存放路径 
				DHL_EMS_FreeShipping: "1" //0:Free Shipping 1:DHL/EMS Free Shipping
			},
			{},{},{}...
		]
	}
	
#搜索-关键字列表

> 用户点击搜索，返回查询结果列表

	GET http://www.esaleschina.com/api/search-keyword-list.php

	- parame key_word string 搜索的关键词
	
返回Json格式的数据

	{
		"status": 1 ,      // 获取成功：1，获取失败：0
		"error_msg": ""    // 例如一共就10页，客户端提交请求11页，则返回错误提示信息
		"msg": [
			{
				Name: "xxxx" //关键字
			},
			{},{},{}...
		]
	}
	
#搜索-产品列表

> 用户点击搜索，返回查询结果列表

	GET http://www.esaleschina.com/api/search-item-by-keyword.php

	- parame key_word string 搜索的关键词
	- parame s string 排序条件 best-match / lowest-price / highest-price / recently-listed
	- parame page string 页码
	- 产品图片:http://{Domain}.esaleschina.com/{Id}/{Image}-s.jpg
	
返回Json格式的数据

	{
		"status": 1 ,      // 获取成功：1，获取失败：0
		"error_msg": ""    // 例如一共就10页，客户端提交请求11页，则返回错误提示信息
		"msg": [
			{
				Id: 	"1",
				NameAlias:	"iPhone-5", //html静态页文件名
				Name: "xxxx" //产品名称
				Image:	"free-shipping-iphone-5-case-iphone-5-skin-colorful-translucent-18242-m.jpg",
				UnitPrice: "$230.0", //单价
				UnitPriceDiscount: "$110.00", //如果没有特价，就留空				
				SaveRate: "20%" //优惠比率
				Domain: "photo" //图片域名
				HtmlPath: "202" //产品静态html存放路径 
				DHL_EMS_FreeShipping: "1" //0:Free Shipping 1:DHL/EMS Free Shipping
			},
			{},{},{}...
		]
	}

#产品详细页

> 用户点击搜索，返回查询结果列表

	GET http://www.esaleschina.com/api/item-detail.php

	- parame prod_id string 搜索的关键词	
	- 产品小图:http://{Domain}.esaleschina.com/{Id}/{Image_m}
	- 产品大图:http://{Domain}.esaleschina.com/{Id}/{Image_l}
	
返回Json格式的数据

	{
		"status": 1 ,      // 获取成功：1，获取失败：0
		"error_msg": ""    // 例如一共就10页，客户端提交请求11页，则返回错误提示信息
		"msg": [
			{
				Id: 	"1",
				NameAlias:	"iPhone-5", //html静态页文件名
				Name: "xxxx" //产品名称
				Image_m: [   //产品小图片
					{Image1},
					{},{},{}
				],
				Image_l: [   //产品大图片
					{Image1},
					{},{},{}
				],
				UnitPrice: "$230.0", //单价
				UnitPriceDiscount: "$110.00", //如果没有特价，就留空				
				SaveRate: "20%" //优惠比率
				Qtys: [		//数量串
					{1},{2},{4} 
				]
				Prices: [	//价格串,折后价
					{1},{2},{4} 
				]
				Description: "xxxx" //产品详细描述
				Domain: "photo" //图片域名
				HtmlPath: "202" //产品静态html存放路径 
				DHL_EMS_FreeShipping: "1" //0:Free Shipping 1:DHL/EMS Free Shipping
			},
			{},{},{}...
		]
	}

#购物车:获取与回写

> 用户点击搜索，返回查询结果列表

	GET http://www.esaleschina.com/api/shopping-cart.php

	- parame t string 类型 1:获取 2:回写
	- parame j string 回写购物车json串	
	
返回Json格式的数据

	{
		"status": 1 ,      // 获取成功：1，获取失败：0
		"error_msg": ""    // 例如一共就10页，客户端提交请求11页，则返回错误提示信息
		"msg": [
			{
				Id: 	"1", //产品ID
				Qty:	"1", //数量
				Price: "230" //单价
				Name:	"xxx", //产品名称
				aQtys: "1,2,3,5,7", //数量串
				aDiscountPrices: "110.00,108.00,105.00,103.00,99.00", //价格串				
				Weight: "2.5" //重量
				L: "50" //长度
				W: "30" //宽度
				H: "20" //高度
				Attributes: "xxxx" 配件串
			},
			{},{},{}...
		]
	}
