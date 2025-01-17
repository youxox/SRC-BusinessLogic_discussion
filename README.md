# 业务逻辑功能点探讨

## 1.身份
- 用户(支付,购物车,限时活动,优惠券)
- 其他用户(抢商品)
- 商户(卖商品,评价,销量)
- 管理员(小游戏,用户数据保护,拿下管理员权限,恶意竞争,风评)
- 客服(XSS,越权,文件上传)

## 2.功能点
### 签到
- 并发 
- 超时签到
- 提前签到 ----> [修改时间 Http头Data参数]
---------------------------------------------
### 充值
- 并发
- 0元充值
- 替换数据包充值
---------------------------------------------
### 转盘抽奖
- 盲盒
- 并发
- 重放
- 替换
---------------------------------------------
### 答题闯关
- 本人答题
- 他人答题 [PK, 不公平性, 修改答案, 分数, 排名]
- 审核
- 管理员
```angular2html
1.并发答题 --> 一次答题获
3.并发签到
4.考试题目泄露 --> 为什么能算?
	|_你答题可以获得东西:积分,礼品,排名,证书
```
---------------------------------------------
### 在线社区
- 1.身份
```angular2html
用户 ---> (钓鱼,XSS)
普通其他用户 ----> (评论,匿名评论)
官方认证用户 ----> (一个视频快速突破逻辑去刷流量)
审核 ----> (某些内容无法通过审核,但是让你通过了[突破风控])
管理员(越权)
```
- 2.信息泄露
```
遇到用户交互地方,就可能存在信息泄露
```
- 3.在线客服
```
1. <h1>1</h1>去判断是否存在XSS
2. 批量打Payload或者去找到一下没被过滤的标
3. 提升危害,是否能够打到Cookie
4. 文件上传打出XSS(文件类型: svg xml pdf html)
```
---------------------------------------------
### 门户网站/官网
- 短信轰炸
- 信息泄露, 调用不同接口, 查看历史数据包, 乱点!
- 返回包里面泄露发布文字的用户数据(userid, 手机号, 身份证)
- 你可以做一些路径扫描(扫备份文件)
___________________________________________

### 招聘网站
- 1.编辑简历,文件上传,造成XSS
```
补充1: 路径寻找,提交的数据包,预览的数据包,泄露访问路径,成功导致XSS
补充1: 文件(xml pdf svg html)
```

- 2.业务逻辑
```angular2html
多次投递,突破简历上线,增加曝光度
 |_单个岗位,多次投递(系统只允许一个岗位投递一次)
 |_多个岗位 投递(系统只允许投递一个岗位)
```
查看他人投递(未授权访问) --> 可遍历id获得此岗位投递人数 --> 可提前查看此岗位投递情况,是否选择投递此岗位
- 3.越权
```angular2html
越权编辑,越权保存(含有敏感数据的一定要尝试越权)
```
- 4.文件遍历(前提是可遍历) 
```
1.pdf -> 2.pdf -> 1.txt
```
- 5.编辑简历 XSS尝试 (看到框就尝试XSS)

----------------------------------------------
### 网站管理
核心: 用户权限(管理员+普通用户),web站点内容编辑
- 1.失效的会话注销
- 2.文件上传导致Getsh
- 3.内容编辑,任意文件上传
- 4.内容编辑,存储型XSS
- 5.信息泄露
- 6.越权
------------------------------------------------
### 并发
关键点:❗注意拦截到关键数据包 再进行并发 并发后数据包才能放掉或丢包
易发生并发漏洞的功能点
- 1.签到
- 2.领取优惠券
- 3.并发下单 
```
a.突破商品数量 b.一张优惠券的多次下单
```
- 4.发言,发文章,品论赚积分
- 5.投票,并发投票
- 6.转盘,抽奖
- 7.领取获奖编码
---------------------------------------------------
