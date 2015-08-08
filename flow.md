#client server flow

##登录、注册

###短信验证

- c->s: 短信验证请求 ：`sms_validation_request`

```
{
	"type":"sms_validation_request",
	"tel":"13866668888",
	"time":"2015-8-4 1:44"
}
```


- s: 检测客户端上次请求的时间，若与此次大于60s，调用第三方平台接口，发送给对应手机号码
- s->c: 短信验证响应：`sms_validation_send`,提示客户端是否成功调用了第三方短信发送接口

```
{
	"type":"sms_validation_send",
	"success":true,
	"error_no":0,
	"err_msg":""
}
```

- c: 搜集用户填写的验证码
- c->s: 短信验证吗：: `sms_validation_code`
```
{
	"type":"sms_validation_code",
	"tel":"13866668888",
	"code":"123456"
}
```

- s: 与第三方借口返回的验证码相比较，
- s->c： 短信验证结果，`sms_validation_result`,若成功应该包含一个token，用于坚定客户端是否通过了短信验证


```
{
	"type":"sms_validation_result",
	"success":false,
	"error_no":1,
	"err_msg" : "验证码不符"
}
```

###信息收集
1. c: 搜集用户信息
2. c->s: 用户信息: `user_register_info`,应该包含上一步所返回的token
3. s: 验证token，将`user_register_info`存入数据库
4. s-c： 用户信息结果： `user_register_result`,包含是否注册成功，以及用户id，token，token有效日期


##主界面

###暗恋

###Around

###个人资料

###设置
