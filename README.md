# trid_doc


#user api

##短信验证请求

- c->s
    - 请求方式：POST
    - URL：http://101.200.89.240/index.php?r=user/sms-validation-request
    - 请求格式：

            {
                "type":"sms_validation_request",
                "tel":"13811112222",
                "time":"1439280893"
            }
            
    - 注意事项:
        - 每个客户端/手机号，每60s只能请求一次，请求过快会返回错误
        - 现阶段没有调用第三方短信服务，验证码直接设置为`123456`



- s->c
    - 成功返回：

            {
                "type": "sms_validation_send",
                "success": true,
                "error_no": 0,
                "error_msg": null
            }


    - 失败返回:


            {
                "type:" "sms_validation_send",
                "success": false,
                "error_no": 1,
                "error_msg": "json decode failed."
            }


    - 错误码:
    
        |error_no|error_msg|description|
        |--------|---------|-----------|
        |1|json decode failed.|输入不是有效的json对象|
        |2|input not valid.|请求不完整，缺少某些属性|
        |3|database error.|数据库异常|
        |4|3rd party sms send failed.|第三方短信服务出错，通常是请求频率过高|




##短信验证码验证

- c->s
    - 请求方式：POST
    - URL：http://101.200.89.240/index.php?r=user/sms-validation-code
    - 请求格式：

            {
                "type":"sms_validation_code",
                "tel":"13811112222",
                "time":"1439280893",
                "code":"123456"
            }
            
    - 注意事项:
        - 无


- s->c
    - 成功返回： 客户端应该存储token，以便在下一步注册中使用

            {
                "type": "sms_validation_result",
                "success": true,
                "token": "59437135717575643276636549645a787678307253673d3d",
                "error_no": 0,
                "error_msg": null
            }


    - 失败返回:

            {
                "type:" "sms_validation_result",
                "success": false,
                "error_no": 1,
                "error_msg": "json decode failed."
            }

    - 错误码:
    
    
        |error_no|error_msg|description|
        |--------|---------|-----------|
        |1|json decode failed.|输入不是有效的json对象|
        |2|input not valid.|请求不完整，缺少某些属性|
        |3|validation code invalid.|验证码错误|





##注册用户

- c->s
    - 请求方式：POST
    - URL：http://101.200.89.240/index.php?r=user/register
    - 请求格式：

            {
                "type":"register",
                "tel":"13811112222",
                "token":"59437135717575643276636549645a787678307253673d3d",
                "username":"zly",
                "password":"123456a"
            }
            
    - 注意事项:
        - `token`为短信验证成功后回复的`token`


- s->c
    - 成功返回： 

            {
                "type": "register"
                "token": "66646b5937726133323071337263386b3861623638513d3d"
                "huanxin_id": "13811112222"
                "huanxin_password": "$2y$10$SyU0uvBEoJG8dxkqVmP8KOyG3ZtDgca.4SvK2LFvTILddmN4eRxbq"
                "success": true
                "error_no": 0
                "error_msg": null
            }
    - 注意事项：
        - `token`为以后与服务端交互所使用的，也就是说如果在`token`有效期内，只需要将其存储起来便可以和服务端交互，不需要重新登录
        - `huanxin_id`和`huanxin_password`为客户端与环信服务端交互所需要的唯二信息

    - 失败返回:

            {
                "type:" "register"
                "success": false
                "error_no": 1
                "error_msg": "json decode failed."
            }

    - 错误码:
    
    
        |error_no|error_msg|description|
        |--------|---------|-----------|
        |1|json decode failed.|输入不是有效的json对象|
        |2|input not valid.|请求不完整，缺少某些属性|
        |3|tel not found.|电话号码错误|
        |4|token not valid.|token不正确，可能是过期或者错误了，需要通过登录流程重新获取新的token|
        |5|tel not verified.|电话号码未通过短信验证|
        |6|database error.|数据库错误|


    


##issues

### composer slow

[composer 镜像](http://www.cnblogs.com/yjf512/p/4585078.html)
