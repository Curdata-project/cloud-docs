# API 参考

所有API需要通过API鉴权。

### 错误样例表

| code  | message                             | api                              |
| ----- | ----------------------------------- | -------------------------------- |
|     0 | 返回成功                             | all                              |
| 90000 | 处理代码中信息解析unwrap错误信息      | all                              |
| 90001 | json字段解析错误                     | all                              |
| 90002 | 文件打开读取错误                     | all                              |
| 90003 | 字符转换相关产生的错误返回            | all                              |
| 90004 | 作为数据库操作失败时返回              | all                              |
| 90005 | 数据库命令执行失败                   | all                              |
| 90006 | 额度转换金额不一致                   | all                              |
| 90007 | 回收数字货币查询不存在                |/api/quota/currency/recycle      |
| 90008 | 该用户数字货币id不存在                |/api/quota/currency/destroy      |
| 90009 | 当前数字货币状态不能销毁或删除        |/api/quota/currency/destroy       |
| 90010 | 获取金额计算出错                     | get /api/quota/detail            |

### API设计

#### 生成数字货币

HTTP 请求：`POST /api/qoutas`

> 此API也可以通过控制台进行图形化调用

请求示例：

```json
[
    {
        "value":100 ,
        "number": 50
	},
    {
        "value": 50,
        "number": 10
    }
]
```

响应示例：

```json
{
    "code": 0,
    "message": "success"
}
```
#### 发行数字货币

HTTP 请求： POST /api/external/exchange

请求示例：

```
{
    "currency_id":<String>, //数字货币id
    "target": "", // 货币接收者的身份标识
}
```

响应示例：

```
{
    "code": 0,
    "message": "success",
    "data": [
        "",
        "",   //发行的数字货币
        ""
    ] 
}
```
#### 查询数字货币
HTTP 请求：GET /api/quota/detail?begin_time=&end_time=2020-09-07T07:42:15.310888
```
(begin_time/end_time(可选参数))
```
> 此API也可以通过控制台进行图形化调用

响应示例：

```json
{
    "code": 0,
    "message": "success",
    "data": {
    "amount":"i64", //总金额
    "suspended_quota": "i64", //未发行
    "circulation_quota": "i64", //已发行
    "destroy_quota": "i64",  //已销毁
    "amount_day":[
       8004750,  //当天
       0,   //前一天
       1000050,
       0,0,0,0
    ],    //近7天单日生成总金额
    "circulation_day":[
       8004750,  //当天
       0,      //前一天
       1000050,
       0,0,0,0
    ]     //近7天单日已发行总金额  
    }
}
```
#### 查询数字货币信息
HTTP 请求：`GET /api/quota/detail/info?page=&count=&currency_id=&status=&begin_time=&end_time=&destroy_time=&destroy_end_time=`
```
(currency_id/status/begin_time/end_time(可选参数))
```
> 此API也可以通过控制台进行图形化调用

响应示例：

```json
{
    "code": 0,
    "message": "success",
    "data":{
      "total":i64,
      "inner":{
        "id":,
        "amount":,
	    "status":,
        "create_time":i64,
        "last_time": i64,
        }
    }
}
```
#### 数字货币回收
HTTP 请求：PUT /api/quota/currency/recycle

请求示例：

```
{
    currency: Vec<String>, //需要回收的一组数字货币交易体
}
```
响应示例：

```json
{
    "code": 0,
    "message": "success"
}
```

#### 数字货币销毁
HTTP 请求：delete /api/quota/currency/destroy

> 此API也可以通过控制台进行图形化调用

请求示例：

```
{
    currency_id: Vec<String>, //需要销毁货币id
}
```
响应示例：

```json
{
    "code": 0,
    "message": "success"
}
```

#### 数字货币删除
HTTP 请求：delete /api/quota/currency/delete

> 此API也可以通过控制台进行图形化调用

请求示例：

```
{
    currency_id: Vec<String>, //需要删除货币id
}
```
响应示例：

```json
{
    "code": 0,
    "message": "success"
}
```