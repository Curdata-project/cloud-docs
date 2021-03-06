# API 参考

所有API需要通过API鉴权。

### API设计

### 错误样例表

| code  | message                             | api                              |
| ----- | ----------------------------------- | -------------------------------- |
|     0 | 成功返回                             | all                              |
| 90001 | 返回部分unwrap错误信息               | post /api/public/transaction     |
| 90002 | 读取配置文件打开失败                  | post /api/public/transaction     |
| 90003 | 字符转换相关产生的错误返回            | post /api/public/transaction      |
| 90004 | 数字货币交易体检查错误                | post /api/public/transaction     |
| 90005 | 数字货币交易状态不满足交易            | post /api/public/transaction     |
| 90006 | 用户信息错误，无此货币                | get /api/public/currency/info    |

#### 自有证书管理


#### 进行交易

HTTP 请求：`POST /api/public/transaction`

请求示例：

```json
{
    "curr_transaction": "String",//数字货币交易体十六进制数
}
```

响应示例：

```json
{
    "code": 0,
    "message": "success",
    "data": [
        "currency",//转移之后的数字货币
        "currency"
    ]
}
```

#### 获取货币信息

HTTP 请求：`GET /api/public/currency/info?currency_id=`

> 此API也可以通过控制台进行图形化调用

响应示例：

```json
{
    "code": 0,
    "message": "success",
    "data": {
        "currency_id": "String", //转移之后的数字货币
        "transaction_id": "String",
        "destroytrans_id": "String", //销毁交易编号
        "status": "String",
        "owner": "String",   //
        "amount": "i64",
        "create_time": "i64",
        "late_time": "i64",  //如果status=destroy状态则该时间为销毁时间
    }
}
```
#### 获取交易信息

HTTP 请求：`GET /api/public/transaction/info?transaction_id=`

> 此API也可以通过控制台进行图形化调用

响应示例：

```json
{
    "code": 0,
    "message": "success",
    "data": {
        "transaction_id": "String", //本次交易编号
        "trans_amount": "u64",  //交易金额
        "transaction_time": "i64",   //交易时间
        "input": "Vec<(String, u64)>", //货币编号：金额
        "output":"Vec<(String, u64)>"
    }
}
```

#### 获取数字货币列表信息

HTTP 请求：`GET /api/public/currency/list?page=i64&count=i64&currency_id=String&status=String&amount=i64&create_time=2020-09-15T06:30:46&create_end_time=&destroy_time=&destroy_end_time=`
//(除page与count为必须参数其余都可选，后面四个参数时间格式一致)

> 此API也可以通过控制台进行图形化调用

响应示例：

```json
{
    "code": 0,
    "message": "success",
    "data":{
        "total": "i64",
        "inner":[
            {
                "currency_id": "String", //数字货币编号
                "amount": "i64",  //交易金额
                "create_time": "i64",   //交易时间
                "status": "String", //货币状态
                "late_time":"i64" //status=destroy时为销毁时间
            },
        ]
    }
}
```
#### 获取交易列表信息

HTTP 请求：`GET /api/public/transaction/list?page=i64&count=i64&transaction_id=String&amount=i64&begin_time=2020-09-15T06:30:46&end_time=`
//(除page与count为必须参数其余都可选，后面两个参数时间格式一致)

> 此API也可以通过控制台进行图形化调用

响应示例：

```json
{
    "code": 0,
    "message": "success",
    "data": {
        "total": "i64",
        "inner":[
            {
                "transaction_id": "String", //本次交易编号
                "amount": "i64",  //交易金额
                "create_time": "i64",   //交易时间
            },
        ]
    }
}
```

#### 投放系统数字货币统计
HTTP 请求：`GET /api/public/currency/statis?begin_time=&end_time=2020-09-07T07:42:15.310888`
(begin_time/end_time(可选参数))

> 此API也可以通过控制台进行图形化调用

响应示例：

```json
{
    "code": 0,
    "message": "success",
    "data": {
    "circulation_quota": "i64", //流通中
    "destroy_quota": "i64",  //已销毁
    "transaction_number": "i64",  //交易次数
    "circulation_day":[
       8004750,  //当天
       0,      //前一天
       1000050,
       0,0,0,0
    ]     //近7天单日已发行总金额  
    }
}
```