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