#app 接口站点


**注意事项：带签名的接口，请求头部必须带上两个参数**

    _timestamp：当前的时间戳
    _sign:md5("xxxxx") //线下沟通
   
   
  
**所有接口返回格式**
  
    {
        "message": "",
        "result": null,
        "returncode": 0
    }   
**returncode介绍**

     -1XX  参数有误
     0：正常
     500：内部错误
     301：token 有误 自动退出

1.注册

    1.1获取短信验证码（需求签名）

       url: /v1/login/getVerificationCode
       参数：
            mobileNo：String "13000000001"   // 手机号
            appversion：String ""            // app 版本号
            deviceid： String  ""            // 设备唯一标识
      
       返回值：returncode= 0 表示正常
            {
                "message": "",
                "result": null,
                "returncode": 0
            }  
            
    1.2.手机号+验证码+密码 登录 （需求签名）
    
       url: /v1/login/reglogin
       参数：
            mobileNo：String "13000000001"   // 手机号
            verificationCode：Sting ""       // 验证码
            password: Sting                  // 密码
            appversion：String ""            // app 版本号
            deviceid： String  ""            // 设备唯一标识
      
       返回值：returncode= 0 表示正常
            {
                "message": "",
                "result": "xxxxx",           // 会话token
                "returncode": 0
            } 
             
2.登录 （手机号+密码）（手机号+验证码）
 
    2.1 手机号+ 密码登录 （需求签名）
    
        url: /v1/login/login
        参数：
                mobileNo：String "13000000001"   // 手机号
                password: Sting                  // 密码
                appversion：String ""            // app 版本号
                deviceid： String  ""            // 设备唯一标识
          
        返回值：returncode= 0 表示正常
                {
                    "message": "",
                    "result": "xxxxx",           // 会话token
                    "returncode": 0
                } 
                 
    2.2 手机号+ 验证码 登录
        
        可参考1.2 密码（password）可以设置一个默认密码

3.修改密码 （手机号+验证码）

    3.1 手机号+验证码+新密码 （参考1.2）

4.个人信息接口

    4.1 根据token 获取信息 也可以用于检测token 是否有效  （需求签名）

        url: /v1/login/checkusertoken
        参数：
                usertoken: Sting                  // 用户token
                appversion：String ""            // app 版本号
                deviceid： String  ""            // 设备唯一标识
          
        返回值：returncode= 0 表示正常
                {
                "message": null,
                "result": {
                    "accesstoken": "1b28a57e-42ce-45e4-9a9b-fb2dd9df91cf",
                    "appversion": null,
                    "avatarurl": null,
                    "birthday": null,
                    "deviceid": null,
                    "email": null,
                    "industry": null,
                    "mobile": "18911692574",
                    "nickname": null,
                    "realname": null,
                    "sex": null
                    },
                "returncode": 0
                }
         
5.退出登录

    5.1 根据token 退出   （需求签名）
            url: /v1/account/logout
            参数：
                    usertoken: Sting                  // 用户token
                    appversion：String ""            // app 版本号
                    deviceid： String  ""            // 设备唯一标识
              
            返回值：returncode= 0 表示正常
                    {
                    "message": "",
                    "result": null,
                    "returncode": 0
                    }
6.我的

    6.1  修改个人信息（头像，手机号，昵称，行业，真实姓名，密码，性别，年龄（下拉）） 
  
          6.1.1 上传头像   （需求签名）
                url: /v1/account/profile  post
                参数：
                      file: File                  // 用户token
                              
                返回值：returncode= 0 表示正常
                      {
                       "message": "",
                        "result": "http:xxxxx.png",
                        "returncode": 0
                        }
          6.1.2 修改个人信息，需要修改个人头像，将第一步的地址，赋值   （需求签名）            
               url: /v1/account/updateAccountinfo
               参数：
                      accesstoken: String                  // 用户token
                      realname: String                  // 真实姓名
                      nickname: String                  // 昵称
                      avatarurl: String                  // 头像地址
                      sex: String                  // 性别  0 未知 1 男 2 女
                      industry: String                  // 行业 
                      birthday: Date                  // 生日   
                      appversion: String                  // app 版本  
                      deviceid: String                  // 设备唯一标识                               
                返回值：returncode= 0 表示正常
                      {
                      "message": "",
                       "result": null,
                       "returncode": 0
                      }                            
    6.2  发票信息 
    
    个人 （个人姓名）、公司（纳税人识别号，抬头（公司名称），公司开户行、开户账号，
    					公司地址、公司电话，收件人，收件人手机号、收件人地址）
    					
    	  6.2.1 获取发票列表  （需求签名）
    	        url: /v1/invoice/getList
                参数：
                      accesstoken: String                  // 用户token                               
                返回值：returncode= 0 表示正常
                   {
                      "message": "",
                      "result": [{
                          id:  Long                 发票ID
                          invoicetype:Integer       发票类型；0 个人 ；1 公司
                          invoicetitle:             抬头
                          identification:           纳税人识别号 (发票类型:公司 必填)
                          companybank:              公司开户行   (发票类型:公司 必填)
                          companycardno:            开户账号    (发票类型:公司 必填)
                          companyaddress            公司地址    (发票类型:公司 必填)
                          companytel:               公司电话    (发票类型:公司 必填)
                          receivename:              收件人      
                          receivemobile:            收件人手机号 
                          receiveaddress:           收件地址      
                          isdefault:                是否设置为默认发票
                          createtime:               创建时间
                          lastmodifytime            最后修改时间
                      }],
                      "returncode": 0
                   }    
    	  6.2.2 添加发票  （需求签名）
              	        url: /v1/invoice/add
                          参数：
                                accesstoken: String                  // 用户token  
                                invoicetype:Integer       发票类型；0 个人 ；1 公司
                                invoicetitle:             抬头
                                identification:           纳税人识别号 (发票类型:公司 必填)
                                companybank:              公司开户行   (发票类型:公司 必填)
                                companycardno:            开户账号    (发票类型:公司 必填)
                                companyaddress            公司地址    (发票类型:公司 必填)
                                companytel:               公司电话    (发票类型:公司 必填)
                                receivename:              收件人      
                                receivemobile:            收件人手机号 
                                receiveaddress:           收件地址      
                                isdefault:   Boolean      是否设置为默认发票                                                             
                          返回值：returncode= 0 表示正常
                             {
                                "message": "",
                                "result": null,
                                "returncode": 0
                             }  
          6.2.2 删除发票  （需求签名）
                        url: /v1/invoice/delete
                        参数：
                                accesstoken: String                  // 用户token 
                                invoiceId:Long                       // 发票id                                      
                        返回值：returncode= 0 表示正常
                                {
                                  "message": "",
                                  "result": null,
                                  "returncode": 0
                                }                     