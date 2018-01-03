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

       url: /v1/login/getVerificationCode    POST
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
    
       url: /v1/login/reglogin              POST
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
    
        url: /v1/login/login                 POST
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

        url: /v1/login/checkusertoken           POST
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
            url: /v1/account/logout         POST
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
                url: /v1/account/profile     POST
                参数：
                      file: File                  // 用户token
                              
                返回值：returncode= 0 表示正常
                      {
                       "message": "",
                        "result": "http:xxxxx.png",
                        "returncode": 0
                        }
          6.1.2 修改个人信息，需要修改个人头像，将第一步的地址，赋值   （需求签名）            
               url: /v1/account/updateAccountinfo       POST
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
    	        url: /v1/invoice/getList   GET
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
              	        url: /v1/invoice/add        GET
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
                        url: /v1/invoice/delete         GET
                        参数：
                                accesstoken: String                  // 用户token 
                                invoiceId:Long                       // 发票id                                      
                        返回值：returncode= 0 表示正常
                                {
                                  "message": "",
                                  "result": null,
                                  "returncode": 0
                                } 

7.获取app 首页 Banner 接口

        url: /v1/courses/banners        GET
        参数：
            无                                                        
        返回值：returncode= 0 表示正常
             {
             "returncode": 0,
             "message": null,
             "result": [
                 {
                     "bannertype": 1, // banner 图片类型 0:图片；1 课程图片；
                     "bannerurl": "http://39.106.32.142:8080/upload/2d19560b4046944a53d4e26fff2b8268.jpg",
                     "courseid": 1,   // 课程ID
                     "sortindex": 0
                 },
                 {
                     "bannertype": 1,
                     "bannerurl": "http://39.106.140.158:8080/upload/profile/20171202/27cf9745-2151-4a90-909e-7d6b8a7f3f5c.png",
                     "courseid": 1,
                     "sortindex": 0
                 },
                 {
                     "bannertype": 0,
                     "bannerurl": "http://39.106.140.158:8080/upload/profile/20171202/27cf9745-2151-4a90-909e-7d6b8a7f3f5c.png",
                     "courseid": null,
                     "sortindex": 0
                 }
              ]
             }
