# seckill
秒杀解决方案

## 基础能力-接口列表

### Activity 活动相关
- CreateActivity 创建活动
- UpdateActivity 修改活动
- ListActivity 活动列表
- DetailActivity 活动详情
- DelActivity 删除活动

### SeckillProduct 秒杀商品相关
- AddSeckillProduct 添加秒杀商品
- UpdateSeckillproduct 修改秒杀商品 p.s. 活动上线时不允许修改库存，商品
- GetSeckillProducts 查看秒杀商品列表
- DetailSeckillProduct 查看秒杀商品详情
- DelSeckillProduct 删除秒杀商品

### 库存操作
- ___StockOpt___ 库存操作: 预扣/回滚/扣除/源渠道售罄

### 缓存管理
- PreLoadCache 预加载商品缓存数据
- Flush 清空活动商品缓存

----

## 秒杀架构设计

### 秒杀流程图
![流程图](http://goldentec-1258944054.cos.ap-guangzhou.myqcloud.com/dev/console-center-console-ctl/company-info/63bba768000d7baa.png)

### 获取秒杀商品列表流程
![流程图](http://goldentec-1258944054.cos.ap-guangzhou.myqcloud.com/dev/console-center-console-ctl/company-info/63c5216e0002e60e.png)

----


----

## 核心lua脚本

### 预扣功能
```lua
local key = KEYS[1]
local surplus_stock_filed = ARGV[1]
local restriction_field = ARGV[2]
local increment = ARGV[3]

-- 最外层过滤，判断秒杀商品库存是否存在
if redis.call('hexists', key, surplus_stock_filed) == 0 then
	return -1
end

-- 获取 '库存', '单次购买最大数量' 字段，用于业务限制
local vals = redis.call('hmget', key, surplus_stock_filed, restriction_field)
if #vals ~= 2 then 
	return -1
end 

local stock = tonumber(vals[1])
local restriction = tonumber(vals[2])

-- 本次购买数量超过限制 返回错误码：-2
if tonumber(increment) > restriction then 
	return -2
end

-- 当秒杀商品库存大于0时，预扣库存
if stock > 0 then
	redis.call('hincrbyfloat', key, surplus_stock_filed, -increment)
	return stock
end

return -1
```

----

### 库存回滚功能
``` lua
local key = KEYS[1]
local surplus_stock_filed = ARGV[1]
local increment = ARGV[2]

if redis.call('hexists', key, surplus_stock_filed) == 0 then
	return -1
end

local stock = tonumber(redis.call('hget', key, surplus_stock_filed))
if stock >= 0 then
	redis.call('hincrbyfloat', key, surplus_stock_filed, increment)
	return stock
end

return -1
```
