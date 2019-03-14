# MyWorldmongo基本操作
http://www.runoob.com/mongodb/mongodb-query.html

库表说明
库名	描述
zzb	生产库
zzb_dev	开发库（开发人员本地）
zzb_inquiry	健康问诊
参数说明
参数	描述
local_server_ip	服务器IP
runtime_env	服务器环境
invoke_time	调用时间
logs.ip	请求IP
input.api_name	接口名字
input.input	入参
output.output	出参
output.cost_time	接口耗时
output.is_success	是否成功 0:成功 1:业务失败 9:服务器错误
查询语句
教程链接：http://www.runoob.com/mongodb/mongodb-aggregate.html

db.api_log.find({查询条件}).sort({需要排序的字段 : 倒叙-1 正序 1})
模糊查询语句
教程链接：http://www.runoob.com/mongodb/mongodb-aggregate.html

db.api_log.find({{需要查询的字段: {$regex: 模糊条件, $options: "i"}}})
聚合查询语句
教程链接：http://www.runoob.com/mongodb/mongodb-aggregate.html

db.api_log.aggregate(
    {$match: {查询条件}},
    {$group: {聚合条件}},
    {$sort:{需要排序的字段 : 倒叙-1 正序 1}}
)
查询语句示例
查询调用时间超过 2000ms 的接口调用信息 倒叙排列
db.api_log.find({"output.cost_time": {$gte: 2000}}).sort({ invoke_time:-1 })
查询接口名字为 /ZZB/api/health/inquiry/POST 的接口调用信息 倒叙排列
db.api_log.find({"input.api_name": "/ZZB/api/health/inquiry/POST"}).sort({ invoke_time:-1 })
查询接口名字为 /ZZB/api/health/inquiry/POST 的接口调用信息 倒叙排列
db.api_log.find({"input.api_name": "/ZZB/api/health/inquiry/POST"}).sort({ invoke_time:-1 })
模糊查询调用时间为 2018-05-28 的接口调用信息 倒叙排列
db.api_log.find({{invoke_time: {$regex: "2018-05-28", $options: "i"}}}).sort({ invoke_time:-1 })
聚合查询 调用IP为 220.191.112.157 各接口调用次数 倒叙
db.api_log.aggregate(
    {$match: {"logs.ip" : "220.191.112.157"}},
    {$group: {_id: "$input.api_name", num: {$sum: 1}}},
    {$sort: {num: -1}}
)
聚合查询 查找各接口调用失败次数 倒叙
db.api_log.aggregate(
    // {$match: {"server_version": "180619", "output.is_success" : {$ne : "0"}}},
    {$match: {"server_version": "180619", "output.is_success" : "9"}},
    {$group: {_id: "$input.api_name", num: {$sum: 1}}},
    {$sort: {num: -1}}
)