8.课程接口
   
    8.1 获取课程列表接口
        
                url: /v1/courses/list       GET
                参数：
                    lastCourseId:0 // Long 获取最后一个课程Id    
                    usertoken:"1111" // 当前登录的用户token                                                      
                返回值：returncode= 0 表示正常
                     {
                     "returncode": 0,
                     "message": null,
                     "result": [
                         {
                         "id": 1,
                         "courseName": "课程中文名",
                         "courseNameEn": "课程英文名",
                         "courseIntroduce": "<h2>H+ 后台主题</h2>\n <p>\n H+是一个完全响应式，基于Bootstrap3.3.6最新版本开发的扁平化主题，她采用了主流的左右两栏式布局，使用了Html5+CSS3等现代技术，她提供了诸多的强大的可以重新组合的UI组件，并集成了最新的jQuery版本(v2.1.1)，当然，也集成了很多功能强大，用途广泛的就jQuery插件，她可以用于所有的Web应用程序，如<b>网站管理后台</b>，<b>网站会员中心</b>，<b>CMS</b>，<b>CRM</b>，<b>OA</b>等等，当然，您也可以对她进行深度定制，以做出更强系统。\n </p>\n <p>\n <b>当前版本：</b>v4.1.0\n </p>\n <p>\n <b>定价：</b><span class=\"label label-warning\">¥988（不开发票）</span>\n </p>",
                         "courseBrannerImg": "http://39.106.32.142:8080/upload/2d19560b4046944a53d4e26fff2b8268.jpg",
                         "courseTeacherName": "测试人员",
                         "courseTeacherEmail": "1@126.com",
                         "courseTeacherIntroduce": "<h2>H+ 后台主题</h2>\n <p>\n H+是一个完全响应式，基于Bootstrap3.3.6最新版本开发的扁平化主题，她采用了主流的左右两栏式布局，使用了Html5+CSS3等现代技术，她提供了诸多的强大的可以重新组合的UI组件，并集成了最新的jQuery版本(v2.1.1)，当然，也集成了很多功能强大，用途广泛的就jQuery插件，她可以用于所有的Web应用程序，如<b>网站管理后台</b>，<b>网站会员中心</b>，<b>CMS</b>，<b>CRM</b>，<b>OA</b>等等，当然，您也可以对她进行深度定制，以做出更强系统。\n </p>",
                         "courseTeacherId": 0,
                         "courseType": 0,//课程类型：0 直播 1 视频
                         "canReview": 1, //是否支持回看 0 ：否 1 ：是
                         "originalPrice": 100, // 原价
                         "currentPrice": 80,   //优惠价
                         "deadlineDays": "2017-12-30 10:29:22"  //观看截止日期
                         "haspayed": 0 // 是否已经报名课程 0 未报名 ; >0 已报名
                         }
                     ]
                     }
    8.2 获取课程明细接口
       
                 url: /v1/courses/detail       GET
                 参数：
                     courseId:5 // Long 课程Id
                     usertoken:"1111" // 当前登录的用户token                                                        
                 返回值：returncode= 0 表示正常
                                                  
                    {
                    "returncode": 0,
                    "message": null,
                    "result": {
                    "id": 5,
                    "courseName": "视频课程",
                    "courseNameEn": "test",
                    "courseIntroduce": "111111",
                    "courseBrannerImg": "http://39.106.32.142:8080/upload/2d19560b4046944a53d4e26fff2b8268.jpg",
                    "courseTeacherName": "qq",
                    "courseTeacherEmail": "111",
                    "courseTeacherIntroduce": "111",
                    "courseTeacherId": 0,
                    "courseType": 1,
                    "canReview": 0,
                    "originalPrice": 100,
                    "currentPrice": 77,
                    "deadlineDays": "2017-12-30 10:58:17",
                    "courseDetail": [
                        {
                        "id": 35,
                        "courseid": 5,
                        "detailtitle": "1111",
                        "detailtitleen": "1111",
                        "publishjson": "",
                        "rtmpurl": "http://39.106.32.142:8080/upload/7f9b61a7d9913ec87aadc5653c5ec1a8.mp4",
                        "starttime": "",
                        "endtime": "",
                        "livestatus": 0,// 直播状态: 0:待直播; 1:直播中；2：重播
                        "livevideourl": "",
                        "livevideoduration": 0
                        }
                    ],
                    "assistantList": [
                        {
                        "id": 17,
                        "courseid": 5,
                        "mobile": "1111",
                        "assistantid": 13
                        }
                    ]
                    }
                    }

    8.3 获取弹幕消息
            
                url: /v1/danmu/list      GET
                参数：
                       courseDetailId=41  // 课程下的可节目Id
                       lastMsgId=0   // Long 最后一个弹幕消息Id                                                      
                返回值：returncode= 0 表示正常
                     {
                     "returncode": 0,
                     "message": null,
                     "result": [
                         {
                         "id": 1,
                         "coursedetailid": 41,
                         "msgtype": 0, // 消息类型：0 文本 1 ：emoji 2 : 图片
                         "msgcontent": "test",
                         "accountid": 11,
                         "realname": "15010082975",
                         "accounttype": 0, // 弹幕用户类型：0 ：学员 1： 助教 2：老师
                         "createtime": "2018-01-03 22:29:47"
                         }
                     ]
                     }
                                           
     8.4 发送弹幕消息
     
                url: /v1/danmu/push      post
                参数：{
                     "usertoken":"e69bab07-2a4c-4484-b0f8-1140b674155d", // 登录用户的token
                     "courseDetailId":41, // 课程下的可节目Id
                     "msgType":0,  // 消息类型：0 文本 1 ：emoji 2 : 图片
                     "msgContent":"弹幕内容"
                   }
                返回值：returncode= 0 表示正常   
                                                       
                   {"returncode":0,"message":"","result":null}                                