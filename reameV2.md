# 高防IP API接口说明书
## 目录

- [准备](#准备)
    - [域名](#域名)
    - [公共参数生成材料](#公共参数生成材料)
    - [生成签名](#生成签名)
    - [公共参数](#公共参数)
    - [code状态码说明](#code状态码说明)
    - [代码示例（以php为例）](#代码示例)
- [1 高防ip管理](#1高防ip管理)
    - [1.1高防IP购买](#1.1高防IP购买)
        - [1.1.1 服务套餐类型](#1.1.1服务套餐类型)
        - [1.1.2 购买接口](#1.1.2购买接口)
        - [1.1.3 升级接口](#1.1.3升级接口)           
        - [1.1.4 续费接口](#1.1.4续费接口)
        - [1.1.5 订单列表](#1.1.5订单列表)
    - [1.2高防IP列表](#1.2高防IP列表)
        - [1.2.1 获取高防IP列表](#1.2.1获取高防IP列表)
        - [1.2.2 删除高防IP](#1.2.2删除高防IP)
        - [1.2.3 转发规则列表](#1.2.3转发规则列表)  
        - [1.2.4 添加转发规则](#1.2.4添加转发规则)
        - [1.2.5 编辑转发规则](#1.2.5编辑转发规则)  
        - [1.2.6 删除转发规则](#1.2.6删除转发规则)
            

<h2 id='准备'>准备</h2>
<h4 id='域名'>域名</h4>

- dev-highip.ddos.com

<h4 id='公共参数生成材料'>公共参数生成材料</h4>

|参数名      |是否必须 |类型    |说明|
|:----:      |:---:|:-----: |:-----:   |
|api_key    |  是     |string  | 公钥，也是代理商身份的唯一标识，由网堤安全提供    |
|api_secret |  是     |string  | 私钥（切勿泄漏），由网堤安全提供    |



<h4 id='生成签名'>生成签名</h4>

 **a**. 将所有请求参数按键名进行排序，然后用‘&’符号对键值对拼接，如:a=1&b=2&c=3

 **b**. 将a步骤得到的字符串拼接上api_secret（私钥）后进行md5加密，得到signature


<h4 id='公共参数'>公共参数</h4>

所有接口的调用都必须以下这两个公共参数，下面接口中不再赘述

|参数名|是否必须|类型|说明|
|:----:    |:---:|:----: |:-----:   |
|api_key |  是 |string  | 公钥，也是代理商身份的唯一标识，由网堤安全提供    |
|signature |  是 |string  | 签名，具体细节请看下面的签名生成    |

<h4 id='code状态码说明'>code状态码说明</h4>

- 100001：	表示公钥为空或者公钥错误

- 100002：	签名为空或签名验证不通过

- -1：	    接口调用失败

- 0	：		调用成功

<h4 id='代码示例'>代码示例</h4>

```
（PHP代码）
$params=['a'=>1,'b'=>2,'c'=>3,'api_key'=>'xxxxxxxxx'];

ksort($params);  //参数按键名进行排序

$str=http_build_query($params); //将参数拼接成a=1&b=2... 的字符串

$signature = md5($str . '私钥');//得到签名

```

<h2 id='1高防IP管理'>1高防IP管理</h3>

<h3 id='1.1高防IP购买'>1.1高防IP购买</h3>
<h4 id='1.1.1服务套餐类型'>1.1.1服务套餐类型</h4>

##### 功能说明：

- 获取套餐列表

##### 请求URL：

- http://[域名]/api/agent/highIpProduct

##### 请求方式：

- GET

##### 格式：

- JSON 

##### 输入参数说明：

（1）公共参数

发送请求时必须传入公共参数，详见公共参数说明。

（2）私有参数

|参数名|是否必须|类型|说明|
|:----:    |:---:|:-----: |:-----:   |
|无 |  无 |无  | 无    |

##### 返回参数说明：

|参数名|描述|
|:----    |-----   |
|list |  高防IP套餐列表    |
|id |  套餐ID    |
|name |  套餐名称    |
|band |  防御峰值    |
|idc_band |  回源带宽    |
|price |  套餐对应价格    |
|product_id |  套餐ID    |
|month_price|单月价格|
|year_price|单年价格|
|msg |  信息说明    |
|code|  状态码    |


##### 返回示例:

****正确时返回示例****：

- JSON示例:

```

{
    "list": [
        {
            "id": 2,
            "name": "10",
            "band": "10",
            "idc_band": "50",
            "price": {
                "product_id": 2,
                "month_price": "7800.00",
                "year_price": "93600.00"
            }
        },
        {
            "id": 3,
            "name": "20",
            "band": "20",
            "idc_band": "50",
            "price": {
                "product_id": 3,
                "month_price": "13800.00",
                "year_price": "165600.00"
            }
        }
    ],
    "msg": "获取成功",
    "code": 0
}

```

**错误时返回示例：**
- JSON示例:

```
{
   
    "msg": "获取失败",
    "code": -1
}
```
<h4 id='1.1.2购买接口'>1.1.2购买接口</h4>

##### 功能说明：

- 购买高防IP服务

##### 请求URL：

- http://[域名]/api/agent/highIpOrder

##### 请求方式：

- POST

##### 格式：

- JSON

##### 输入参数说明:

（1）公共参数

发送请求时必须传入公共参数，详见公共参数说明。

（2）私有参数

|参数名|是否必须|类型|说明|
|:----: |:----:|:---:|:----:  |
|product_id|是|int|套餐ID|
|duration|是|int|购买时长|
|duration_type|是|int|时间类型，2-月，3-年|
|order_type|是|int|填写固定值 0|

* order_type参数说明：0-购买，1-升级，2-续费；这里是购买接口，传0即可。同理，升级与续费接口必须传相应的值，下面不在赘述


##### 返回参数说明：

|参数名|描述|
|:----:    |:-----:   |
|data |  订单详情    |
|id |  订单ID    |
|order_no |  订单编号    |
|type |  订单类型（0-新购，1-升级，2-续费）|
|instance_id | 高防IP的ID|
|product_id | 套餐ID |
|duration | 时长|
|duration_type | 时间类型 |
|status | 订单状态，1-订单处理中，3-已完成  |
|total_price | 总价格 |
|real_price | 实际价格 |
|create_timestamp | 订单创建时间 |
|product_end_timestamp | 服务到期时间 |
|msg |  信息说明    |
|code|  状态码    |

##### 返回示例:

****正确时返回示例****：

- JSON示例:

```

{
    "data": {
        "id": 327,
        "order_no": "PO32017071212225995040988",
        "type": 0,
        "instance_id": 77,
        "product_id": 2,
        "duration": 1,
        "duration_type": 2,
        "status": 3,
        "total_price": "7800.00",
        "real_price": "7800.00",
        "create_timestamp": 1499833379,
        "product_end_timestamp": 1502511780
    },
    "msg": "下单成功",
    "code": 0
}

```

**错误时返回示例：**
- JSON示例:

```
{
   
    "msg": "套餐时间参数不正确或该套餐类型不能续费",
    "code": -1
}
```

<h4 id='1.1.3升级接口'>1.1.3 升级接口</h4>

##### 功能说明：

- 升级高防IP服务

##### 请求URL：

- http://[域名]/api/agent/highIpOrder

##### 请求方式：

- POST

##### 格式：

- JSON

##### 输入参数说明:

（1）公共参数

发送请求时必须传入公共参数，详见公共参数说明。

（2）私有参数

|参数名|是否必须|类型|说明|
|:----: |:----:|:---:|:----:  |
|product_id|是|int|套餐ID|
|instance_id|是|int|高防IP的ID|
|order_type|是|int|值为 1|

##### 返回参数说明：

|参数名|描述|
|:----:    |:-----:   |
|data |  订单详情    |
|id |  订单ID    |
|order_no |  订单编号    |
|type |  订单类型（0-新购，1-升级，2-续费）|
|instance_id | 高防IP的ID|
|product_id | 套餐ID |
|duration | 时长（升级情况下为0）|
|duration_type | 时间类型（升级情况下为0） |
|status | 订单状态，1-订单处理中，3-已完成  |
|total_price | 总价格 |
|real_price | 实际价格 |
|create_timestamp | 订单创建时间戳 |
|product_end_timestamp | 服务到期时间戳 |
|msg |  信息说明    |
|code|  状态码    |

##### 返回示例:

****正确时返回示例****：

- JSON示例:

```
{
    "data": {
        "id": 327,
        "order_no": "PO32017071212225995040988",
        "type": 1,
        "instance_id": 77,
        "product_id": 2,
        "duration": 1,
        "duration_type": 2,
        "status": 3,
        "total_price": "7800.00",
        "real_price": "7800.00",
        "create_timestamp": 1499833379,
        "product_end_timestamp": 1502511780
    },
    "msg": "下单成功",
    "code": 0
}

```

**错误时返回示例：**
- JSON示例:

```
{
   
    "msg": "套餐不存在",
    "code": -1
}
```
<h4 id='1.1.4续费接口'>1.1.4 续费接口</h4>

##### 功能说明：

- 高防IP续费服务

##### 请求URL：

- http://[域名]/api/agent/highIpOrder

##### 请求方式：

- POST

##### 格式：

- JSON

##### 输入参数说明:

（1）公共参数

发送请求时必须传入公共参数，详见公共参数说明。

（2）私有参数

|参数名|是否必须|类型|说明|
|:----: |:----:|:---:|:----:  |
|product_id|是|int|套餐ID|
|duration | 是 | int | 时长 |
|duration_type | 是 | int | 时间类型 |
|instance_id|是|int|高防IP的ID|
|order_type|是|int|值为 2|

##### 返回参数说明：

|参数名|描述|
|:----:    |:-----:   |
|data |  订单详情    |
|id |  订单ID    |
|order_no |  订单编号    |
|type |  订单类型（0-新购，1-升级，2-续费）|
|instance_id | 高防IP的ID|
|product_id | 套餐ID |
|duration | 时长|
|duration_type | 时间类型 |
|status | 订单状态，1-订单处理中，3-已完成  |
|total_price | 总价格 |
|real_price | 实际价格 |
|create_timestamp | 订单创建时间戳 |
|product_end_timestamp | 服务到期时间戳 |
|msg |  信息说明    |
|code|  状态码    |

##### 返回示例:

****正确时返回示例****：

- JSON示例:

```

{
    "data": {
        "id": 327,
        "order_no": "PO32017071212225995040988",
        "type": 1,
        "instance_id": 77,
        "product_id": 2,
        "duration": 1,
        "duration_type": 2,
        "status": 3,
        "total_price": "7800.00",
        "real_price": "7800.00",
        "create_timestamp": 1499833379,
        "product_end_timestamp": 1502511780
    },
    "msg": "下单成功",
    "code": 0
}

```

**错误时返回示例：**
- JSON示例:
```
{
   
    "msg": "套餐不存在",
    "code": -1
}
```
<h4 id='1.1.5订单列表'>1.1.5 订单列表</h4>

##### 功能说明：

- 获取高防IP订单列表

##### 请求URL：

- http://[域名]/api/agent/highIpOrder

##### 请求方式：

- GET

##### 格式：

- JSON

##### 输入参数说明:

（1）公共参数

发送请求时必须传入公共参数，详见公共参数说明。

（2）私有参数

|参数名|是否必须|类型|说明|
|:----: |:----:|:---: |:----:  |
|page | 否 | int | 第几页 |
|nub | 否 | int | 每页数据条数 |
|type | 否 | int | 订单类型 0-新购，1-升级，2-续费 |
|kwd | 否 | string | 订单编号模糊搜索 |

##### 返回参数说明：

|参数名|描述|
|:----:    |:-----:   |
|list |  订单列表    |
|id |  订单ID    |
|order_no |  订单编号    |
|type |  订单类型（0-新购，1-升级，2-续费）|
|instance_id | 高防IP的ID|
|product_id | 套餐ID |
|duration | 时长|
|duration_type | 时间类型 |
|status | 订单状态，1-订单处理中，3-已完成  |
|total_price | 总价格 |
|real_price | 实际价格 |
|create_timestamp | 订单创建时间戳 |
|product_end_timestamp | 服务到期时间戳 |
|msg |  信息说明    |
|code|  状态码    |

##### 返回示例:

****正确时返回示例****：

- JSON示例:

```
{
    "list": [
        {
            "id": 327,
            "order_no": "PO32017071212225995040988",
            "type": 0,
            "instance_id": 77,
            "product_id": 2,
            "duration": 1,
            "duration_type": 2,
            "status": 3,
            "total_price": "7800.00",
            "real_price": "7800.00",
            "create_timestamp": 1499833379,
            "product_end_timestamp": 1502511780
        },
        {
            "id": 326,
            "order_no": "PO32017071212104222100988",
            "type": 0,
            "instance_id": 76,
            "product_id": 2,
            "duration": 1,
            "duration_type": 2,
            "status": 3,
            "total_price": "7800.00",
            "real_price": "7800.00",
            "create_timestamp": 1499832642,
            "product_end_timestamp": 1502511042
        }
    ],
    "msg": "获取成功",
    "code": 0
}

```

**错误时返回示例：**
- JSON示例:

```
{
   
    "msg": "套餐不存在",
    "code": -1
}
```

---
<h3 id='1.2高防IP列表'>1.2 高防IP列表</h3>服务套餐类型
<h4 id='1.2.1获取高防IP列表'>1.2.1 获取高防IP列表</h4>

##### 功能说明：

- 获取高防IP列表

##### 请求URL：

- http://[域名]/api/agent/highIp
- 
##### HTTP请求方式：

- GET

##### 格式：
- JSON

##### 输入参数说明：
（1）公共参数

发送请求时必须传入公共参数，详见公共参数说明。

（2）私有参数

|参数名|是否必须|类型|说明|
|:----:    |:---:|:-----: |:-----:   |
|kwd |  否 |string  | 模糊搜索ip的关键字    |


##### 返回参数说明：

|参数名|描述|
|:----:    |:-----:   |
|list |  高防IP列表    |
|id |  高防IP的ID    |
|high_ip |  高防IP    |
|order_id |  订单ID    |
|end_timestamp |  服务到期时间戳    |
|start_timestamp |  服务开始时间戳    |
|created_at |  高防IP创建时间    |
|updated_at |  高防IP更新时间    |
|msg |  信息说明    |
|code|  状态码    |


##### 返回示例:

****正确时返回示例****：

- JSON示例:

```
{
    "list": [
        {
            "id": 70,
            "high_ip": "1.1.1.10",
            "product_type": 1,
            "product_id": 3,
            "order_id": 301,
            "end_timestamp": 1505784693,
            "start_timestamp": 1497835893,
            "create_timestamp": 1497835893,
            "created_at": "2017-06-19 11:16:37",
            "updated_at": "2017-06-19 11:16:24"
        },
        {
            "id": 71,
            "high_ip": "1.1.1.12",
            "product_type": 1,
            "product_id": 2,
            "order_id": 305,
            "end_timestamp": 1500432851,
            "start_timestamp": 1497840851,
            "create_timestamp": 1497840851,
            "created_at": "2017-06-19 10:54:11",
            "updated_at": "2017-06-19 10:54:11"
        },
    ],
    "msg": "获取成功",
    "code": 0
}
```

**错误时返回示例：**
- JSON示例:

```
{
   
    "msg": "IP列表为空",
    "code": -1
}
```


##### 备注:

- 暂无

<h4 id='1.2.2删除高防IP'>1.2.2 删除高防IP</h4>

##### 功能说明：

- 删除高防IP列表，接口内部会判断是否合法删除，如“IP不属于用户”，“IP服务未到期”将无法删除。

##### 请求URL:

- http://[域名]/api/agent/highIp/[id]
 
##### HTTP请求方式:

- DELETE

##### 格式：

- JSON

##### 输入参数说明：
（1）公共参数

发送请求时必须传入公共参数，详见公共参数说明。

（2）私有参数

|参数名|是否必须|类型|说明|
|:----:    |:---:|:-----: |:-----:   |
|无 |  无 |无  | 无    |


##### 返回参数说明：

|参数名|描述|
|:----:    |:-----:   |
|msg |  信息说明    |
|code|  状态码    |


##### 返回示例:

**正确时返回示例:**

JSON示例：

```
{
    "msg": "删除成功",
    "code": 0
}
```

**错误时返回示例:**
- JSON示例:

```
{
    "msg": "删除失败",
    "code": -1
}

```

##### 备注:

- 暂无

<h4 id='1.2.3转发规则列表'>1.2.3 转发规则列表</h4>

##### 功能说明：

- 获取转发规则列表

##### 请求URL:

- http://[域名]/api/agent/highIpRule

##### HTTP请求方式:

- GET

##### 格式：
- JSON

##### 输入参数说明:
（1）公共参数

发送请求时必须传入公共参数，详见公共参数说明。

（2）私有参数

|参数名|是否必须|类型|说明|
|:----:    |:---:|:-----: |:-----:   |
|id |  是 |int  | 高防IP的ID    |
|kwd |  否 |string  | 精准搜索转发端口    |


##### 返回参数说明:

|参数名|描述|
|:----:    |:-----:   |
|list |  转发规则列表    |
|id |  转发规则的ID    |
|high_ip_id |  高防IP的ID   |
|relay_port |  转发端口    |
|source_port |  源端口    |
|source_ip |  源站IP，格式为“ip1,ip2”    |
|type |  规则类型 1-dnat，2-snat   |
|protocol |  转发协议   |
|create_timestamp |  创建时间戳   |
|created_at |  创建时间  |
|updated_at |  更新时间    |
|msg |  信息说明    |
|code|  状态码    |


##### 返回示例:

**正确时返回示例:**

JSON示例：

```
{
    "list": [
        {
            "id": 171,
            "high_ip_id": 70,
            "relay_port": 1714,
            "source_port": 1133,
            "source_ip": "1.4.2.5,1.4.3.4",
            "type": 1,
            "protocol": [
                "UDP",
                "TCP"
            ],
            "create_timestamp": 1499763196,
            "created_at": "2017-07-11 16:53:16",
            "updated_at": "2017-07-11 16:53:16"
        }
    ],
    "msg": "获取成功",
    "code": 0
}
```

**错误时返回示例:**
- JSON示例:
```
{
    "msg": "规则列表为空",
    "code": -1
}

```

##### 备注:

- 暂无

<h4 id='1.2.4添加转发规则'>1.2.4 添加转发规则</h4>

##### 功能说明：

- 添加转发规则列表，接口内部判断添加是否合法，如：“IP是否存在”，“规则最多为100条”，“转发端口是否已经存在”。

##### 请求URL:

- http://[域名]/api/agent/highIpRule

##### HTTP请求方式:

- POST

##### 格式：
- JSON

##### 输入参数说明:
（1）公共参数

发送请求时必须传入公共参数，详见公共参数说明。

（2）私有参数

|参数名|是否必须|类型|说明|
|:----:    |:---:|:-----: |:-----:   |
|id |  是 |int  | 高防IP的ID    |
|natPort |  是 |int  | 转发端口，不能为80，8080，443    |
|sourcePort |  是 |int  | 源端口    |
|rulesProtoal |  是 |string  | 转发协议，格式为“TCP”、“UDP”、“TCP&UDP”、“UDP&TCP”    |
|rulesDestPoolIp |  是 |string  | 源站IP，格式为“ip1,ip2”    |
|rulesRemark |  否 |string  | 规则备注    |


##### 返回参数说明:

|参数名|描述|
|:----:    |:-----:   |
|msg |  信息说明    |
|code|  状态码    |


##### 返回示例:

**正确时返回示例:**

JSON示例：

```
{
    "msg": "添加成功",
    "code": 0
}
```

**错误时返回示例:**

- JSON示例:
```
{
    "msg": "添加失败",
    "code": -1
}

```

##### 备注:

- 暂无


<h4 id='1.2.5编辑转发规则'>1.2.5 编辑转发规则</h4>

##### 功能说明：

- 编辑转发规则列表，接口内部判编辑是否合法，如：“转发规则是否存在”，“规则最多为100条”，“转发端口是否已经存在”。

##### 请求URL:

- http://[域名]/api/agent/highIpRule/[id]  （id为转发规则ID）

##### HTTP请求方式:

- PUT

##### 格式：
- JSON

##### 输入参数说明:
（1）公共参数

发送请求时必须传入公共参数，详见公共参数说明。

（2）私有参数

|参数名|是否必须|类型|说明|
|:----:    |:---:|:-----: |:-----:   |
|id |  是 |int  | 转发规则的ID    |
|natPort |  是 |int  | 转发端口，不能为80，8080，443     |
|sourcePort |  是 |int  | 源端口  |
|rulesProtoal |  是 |string  | 转发协议，格式为“TCP”、“UDP”、“TCP&UDP”、“UDP&TCP”    |
|rulesDestPoolIp |  是 |string  | 源站IP，格式为“ip1,ip2”    |
|rulesRemark |  否 |string  | 规则备注    |


##### 返回参数说明:

|参数名|描述|
|:----:    |:-----:   |
|msg |  信息说明    |
|code|  状态码    |


##### 返回示例:

**正确时返回示例:**

JSON示例：

```
{
    "msg": "修改成功",
    "code": 0
}
```

**错误时返回示例:**
- JSON示例:

```
{
    "msg": "修改失败",
    "code": -1
}

```

##### 备注:

- 暂无

<h4 id='1.2.6删除转发规则'>1.2.6 删除转发规则</h4>

##### 功能说明：

- 删除转发规则，内部接口判读是否合法删除，如“规则不存在”将无法删除。

##### 请求URL:

- http://[域名]/api/agent/highIpRule/[id]  （id为转发规则ID）

##### HTTP请求方式:

- DELETE

##### 格式：
- JSON

##### 输入参数说明:
（1）公共参数

发送请求时必须传入公共参数，详见公共参数说明。

（2）私有参数

|参数名|是否必须|类型|说明|
|:----:    |:---:|:-----: |:-----:   |
|无 |  无 |无  | 无    |



##### 返回参数说明:

|参数名|描述|
|:----:    |:-----:   |
|msg |  信息说明    |
|code|  状态码    |


##### 返回示例:

**正确时返回示例:**

JSON示例：

```
{
    "msg": "删除成功",
    "code": 0
}
```

**错误时返回示例:**
- JSON示例:

```
{
    "msg": "删除失败",
    "code": -1
}

```

##### 备注:

- 暂无
 
