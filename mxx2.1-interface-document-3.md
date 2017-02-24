[toc]

## 麦学习2.1版本接口
### 1 用户中心相关
#### 1.1 token相关接口
> 和微校园token保持一致即可

#### 1.2 用户名/手机号/邮箱、密码/手机短信验证码登录接口
> 参考login.5.1 用户名/手机号/邮箱、密码/手机短信验证码登录接口
> 登录接口不需要token验证
> 先登录账号，再选择学校和角色登陆系统
> 用户名、用户手机、用户邮箱填一个即可；输入密码或者短信验证码登录，登录成功后选择用户学校，进入系统；如果用户不存在则提示登录失败；
> 新增图片验证码服务，需要在登录的时候填入该参数，验证通过后才允许登录；
``` json
Url:        /mstudy/login
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'clientType':  客户端类型（1-手机号、邮箱、用户名密码方式登录;2-手机验证码登录;3-微信登录;4-支付宝登录;）（必填）
    - 'userType':    登录角色（1-教师；2-家长；3-学生）
    - 'account':     用户账号（必填-用户名、邮箱、手机号选一）
    - 'email':       用户邮箱（必填-用户名、邮箱、手机号选一）
    - 'phone':       用户手机号（必填-用户名、邮箱、手机号选一）
    - 'password':    用户密码（必填 -密码和手机验证码二选一）
    - 'captcha':     用户收到的手机验证码（必填 -密码和手机验证码二选一）
    - 'code':        图片验证码信息   ---必填项
    - 'codeKey':     图片验证码key值   ---必填项
示例
{
    "clientType":1,
    "userType":1,
    "account":"luoidbashi",
    "email":"eee@qq.com",
    "phone":"15265236523",
    "password":"123456",
    "captcha":"63656",
    "code":"63656",
    "codeKey":"sfasdfkjwejis"
}
Response:  
    - 'status'：          0->登录成功
                          1->登录失败，账号或密码不正确
                          2->验证码错误
    - 'schoolName'：      学校名称
    - 'isUse'：           是否启用，0-未启用1-启用，只返回为1的
    - 'uid'：             用户id
    - 'token'：           临时token
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data": {
        "info":"登录成功",
        "token":"1kjsldfj23lasjdfl",
        "uid":14,
        "userType":1,
        "school":[
            {
                "sid":1,
                "isUse":1,
                "schoolName":"英才中学"
            },
            {
                "sid":2,
                "isUse":1,
                "schoolName":"博文中学"
            },
            {
                "sid":3,
                "isUse":1,
                "schoolName":"南阳中学"
            }
        ]
    }
}
```

#### 1.3 选择登录学校及角色接口
> 参考login.5.2 选择登录学校及角色接口
>不需要token验证
>用户登录成功后获取该用户的学校列表，用户选择所需要登录的学校id，生成token进入系统；
>和微校园区别在于userId变为uid;
>新增学生登录返回信息
>2017-02-23（luodibashi）:update：新增用户需要补全信息状态时的返回值；
``` json
Url:        /mstudy/login/in
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'sid':           学校id
    - 'userType':      登录角色（1-教师；2-家长；3-学生）
    - 'uid':           用户id           
    - 'token'：        临时token
示例
{
    "sid":1,
    "userType":1,
    "uid":12,
    "token":"1kjsldfj23lasjdfl",
}
Response:  
    - 'status'：         0->登录成功
                        1->登录失败，用户已不存在
    - 'schoolName'：      学校名称
    - 'teacherName'：     教师姓名
    - 'phone'：           教师或家长手机号
    - 'email'：           教师或家长邮箱
    - 'token'：           持久化token
    - 'idType':           教师的权限角色  0-普通教师，1-学校管理员，2-超级管理员
    - 'loginTime':        登陆时间
    - 'loginStatus':      登陆状态
    - 'uid':              登陆用户id
    - 'tid':              登陆教师id
    - 'schoolId':         登陆学校id
    - 'userType':         登录用户角色（1-教师；2-家长；3-学生）
    - 'loginTime':        登陆时间
    - 'parentId':         登陆家长id
    - 'parentName':       登陆家长姓名
    - 'role':             登陆家长角色studentInfo
    - 'studentInfo':      登陆家长相关学生详情
    - 'studentName':      登陆家长相关学生姓名
    - 'cid':              登陆家长相关学生的班级id
    - 'className':        登陆家长相关学生的班级名称#名称为年级+班级
    - 'isDefault'         是否为主家长（0-否；1-是；） 主家长只能有一个
    - 'voucher'           补全信息凭证（被使用成功后就会失效）
```
> **返回结果示例教师：userType=1**
``` json
{
    "status": 0,
    "data": {
        "token":"121280384350617285934525656",
        "phone":"18627806720",
        "loginTime":"1611301212001",
        "schoolName":"水果湖第一小学",
        "email":"406@qq.com",
        "loginStatus":"0",
        "uid":28,
        "tid":22,
        "schoolId":9,
        "teacherName":"张三分 ",
        "userType":1,
        "idType":1
    }
}
```
> **返回结果示例家长：userType=2**
``` json
{
    "status": 0,
    "data": {
        "token":"121280384350617285934525656",
        "schoolId":123456789,
        "schoolName":"水果湖第一小学",
        "loginStatus":0,
        "loginTime":"08301253",
        "uid":28,
        "userType":2,
        "parentId":22,
        "parentName":"张三三",
        "phone":"18627806720",
        "email":"406@qq.com",
        "studentInfo":[
            {
                "studentName":"张三分 ",
                "studentId":12,
                "cid":12,
                "className":"二年级三班", 
                "role":0,    
                "isDefault":0          
            },
            {
                "studentName":"张四分 ",
                "studentId":13,
                "cid":12,
                "className":"三年级二班", 
                "role":0,    
                "isDefault":1         
            },
        ]
    }
}
```
> **返回结果示例学生：userType=3**
``` json
{
    "status": 0,
    "data": {
        "token":"121280384350617285934525656",
        "schoolId":123456789,
        "schoolName":"水果湖第一小学",
        "loginStatus":0,
        "loginTime":"08301253",
        "uid":28,
        "userType":3,
        "studentName":"张三分 ",
        "studentId":12,
        "phone":"18627806720",
        "email":"406@qq.com"
    }
}
```
> 当用户名字不存在时，需要进行补全信息，返回值为：
> **返回结果示例(教师)：**
``` json
{
    "status": 0,
    "data": {
        "tid":22,
        "voucher":"121280384350617285934525656"
    }
}
```
> **返回结果示例(家长)：**
``` json
{
    "status": 0, 
    "data": {
        "parentId":14,
        "voucher":"121280384350617285934525656"
    }
}
```
> **返回结果示例(学生)：**
``` json
{
    "status": 0,
    "data": {
        "studentId":1,
        "voucher":"121280384350617285934525656"
    }
}
```


#### 1.4 生成短信验证码接口
>参考login.5.3 生成短信验证码接口
>不需要token验证
>用户使用手机短信验证码登录系统是，需要点击获取短信验证码获取，点击后会生成验证码，格式为6为随机数字，验证有效期为30分钟，过期将失效，需要重新生成验证码；重新生成验证码需要间隔60s；重新生成验证码后，上一个验证码会失效；
``` json
Url:        /mstudy/captcha/create
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'phone':       用户手机号
示例
{
    "phone":"13562563656"
}
Response:  
    - 'status'：     0->验证码生成成功
                     1->验证码生成失败
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data": {
        "info":"ok",

    }
}
```

#### 1.5 忘记密码（手机）接口
>参考login.5.4 忘记密码（手机）接口
>忘记密码功能为用户通过手机短信方式重新设置密码的功能，通过手机号方式重置，系统会发送短信验证码，用户输入验证码验证成功后，即可修改密码；
``` json
Url:        /mstudy/forgetPassword
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'phone':       用户手机号
    - 'captcha':     短信验证码
示例
{
    "phone":"13526523256",             
    "captcha":"63656"
}
Response:  
    - 'status'：    0->验证成功
                    1->验证失败，手机号不是系统用户
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data": {
        "info":"验证成功",
        "uid":12
    }
}
```

#### 1.6 重置密码接口
> 参考login.5.5 重置密码接口
> 重置密码功能为用户通过手机短信或者邮箱验证码方式重新设置密码的功能，用户通过验证码的方式进入到重置密码页面，即可修改密码；修改完成后即登录成功；
> 2017-2-20(luodibashi):update;
> 2017-2-23(luodibashi):update;
``` json
Url:        /mstudy/resetPassword
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'newPassword':        新密码  (加密方式类似于登录密码)
    - 'uid':                用户id  
示例:
{
    "uid":6,
    "newPassword":"13526523256"          
}
Response:  
    - `status`：    0->重置密码成功
                    1->重置密码失败，用户不存在
                    2->重置密码失败，两次输入的密码不一致
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data": {
        "info":"重置密码成功"
    }
}
```

#### 1.7 修改密码接口
> 密码管理修改提交接口提交了特定用户的密码，生效后原有密码失效
> 需要使用Token验证查询者身份
``` json
Url:        /mstudy/changePassword?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'oldpassword':           提交的旧密码，如""，加密方法待定
    - 'newpassword':           提交的新密码，如""，加密方法待定
    - 'token'：                token
示例：
{
    "oldpassword":"3223222",
    "newpassword":"7888232"
}
Response:
    - `status`：0->ok
                1->无权限修改密码
                2->修改密码失败，旧密码不正确
                3->修改密码失败，新密码与旧密码一致
                4->修改密码失败，新密码强度不够
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
```

> **返回结果示例：**


``` json
{
    "status": 0,
    "data":{
        "info":"ok"
    }
}
```

#### 1.8 忘记密码（邮箱）接口
> 不需要token验证
> 邮件内容为验证码（有效期为30分钟），验证码为4位字母与数字的组合，需要拿到该验证码输入验证通过获取uid,然后调用1.6接口进行重置密码；
> 2017-2-20(luodibashi):update;
> 2017-2-23(luodibashi):update;
``` json
Url:        /mstudy/forgetPassword/email
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'email':              邮箱
    - 'captcha':            邮箱验证码
实例：
{
    "email":"12qq@q.com",             
    "captcha":"63656"
}
Response:  
    - 'status'：    0->OK
                    1->系统中不能存在此邮箱，无法发送邮件
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok",
        "uid":12
    }
}
```

#### 1.9.1 注册新用户生成用户id接口
> 用于学生、教师、家长新账号注册接口
> 该接口用于注册新账号（系统中不存在待注册账号）
> 当注册教师时默认idType为0（普通教师）；
> 该接口请求成功后会返回用户id和相应补全信息凭证（凭证使用成功后就会失效），需要根据角色调用补全信息接口完成注册
> 2017-2-23(luodibashi):add;
``` json
Url:        /mstudy/register/create
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'sid':         学校id   必填
    - 'userType':    角色（1-教师；2-家长；3-学生）   必填
    - 'account':     用户名（必填-用户名、邮箱、手机号选一）
    - 'email':       用户邮箱（必填-用户名、邮箱、手机号选一）
    - 'phone':       用户手机号（必填-用户名、邮箱、手机号选一）
    - 'password':    设置用户密码（必填）加密方式同登录
    - 'captcha':     用户收到的邮箱或短信验证码（账号类型为邮箱或手机号时，必填）
    - 'code':        图片验证码信息   （账号类型为用户名时，必填）
    - 'codeKey':     图片验证码key值  （账号类型为用户名时，必填）
示例用户名注册：
{
    "sid":1,
    "userType":1,
    "account":"luoidbashi",
    "password":"123456",
    "code":"63656",
    "codeKey":"sfasdfkjwejis"
}
示例邮箱注册：
{
    "sid":1,
    "userType":1,
    "email":"luoidbashi@qq.com",
    "captcha":"123456",
    "password":"123456"
}
示例手机号注册：
{
    "sid":1,
    "userType":1,
    "phone":18627525252,
    "captcha":"123456",
    "password":"123456"
}
Response:  
    - 'status'：          0->ok
                          1->账号已存在，请使用绑定老账号完成注册
                          2->验证码错误
    - 'tid'：             教师id
    - 'parentId'：        家长id
    - 'studentId'：       学生id
    - 'voucher'：         补全信息凭证
```
> **返回结果示例(教师)：**
``` json
{
    "status": 0,
    "data": {
        "tid":22,
        "voucher":"121280384350617285934525656"
    }
}
```
> **返回结果示例(家长)：**
``` json
{
    "status": 0, 
    "data": {
        "parentId":14,
        "voucher":"121280384350617285934525656"
    }
}
```
> **返回结果示例(学生)：**
``` json
{
    "status": 0,
    "data": {
        "studentId":1,
        "voucher":"121280384350617285934525656"
    }
}
```

#### 1.9.2 绑定老用户生成用户id接口
> 用于用户已有系统账号绑定机构下学生、教师、家长角色的功能
> 该接口用于绑定老账号（目标学校目标角色中不存在待注册账号）
> 该接口请求成功后会返回用户id和相应补全信息凭证（凭证使用成功后就会失效），需要根据角色调用补全信息接口完成注册
> 2017-2-23(luodibashi):add;
``` json
Url:        /mstudy/register/binding
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'sid':         学校id   必填
    - 'userType':    角色（1-教师；2-家长；3-学生）   必填
    - 'account':     用户名（必填-用户名、邮箱、手机号选一）
    - 'email':       用户邮箱（必填-用户名、邮箱、手机号选一）
    - 'phone':       用户手机号（必填-用户名、邮箱、手机号选一）
    - 'password':    用户密码（必填）加密方式同登录
    - 'code':        图片验证码信息   ---必填项
    - 'codeKey':     图片验证码key值   ---必填项
示例：
{
    "sid":1,
    "userType":1,
    "account":"luoidbashi",
    "password":"123456",
    "code":"63656",
    "codeKey":"sfasdfkjwejis"
}
Response: 
    - 'status'：          0->ok
                          1->账号已存在，请使用绑定老账号完成注册
                          2->验证码错误
    - 'tid'：             教师id
    - 'parentId'：        家长id
    - 'studentId'：       学生id
    - 'voucher'：         补全信息凭证
```
> **返回结果示例(教师)：**
``` json
{
    "status": 0,
    "data": {
        "tid":22,
        "voucher":"121280384350617285934525656"
    }
}
```
> **返回结果示例(家长)：**
``` json
{
    "status": 0, 
    "data": {
        "parentId":14,
        "voucher":"121280384350617285934525656"
    }
}
```
> **返回结果示例(学生)：**
``` json
{
    "status": 0,
    "data": {
        "studentId":1,
        "voucher":"121280384350617285934525656"
    }
}
```

#### 1.9.3 补全教师信息并完成注册接口
> 该接口适用于注册新账号和绑定老账号功能，为通用接口；
> 需要根据1.9.1或者1.9.2接口获取相应的参数再补全信息
> 上传头像调用7.2接口获取url
> 2017-2-23(luodibashi):add;
``` json
Url:        /mstudy/register/completion/teacher
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'tid':         教师id   必填
    - 'voucher':     补全信息凭证   必填
    - 'teacherName': 教师姓名   必填
    - 'courseTagId': 教师科目标签id集合  必填
    - 'stageGradeTagId': 教师阶段年级标签id集合  必填    
    - 'photo':       教师头像 
    - 'gender':      教师性别（0-男；1-女；2-保密；）
示例：
{
    "tid":1,
    "voucher":"lkjfasldjflkjsadf",
    "teacherName":"luoidbashi",
    "courseTagId":[1,2],
    "stageGradeTagId":[3,4],
    "photo":"www.mmm.s/dfsdf.png",
    "gender":1
}
Response: 
    - 'status'：          0->ok
                          1->tid不存在
                          2->补全信息凭证无效
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data": {
        "info":"ok"
    }
}
```

#### 1.9.4 补全家长信息并完成注册接口
> 该接口适用于注册新账号和绑定老账号功能，为通用接口；
> 需要根据1.9.1或者1.9.2接口获取相应的用户id再补全信息
> 2017-2-23(luodibashi):add;
``` json
Url:        /mstudy/register/completion/parent
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'parentId':    家长id   必填
    - 'voucher':     补全信息凭证   必填
    - 'parentName':  家长姓名   必填  
    - 'photo':       头像 
    - 'workUnit':    头像 
    - 'position':    头像 
    - 'gender':      性别（0-男；1-女；2-保密；）
示例：
{
    "parentId":1,
    "voucher":"lkjfasldjflkjsadf",
    "parentName":"luoidbashi",
    "photo":"www.mmm.s/dfsdf.png",
    "gender":1,
    "workUnit":"某某公司",        
    "position":"工程师"
}
Response: 
    - 'status'：          0->ok
                          1->parentId不存在
                          2->补全信息凭证无效
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data": {
        "info":"ok"
    }
}
```
#### 1.9.5 补全学生信息并完成注册接口
> 该接口适用于注册新账号和绑定老账号功能，为通用接口；
> 需要根据1.9.1或者1.9.2接口获取相应的用户id再补全信息
> 2017-2-23(luodibashi):add;
``` json
Url:        /mstudy/register/completion/student
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'studentId':   学生id   必填
    - 'voucher':     补全信息凭证   必填
    - 'studentName':  学生姓名   必填  
    - 'photo':       头像 
    - 'gender':      性别（0-男；1-女；2-保密；）
    - 'number':      学号
示例：
{
    "studentId":1,
    "voucher":"lkjfasldjflkjsadf",
    "studentName":"luoidbashi",
    "photo":"www.mmm.s/dfsdf.png",
    "gender":1,
    "number":1
}
Response: 
    - 'status'：          0->ok
                          1->studentId不存在
                          2->补全信息凭证无效
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data": {
        "info":"ok"
    }
}
```

#### 1.9.6 生成邮箱验证码接口
> 用户使用邮箱生成验证码，需要点击获取验证码，验证码格式为4位随机字符串（数字和字母的组合），验证有效期为30分钟，过期将失效，需要重新生成验证码；重新生成验证码需要间隔60s；重新生成验证码后，上一个验证码会失效；
> 2017-2-23(luodibashi):add;
``` json
Url:        /mstudy/email/captcha/create
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'email':       用户邮箱
示例
{
    "email":"56@qq.com"
}
Response:  
    - 'status'：     0->验证码生成成功
                     1->验证码生成失败
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```

#### 1.10 邮箱账号激活接口（废弃）
> 使用场景分为两种：第一种是用户使用邮箱账号登录如果该邮箱未激活，则不允许登录，提示未激活，需要激活后登录；第二种为用户使用手机号或用户名登录系统，邮箱未激活，需要激活邮箱时可以调用激活；
> 2017-2-20(luodibashi):update;
> 2017-2-23(luodibashi):update;
``` json
Url:        /mstudy/email/activation
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'email':              邮箱
    - 'captcha':            邮箱验证码
实例：
{
    "email":"156@qq.com",
    "captcha":"15ed"
}
Response:  
    - 'status'：    0->ok
                    1->激活失败，验证码已失效
                    2->激活失败，邮箱不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```

#### 1.11.1 用户手机号修改接口
> 需要token验证
> 用户token必须与被修改者的uid保持一致；
> 首先需要验证手机号是自己所有调用1.13接口验证，返回成功后才允许传入uid进行修改（该过程由前端控制）；
> 修改手机号需要输入新手机号（新手机号系统内必须唯一）并发送验证码，填入获取验证码，提交后返回成功即可完成手机号修改；
> 2017-2-20(luodibashi):update;
``` json
Url:        /mstudy/modify/phone?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'uid':                用户id
    - 'phone':              新手机号
    - 'captcha':            验证码
    - 'token':              token
实例：
{
    "uid":1,
    "phone":"15625235235",
    "captcha":"123456"
}
Response:  
    - 'status'：    0->ok
                    1->用户id不存在
                    2->手机号已经存在
                    3->验证码错误
                    4->token和uid不符
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```

#### 1.11.2 用户的用户名修改接口
> 需要token验证
> 用户名系统内唯一，用户名首位必须是字母，长度限制在50个字符；
> 用户身份需要调用1.13接口来验证，验证通过后允许修改；
> 用户token应该与uid保持一致，即用户只能修改自己的用户名
> 2017-2-20(luodibashi):update;
``` json
Url:        /mstudy/modify/account?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'uid':                用户id
    - 'account':            用户的用户名
    - 'token':              token
实例：
{
    "uid":1,
    "account":"newacount"
}
Response:  
    - 'status'：    0->ok
                    1->用户id不存在
                    2->account已经存在
                    3->token和uid不符
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```
#### 1.11.3 用户邮箱修改接口
> 需要token验证
> 用户token必须与被修改者的uid保持一致；
> 用户身份需要调用1.13接口来验证，验证通过后允许修改；
> 输入新的邮箱账号，并点击发送邮件获取验证码（有效期为30分钟），录入验证码修改完成；
> 2017-2-20(luodibashi):update;
> 2017-2-23(luodibashi):update;
``` json
Url:        /mstudy/modify/email?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'uid':                用户id
    - 'email':              邮箱
    - 'captcha':            验证码
    - 'token':              token
实例：
{
    "uid":1,
    "email":"12qq@q.com",
    "captcha":"12qq"
}
Response:  
    - 'status'：    0->OK
                    1->用户id不存在
                    2->邮箱已存在
                    3->token和uid不符
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```
#### 1.11.4 用户名、手机号、邮箱系统中是否唯一查询接口
> 用于异步验证账号是否唯一，使用场景为用户输入邮箱或者手机号或者账号调用该接口验证系统中是否存在；
> 2017-2-20(luodibashi):update;
> 2017-2-23(luodibashi):update;
``` json
Url:        /mstudy/user/only
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'email':      邮箱（邮箱、手机号、用户名三选一）
    - 'phone':      手机号（邮箱、手机号、用户名三选一）
    - 'account':    用户名（邮箱、手机号、用户名三选一）
实例：
{
    "email":"12qq@q.com"
}
Response:  
    - 'status'：    0->ok
                    1->邮箱账号已存在
                    2->手机账号已存在
                    3->用户名账号已存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```

#### 1.11.5 用户名、手机号、邮箱账号在机构某一角色中是否唯一查询接口
> 用于异步验证机构某一角色中账号是否唯一，使用场景为注册绑定老用户是做异步验证使用；
> 2017-2-23(luodibashi):add;
``` json
Url:        /mstudy/school/role/only
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'email':      邮箱（邮箱、手机号、账号三选一）
    - 'phone':      手机号（邮箱、手机号、账号三选一）
    - 'account':    账号（邮箱、手机号、账号三选一）
    - 'sid':        学校id    ---必填
    - 'userType':   角色（1-教师；2-家长；3-学生）   ---必填
实例：
{
    "email":"12qq@q.com",
    "sid":100000,
    "userType":1
}
Response:  
    - 'status'：    0->ok
                    1->该用户已经是该机构的教师
                    2->该用户已经是该机构的家长
                    3->该用户已经是该机构的学生
                    4->系统中不存在该用户
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```

#### 1.12 验证码图片生成服务接口
> image
> 在其他需要验证码的地方，需要传入该参数才能通过
```json
Url:        /mstudy/image/code?w=[w]&h=[h]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'w':          验证码的宽度，（不传则默认为100*40px）
    - 'h':          验证码的高度，（不传则默认为100*40px）
Response:  
    - 'status'：      0->成功
                      1->获取失败
    - 'codeKey'：   验证码的key值
    - 'image'：     验证码图片,base64转码格式
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data": {
        "codeKey":"a10ddsssssd",
        "image":"ksjdkfjlasjdfkljslkdjfowe1239879oiajsdfjowiaueoiru2w3923j423j42j34o2ju3oi4oiuouosaud9028394"
    }
}
```

#### 1.13 用户账号有效性验证接口
> 需要token验证；
> 用于验证账号有效性；使用场景为用户名、手机号、邮箱修改情况下需要先验证账号的密码；
> 用户token必须与被修改者的uid保持一致；
> 2017-2-20(luodibashi):update;
``` json
Url:        /mstudy/user/confirmation?token=[token]
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'uid':         用户id
    - 'password':    用户密码（加密方法与登录相同）
示例
{
    "uid":12345,
    "password":"123456"
}
Response:  
    - 'status'：          0->验证成功
                          1->验证失败
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"验证成功"
    }
}
```

### 2 学校管理相关接口
#### 2.1 学校列表查询接口
>参考login.4.1 学校列表查询接口
> 可以获取系统中所有的学校列表，不需要token验证或者使用特殊的游客token；
> 该接口主要目的为系统管理员或者用户登录选择学校时使用，需要学校名字和id；
``` json
Url:        /mstudy/school/list?type=[type]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'type':    参数不传或者为0-仅返回有效的学校，1-返回所有学校
Response:  
    - 'status'：      0->成功
                      1->获取失败   
    - 'sid':          学校id
    - 'schoolName':   学校名字
    - 'isUse':        是否启用0-停用，1-启用
    - 'count':        学校数量
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data": {
        "count":10,
        "school":[
            {
                "sid":1,
                "isUse":1,
                "schoolName":"英才中学"
            },
            {
                "sid":2,
                "isUse":1,
                "schoolName":"博文中学"
            },
            {
                "sid":3,
                "isUse":1,
                "schoolName":"南阳中学"
            }
        ]
    }
}
```

#### 2.1.1 根据学校唯一识别码搜索学校接口
> 输入学校唯一识别码，输出系统中有效的学校名称和sid（无效的不会返回）
> 使用场景为注册的时候选择学校使用
``` json
Url:        /mstudy/school/mark
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'sCode':       学校识别码（系统内唯一，预留用作选择学校使用）
Response:  
    - 'status'：      0->ok
                      1->学校不存在  
    - 'sid':          学校id
    - 'schoolName':   学校名字
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data": {
        "sid":10000,
        "schoolName":"英才中学"
    }
}
```

#### 2.2 学校详细信息查询接口
>参考login.4.2 学校详细信息查询接口
>可以通过学校id获取学校详细信息
>需要token验证身份信息
``` json
Url:        /mstudy/school/info?sid=[sid]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'sid':      学校的id,可不传此参数，会默认获取token中包含sid的信息，传sid会获取其详细信息
Response:  
    - 'status'： 0->成功
                1->获取失败
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
    - 'sid':          学校id
    - 'schoolName':   学校名字
    - 'isUse':        是否启用0-停用，1-启用
    - 'createUid':    创建者uid
    - 'contacts':     创建者uid
    - 'phone':        联系电话
    - 'address':      学校地址
    - 'sCode':        学校识别码
    - 'longitude':    学校经度（数值范围为小数点前最大为3位数，小数点后最大为7位数，如：123.1234567，示例为最大位数限制）
    - 'latitude':     学校维度（数值范围为小数点前最大为3位数，小数点后最大为7位数，如：123.1234567，示例为最大位数限制）
    - 'createTime':   创建时间
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data": {
        "sid":10,
        "schoolName":"英才中学",
        "isUse":1,
        "createUid":12,
        "contacts":"张校长",
        "phone":"18525256525",
        "address":"某某地址",
        "sCode":"STDX",
        "longitude":"10.123456",
        "latitude":"20.25623",
        "createTime":"2016-8-9 00:00:00"
    }
}
```

#### 2.3 学校详细信息新增接口
>参考login.4.3 学校详细信息新增接口
>由系统管理员操作学校新增，其他角色不能处理这项业务，学校名字和学校识别码系统中唯一，不能重复，需要检查；
>需要token验证身份信息
``` json
Url:        /mstudy/school?token=[token]
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'createUid':      创建者uid
    - 'schoolName':     学校名字，系统唯一不能重复
    - 'contacts':       学校联系人
    - 'phone':          联系电话
    - 'address':        学校地址
    - 'sCode':          学校识别码（系统内唯一，预留用作选择学校使用）
    - 'longitude':    学校经度（数值范围为小数点前最大为3位数，小数点后最大为7位数，如：123.1234567，示例为最大位数限制）
    - 'latitude':     学校维度（数值范围为小数点前最大为3位数，小数点后最大为7位数，如：123.1234567，示例为最大位数限制）
    - 'token':          token
示例
{
    "createUid":1,
    "schoolName":"衡水一中",
    "isUse":1,
    "contacts":"张三三",
    "phone":"15785685658",
    "address":"河北省衡水市向阳大道",
    "longitude":"10.123456",
    "latitude":"20.25623",
    "sCode":"HSYZ"
}
Response:  
    - `status`： 0->ok
                1->新增失败，识别码已存在
                2->新增失败，该学校名字已存在
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
```

> **返回结果示例：**

``` json
{
    "status": 0,
    "data": {
        "info":"ok"
    }
}
```

#### 2.4 学校详细信息修改/停用（启用）接口
>参考login.4.4 学校详细信息修改/停用（启用）接口
>需要token验证身份信息
>可以由系统管理员来修改（可以修改其他任何学校管理员的资料），也可以由学校管理员来自己修改自己的资料（仅能修改自己的）
``` json
Url:        /mstudy/school?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'sid':            学校id  #系统管理员允许传sid修改某个学校，如果传该参数则以此为准，如果不传则以token中的sid为准
    - 'schoolName':     学校名字，系统唯一不能重复
    - 'isUse':          是否启用0-停用，1-启用，默认新增为启用
    - 'contacts':       学校联系人
    - 'phone':          联系电话
    - 'address':        学校地址
    - 'sCode':          学校识别码（系统内唯一，预留用作选择学校使用）
    - 'longitude':    学校经度（数值范围为小数点前最大为3位数，小数点后最大为7位数，如：123.1234567，示例为最大位数限制）
    - 'latitude':     学校维度（数值范围为小数点前最大为3位数，小数点后最大为7位数，如：123.1234567，示例为最大位数限制）
    - 'token':          token
示例修改
{
    "sid":1,                
    "schoolName":"衡水一中",
    "contacts":"张三三",
    "phone":"15785685658",
    "address":"河北省衡水市向阳大道",
    "sCode":"HSYZ",
    "longitude":"10.123456",
    "latitude":"20.25623"
}
示例停用（启用）
{
    "sid":1,               
    "isUse":1
}
Response:  
    - `status`： 0->ok
                1->修改失败，识别码已存在
                2->修改失败，该学校名字已存在
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data": {
        "info":"ok"
    }
}
```

#### 2.5 学校删除接口
>参考login.4.5 学校删除接口
>只允许系统管理员使用
>需要token验证身份信息
``` json
Url:        /mstudy/school?token=[token]
Method:     DELETE
Header:     Content-type:application/json
Parameter:
    - 'sid':            学校id
    - 'token':          token
示例
{
    "sid":1
}
Response:  
    - `status`： 0->ok
                1->删除失败，该学校已经关联了教工
                2->删除失败，该学校已不存在
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
```
> **返回结果示例：**

``` json
{
    "status": 0,
    "data": {
        "info":"ok"
    }
}
```

#### 2.6 创建学校管理员接口
> 参考login.6.2 创建学校管理员接口
> 首先验证token中用户必须为系统管理员（idTpye=2），否则不能创建学校管理员
> 新增成功后idType值需置为1；
> 需要token验证
``` json
Url:        /mstudy/create/school/administrators?token=[token]
Method:     POST
Header:     Content-type:application/json
Parameter:    
    -'token'：               token
    -'sid'：                 学校id
    -'teacherName':          教师姓名
    -'letter':               姓名首字母
    -'gender':               教师性别（0-男；1-女；2-保密；）
    -'dateOfBirth':          出生年月
    -'startWorkT':           工作时间
    -'jobNumber':            工号
    -'cardType':             证件类型
    -'cardNumber':           证件号码
    -'homeAddress':          家庭住址
    -'phone':                电话
    -'photo':                教工照片
    -'email':                邮箱
    
示例：
{
    "sid":1,
    "teacherName":"张三",
    "letter": "A",
    "gender":1,              
    "dateOfBirth":"2011-7-8 00:00:00",
    "startWorkT":"2011-7-8 00:00:00",
    "jobNumber":"0189302",
    "cardType":1,
    "cardNumber":"425222522223252112",
    "homeAddress":"斯里兰卡",
    "phone":13234556677,
    "photo": "url", 
    "email":"ee@ww.com"
}
Response:  
    - `status`：    0->创建成功
                    1->您没有足够权限，请联系系统管理员
                    2->学校或者用户不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data": {
        "info":"创建成功"
    }
}
```

#### 2.7 设置/取消教工为学校管理员接口
> 参考login.6.1 设置/取消教工为学校管理员接口
> 学校管理员可以设置或取消该学校其他教工为该学校管理员，不能跨学校设置；
> 设置或取消学校管理员的时候需要验证处理人是否有权限，需要从token中获取该用户身份，条件是在该学校有管理员权限或者具备系统管理员权限，否则不能设置和修改；
> 系统管理员暂不能通过该接口来设置；
> 需要token验证
``` json
Url:        /mstudy/set/school/administrators?token=[token]
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'tid':                教工id
    - 'sid':                学校id
    - 'idType':             用户角色：0-普通教师；1-学校管理员；2-系统管理员；
    - 'token':              token         
示例
{
    "tid":1,
    "sid":2,
    "idType":1         
}
Response:  
    - `status`：    0->设置成功
                    1->您没有足够权限，请联系系统管理员
                    2->学校或者用户不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data": {
        "info":"设置成功"
    }
}
```

#### 2.8 查询指定学校学生列表接口
> 查询指定学校学生列表接口
> 只有超级管理员（idType=2）能够查询其他sid的学生列表，学校管理员（idType=1）则不用传sid，直接从token中获取该学校的学生列表；
> isUse字段代表的意思为该用户的该角色是否启用，所以需要在对应角色表中查询该状态，默认为1启用状态；
``` json
Url:        /mstudy/school/student?sid=[sid]&page=[page]&size=[size]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'sid':                学校id    ---非必填，超级管理员使用，学校管理员不用填
    - 'page':               分页参数，不填则默认返回第一页
    - 'size':               每页显示数据量，不填则默认每页12条
    - 'token':              token
Response:  
    - 'status'：    0->ok
                    1->学校不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
    - 'page':               分页参数，不填则默认返回第一页
    - 'size':               每页显示数据量，不填则默认每页12条
    - 'studentList':        学生列表json
    - 'uid':                用户id
    - 'studentId':          学生id
    - 'studentName':        学生姓名
    - 'number':             学号
    - 'studentCode':        学籍号
    - 'letter':             姓名汉语拼音首字母
    - 'email':              邮箱
    - 'phone':              手机号
    - 'account':            账号
    - 'photo':              头像
    - 'isUse':              是否启用：0-否；1-启用；
    - 'createTime':         创建时间        
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "page":1,
        "size":12,
        "count":200,
        "studentList":[
            {
                "uid":1,
                "studentId":1,
                "studentName":"zhansansan",
                "number":"01928293",
                "studentCode":"18373839238393",
                "letter":"Z",
                "email":"123@mxx.com",
                "phone":"15123212352",
                "account":"15123212352",
                "photo":"http://wxy.maixuexi.cn/img.jpg",
                "isUse":1,
                "createTime":"2016-12-20 12:12:00"
            },
            {
                "uid":1,
                "studentId":1,
                "studentName":"zhansansan",
                "number":"01928293",
                "studentCode":"18373839238393",
                "letter":"Z",
                "email":"123@mxx.com",
                "phone":"15123212352",
                "account":"15123212352",
                "photo":"http://wxy.maixuexi.cn/img.jpg",
                "isUse":1,
                "createTime":"2016-12-20 12:12:00"
            }
        ]
    }
}
```

#### 2.9 查询指定学校教师列表接口
> 需要token验证
> 只有超级管理员（idType=2）能够查询其他sid的教师列表，学校管理员（idType=1）则不用传sid，直接从token中获取该学校的教师列表；
> isUse字段代表的意思为该用户的该角色是否启用，所以需要在对应角色表中查询该状态，默认为1启用状态；
> 需要获取用户是否具有主观题权限；
``` json
Url:        /mstudy/school/teacher?sid=[sid]&page=[page]&size=[size]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'sid':                学校id    ---不传则获取token中sid
    - 'page':               分页参数，不填则默认返回第一页
    - 'size':               每页显示数据量，不填则默认每页12条
    - 'token':              token
Response:  
    - 'status'：    0->ok
                    1->学校不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
    - 'page':               分页参数，不填则默认返回第一页
    - 'size':               每页显示数据量，不填则默认每页12条
    - 'teacherList':        教师列表json
    - 'uid':                用户id
    - 'tid':                教师id
    - 'teacherName':        教师姓名
    - 'letter':             姓名汉语拼音首字母
    - 'email':              邮箱
    - 'phone':              手机号
    - 'account':            账号
    - 'photo':              头像
    - 'idType':             用户类型：0-普通教师；1-学校管理员；2-超级管理员；
    - 'subPermission':      用户主观题权限：0-无权限；1-有权限；
    - 'isUse':              是否启用：0-否；1-启用；
    - 'createTime':         创建时间   
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "page":1,
        "size":12,
        "count":200,
        "teacherList":[
            {
                "uid":1,
                "tid":1,
                "teacherName":"zhansansan",
                "letter":"Z",
                "email":"123@mxx.com",
                "phone":"15123212352",
                "account":"15123212352",
                "photo":"http://wxy.maixuexi.cn/img.jpg",
                "idType":1,
                "subPermission":1,
                "isUse":1,
                "createTime":"2016-12-20 12:12:00"
            },
            {
                "uid":1,
                "tid":1,
                "teacherName":"zhansansan",
                "letter":"Z",
                "email":"123@mxx.com",
                "phone":"15123212352",
                "account":"15123212352",
                "photo":"http://wxy.maixuexi.cn/img.jpg",
                "idType":1,
                "subPermission":1,
                "isUse":1,
                "createTime":"2016-12-20 12:12:00"
            }
        ]
    }
}
```
#### 2.9.1 查询指定教师权限接口
> 需要token验证
> 需要根据token中的tid查询用户权限
``` json
Url:        /mstudy/teacher/permission?token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'token':              token
Response:  
    - 'status'：    0->ok
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface 
    - 'subPermission':      用户主观题权限：0-无权限；1-有权限；
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "subPermission":1
    }
}
```

#### 2.9.2 修改指定教师权限接口
> 需要token验证
> 只有系统管理员（idType=2）的用户才能够有权限修改；
``` json
Url:        /mstudy/teacher/permission?token=[token]
Method:     PUT 
Header:     Content-type:application/json
Parameter:
    - 'tid':                token
    - 'subPermission':      用户主观题权限：0-无权限；1-有权限；
    - 'token':              token
实例：
{
    "tid":1,
    "subPermission":1
}
Response:  
    - 'status'：    0->ok
                    1->tid不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface 
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```

#### 2.10 查询指定学校家长列表接口
> 需要token验证
> 只有超级管理员（idType=2）能够查询其他sid的家长列表，学校管理员（idType=1）则不用传sid，直接从token中获取该学校的家长列表；
> isUse字段代表的意思为该用户的该角色是否启用，所以需要在对应角色表中查询该状态，默认为1启用状态；
``` json
Url:        /mstudy/school/parent?sid=[sid]&page=[page]&size=[size]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'sid':                学校id    ---不传则获取token中sid
    - 'page':               分页参数，不填则默认返回第一页
    - 'size':               每页显示数据量，不填则默认每页12条
    - 'token':              token
Response:  
    - 'status'：    0->ok
                    1->学校不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
    - 'page':               分页参数，不填则默认返回第一页
    - 'size':               每页显示数据量，不填则默认每页12条
    - 'parentList':         家长列表json
    - 'uid':                用户id
    - 'parentId':           家长id
    - 'parentName':         家长姓名
    - 'letter':             姓名汉语拼音首字母
    - 'email':              邮箱
    - 'phone':              手机号
    - 'account':            账号
    - 'photo':              头像
    - 'isUse':              是否启用：0-否；1-启用；
    - 'createTime':         创建时间   
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "page":1,
        "size":12,
        "count":200,
        "parentList":[
            {
                "uid":1,
                "parentId":1,
                "parentName":"zhansansan",
                "letter":"Z",
                "email":"123@mxx.com",
                "phone":"15123212352",
                "account":"15123212352",
                "photo":"http://wxy.maixuexi.cn/img.jpg",
                "isUse":1,
                "createTime":"2016-12-20 12:12:00"
            },
            {
                "uid":1,
                "parentId":1,
                "parentName":"zhansansan",
                "letter":"Z",
                "email":"123@mxx.com",
                "phone":"15123212352",
                "account":"15123212352",
                "photo":"http://wxy.maixuexi.cn/img.jpg",
                "isUse":1,
                "createTime":"2016-12-20 12:12:00"
            }
        ]
    }
}
```

#### 2.11 学生信息增删改查相关

##### 2.11.1 查询学生详细信息接口
> 需要token验证
> 
``` json
Url:        /mstudy/student?studentId=[studentId]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'studentId':          学生id     ---必填项
    - 'token':              token
Response:  
    - 'status'：    0->ok
                    1->学生id不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
    - 'uid':                用户id
    - 'studentId':          学生id
    - 'studentName':        学生姓名
    - 'number':             学号
    - 'studentCode':        学籍号
    - 'letter':             姓名汉语拼音首字母
    - 'email':              邮箱
    - 'phone':              手机号
    - 'account':            账号
    - 'photo':              头像
    - 'createTime':         创建时间    
    - 'dateOfSchool':       入学日期
    - 'studentType':        学生类别
    - 'nation':             民族
    - 'nativePlace':        籍贯
    - 'cardType':           证件类型 
    - 'cardNumber':         证件号
    - 'political':          政治面貌
    - 'isOverseas':         是否港澳台侨胞   0-否；1-是
    - 'dateOfBirth':        出生日期
    - 'residenceType':      户口类型
    - 'residenceAddress':   户口地址    
    - 'isUse':              是否启用：0-否；1-启用；
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "uid":1,
        "studentId":1,
        "studentName":"zhansansan",
        "number":"01928293",
        "studentCode":"18373839238393",
        "letter":"Z",
        "email":"123@mxx.com",
        "phone":"15123212352",
        "account":"15123212352",
        "photo":"http://wxy.maixuexi.cn/img.jpg",
        "createTime":"2016-12-20 12:12:00",
        "dateOfSchool": "234324242",       
        "studentType": "分配",            
        "nation": "汉",                    
        "nativePlace": "湖北武汉",          
        "cardType": "身份证",                        
        "cardNumber": "142523521652",      
        "political": "中共党员",           
        "isOverseas": "0",                 
        "dateOfBirth": "1465228800",       
        "residenceType": "13987723451",    
        "residenceAddress": "新春一街12号",
        "isUse":1
    }
}
```

##### 2.11.2 新增学生信息接口
> 需要token验证
> 
``` json
Url:        /mstudy/student?token=[token]
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'token':              token
    - 'studentName':        学生姓名  ---必填
    - 'number':             学号
    - 'studentCode':        学籍号
    - 'letter':             姓名汉语拼音首字母
    - 'email':              邮箱---选填（手机、邮箱、账号必须填至少一个）
    - 'phone':              手机号---选填（手机、邮箱、账号必须填至少一个）
    - 'account':            账号---选填（手机、邮箱、账号必须填至少一个）
    - 'photo':              头像
    - 'dateOfSchool':       入学日期
    - 'studentType':        学生类别
    - 'nation':             民族
    - 'nativePlace':        籍贯
    - 'cardType':           证件类型 
    - 'cardNumber':         证件号
    - 'political':          政治面貌
    - 'isOverseas':         是否港澳台侨胞   0-否；1-是
    - 'dateOfBirth':        出生日期
    - 'residenceType':      户口类型
    - 'residenceAddress':   户口地址    
实例：
{
    "studentName":"zhansansan",
    "number":"01928293",
    "studentCode":"18373839238393",
    "letter":"Z",
    "email":"123@mxx.com",
    "phone":"15123212352",
    "account":"15123212352",
    "photo":"http://wxy.maixuexi.cn/img.jpg",
    "dateOfSchool": "234324242",       
    "studentType": "分配",            
    "nation": "汉",                    
    "nativePlace": "湖北武汉",          
    "cardType": "身份证",                        
    "cardNumber": "142523521652",      
    "political": "中共党员",           
    "isOverseas": "0",                 
    "dateOfBirth": "1465228800",       
    "residenceType": "13987723451",    
    "residenceAddress": "新春一街12号"
}
Response:  
    - 'status'：    0->ok
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
    
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```

##### 2.11.3 修改学生信息接口
> 需要token验证
> 手机号、邮箱、账号不允许调用此接口修改
``` json
Url:        /mstudy/student?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'token':              token
    - 'studentId':          学生id
    - 'studentName':        学生姓名  
    - 'number':             学号
    - 'studentCode':        学籍号
    - 'letter':             姓名汉语拼音首字母
    - 'photo':              头像
    - 'dateOfSchool':       入学日期
    - 'studentType':        学生类别
    - 'nation':             民族
    - 'nativePlace':        籍贯
    - 'cardType':           证件类型 
    - 'cardNumber':         证件号
    - 'political':          政治面貌
    - 'isOverseas':         是否港澳台侨胞   0-否；1-是
    - 'dateOfBirth':        出生日期
    - 'residenceType':      户口类型
    - 'residenceAddress':   户口地址   
实例：
{
     "studentId":1,
     "studentName":"zhansansan",
     "number":"01928293",
     "studentCode":"18373839238393",
     "letter":"Z",
     "photo":"http://wxy.maixuexi.cn/img.jpg",
     "dateOfSchool": "234324242",       
     "studentType": "分配",            
     "nation": "汉",                    
     "nativePlace": "湖北武汉",          
     "cardType": "身份证",                        
     "cardNumber": "142523521652",      
     "political": "中共党员",           
     "isOverseas": "0",                 
     "dateOfBirth": "1465228800",       
     "residenceType": "13987723451",    
     "residenceAddress": "新春一街12号"
}
Response:  
    - 'status'：    0->ok
                    1->修改失败，学生不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
    
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```
##### 2.11.4 删除学生信息接口
> 需要token验证
> 支持删除多个学生
> 如果学生有关联相关家长，则解除相关联关系
``` json
Url:        /mstudy/student?token=[token]
Method:     DELETE
Header:     Content-type:application/json
Parameter:
    - 'token':              token
    - 'studentId':          学生id
实例：
{
     "studentId":[1,2,3,4]
}
Response:  
    - 'status'：    0->ok
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
    
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```

#### 2.12 教师信息增删改查相关
##### 2.12.1 查询教师详细信息接口
> 需要token验证
``` json
Url:        /mstudy/teacher?tid=[tid]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'tid':                教师id    ---必填参数
    - 'token':              token
Response:  
    - 'status'：    0->ok
                    1->教师id不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
    - 'uid':                用户id
    - 'tid':                教师id
    - 'teacherName':        教师姓名
    - 'letter':             姓名汉语拼音首字母
    - 'email':              邮箱
    - 'phone':              手机号
    - 'account':            账号
    - 'photo':              头像
    - 'idType':             用户类型：0-普通教师；1-学校管理员；2-超级管理员；
    - 'isUse':              是否启用：0-否；1-启用；
    - 'gender':             教师性别（0-男；1-女；2-保密；）
    - 'dateOfBirth':        出生年月
    - 'startWorkT':         开始工作时间
    - 'jobNumber':          工号
    - 'cardType':           证件类型
    - 'cardNumber':         证件号码
    - 'homeAddress':        家庭住址
    - 'createTime':         创建时间 
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "uid":1,
        "tid":1,
        "teacherName":"zhansansan",
        "letter":"Z",
        "email":"123@mxx.com",
        "phone":"15123212352",
        "account":"15123212352",
        "photo":"http://wxy.maixuexi.cn/img.jpg",
        "idType":1,
        "isUse":1,
        "gender":1,                       
        "dateOfBirth":"2011-7-8 00:00:00",         
        "startWorkT":"2011-7-8 00:00:00",       
        "jobNumber":"0189302",             
        "cardType":1,                    
        "cardNumber":"425222522223252112",  
        "homeAddress":"斯里兰卡",             
        "createTime":"2016-12-20 12:12:00"
    }
}
```

##### 2.12.2 新增教师信息接口
> 需要token验证
``` json
Url:        /mstudy/teacher?token=[token]
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'token':              token
    - 'teacherName':        教师姓名   ---必填
    - 'letter':             姓名汉语拼音首字母
    - 'email':              邮箱 ---可选（手机号、邮箱、账号三选一）
    - 'phone':              手机号 ---可选（手机号、邮箱、账号三选一）
    - 'account':            账号 ---可选（手机号、邮箱、账号三选一）
    - 'photo':              头像
    - 'gender':             教师性别（0-男；1-女；2-保密；）
    - 'dateOfBirth':        出生年月
    - 'startWorkT':         开始工作时间
    - 'jobNumber':          工号
    - 'cardType':           证件类型
    - 'cardNumber':         证件号码
    - 'homeAddress':        家庭住址
实例：
{
    "teacherName":"zhansansan",
    "letter":"Z",
    "email":"123@mxx.com",
    "phone":"15123212352",
    "account":"15123212352",
    "photo":"http://wxy.maixuexi.cn/img.jpg",
    "gender":1,                       
    "dateOfBirth":"2011-7-8 00:00:00",         
    "startWorkT":"2011-7-8 00:00:00",       
    "jobNumber":"0189302",             
    "cardType":1,                    
    "cardNumber":"425222522223252112",  
    "homeAddress":"斯里兰卡"
}
Response:  
    - 'status'：    0->ok
                    1->教师已经存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```
##### 2.12.3 修改教师信息接口
> 需要token验证
> 手机号、邮箱、账号不允许调用此接口修改
``` json
Url:        /mstudy/teacher?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'tid':                教师id    ---必填参数
    - 'token':              token
    - 'teacherName':        教师姓名
    - 'letter':             姓名汉语拼音首字母
    - 'photo':              头像；
    - 'isUse':              是否启用：0-否；1-启用；
    - 'gender':             教师性别（0-男；1-女；2-保密；）
    - 'dateOfBirth':        出生年月
    - 'startWorkT':         开始工作时间
    - 'jobNumber':          工号
    - 'cardType':           证件类型
    - 'cardNumber':         证件号码
    - 'homeAddress':        家庭住址
实例：
{
     "tid":1,
     "teacherName":"zhansansan",
     "letter":"Z",
     "photo":"http://wxy.maixuexi.cn/img.jpg",
     "gender":1,                       
     "dateOfBirth":"2011-7-8 00:00:00",         
     "startWorkT":"2011-7-8 00:00:00",       
     "jobNumber":"0189302",             
     "cardType":1,                    
     "cardNumber":"425222522223252112",  
     "homeAddress":"斯里兰卡"
}
Response:  
    - 'status'：    0->ok
                    1->教师id不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface

```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```
##### 2.12.4 删除教师信息接口
> 需要token验证
> 支持删除多个
``` json
Url:        /mstudy/teacher?token=[token]
Method:     DELETE
Header:     Content-type:application/json
Parameter:
    - 'tid':                教师id    ---必填参数
    - 'token':              token
实例：
{
     "tid":[1,2,3]
}
Response:  
    - 'status'：    0->ok
                    1->教师id不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface

```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```

#### 2.13 家长信息增删改查相关
##### 2.13.1 查询家长详细信息接口
> 需要token验证
``` json
Url:        /mstudy/parent?parentId=[parentId]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'parentId':           家长id    ---必填参数
    - 'token':              token
Response:  
    - 'status'：    0->ok
                    1->家长id不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
    - 'uid':                用户id
    - 'parentId':           家长id
    - 'parentName':         家长姓名
    - 'letter':             姓名汉语拼音首字母
    - 'email':              邮箱
    - 'phone':              手机号
    - 'account':            账号
    - 'photo':              头像
    - 'workUnit':           工作单位
    - 'position':           职位
    - 'isUse':              是否启用：0-否；1-启用；
    - 'createTime':         创建时间   
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "uid":1,
        "parentId":1,
        "parentName":"zhansansan",
        "letter":"Z",
        "email":"123@mxx.com",
        "phone":"15123212352",
        "account":"15123212352",
        "photo":"http://wxy.maixuexi.cn/img.jpg",
        "workUnit":"某某公司",        
        "position":"工程师",  
        "isUse":1,            
        "createTime":"2016-12-20 12:12:00"
    }
}
```

##### 2.13.2 新增家长信息接口
> 需要token验证
``` json
Url:        /mstudy/parent?token=[token]
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'token':              token
    - 'parentName':         家长姓名  ---必填参数
    - 'letter':             姓名汉语拼音首字母
    - 'email':              邮箱 ---可选（手机号、邮箱、账号三选一）
    - 'phone':              手机号 ---可选（手机号、邮箱、账号三选一）
    - 'account':            账号 ---可选（手机号、邮箱、账号三选一）
    - 'photo':              头像
    - 'workUnit':           工作单位
    - 'position':           职位
实例：
{
    "parentName":"zhansansan",
    "letter":"Z",
    "email":"123@mxx.com",
    "phone":"15123212352",
    "account":"15123212352",
    "photo":"http://wxy.maixuexi.cn/img.jpg",
    "workUnit":"某某公司",        
    "position":"工程师"
}
Response:  
    - 'status'：    0->ok
                    1->家长已经存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```

##### 2.13.3 修改家长信息接口
> 需要token验证
> 手机号、邮箱、账号不允许调用此接口修改
``` json
Url:        /mstudy/parent?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'parentId':           家长id    ---必填参数
    - 'token':              token
    - 'parentName':         家长姓名
    - 'letter':             姓名汉语拼音首字母
    - 'photo':              头像
    - 'workUnit':           工作单位
    - 'position':           职位
实例：
{
    "parentId":1,
    "parentName":"zhansansan",
    "letter":"Z",
    "photo":"http://wxy.maixuexi.cn/img.jpg",
    "workUnit":"某某公司",        
    "position":"工程师"
}
Response:  
    - 'status'：    0->ok
                    1->家长id不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```

##### 2.13.4 删除家长信息接口
> 需要token验证
> 可删除多个
``` json
Url:        /mstudy/parent?token=[token]
Method:     DELETE
Header:     Content-type:application/json
Parameter:
    - 'parentId':           家长id    ---必填参数
    - 'token':              token
实例：
{
    "parentId":[1,2,3]
}
Response:  
    - 'status'：    0->ok
                    1->家长id不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```

#### 2.14.1 查询指定学生的家长列表接口
> 需要token验证
``` json
Url:        /mstudy/student/parent?studentId=[studentId]&type=[type]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'studentId':          学生id    ---必填参数
    - 'type':               家长类型：0-返回所有有效的家长，1-返回所有家长，不传默认为返回所有有效的家长
    - 'token':              token
Response:  
    - 'status'：    0->ok
                    1->学生id不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
    - 'id':                 学生家长关联表id
    - 'uid':                家长uid
    - 'parentId':           家长id
    - 'parentName':         家长姓名
    - 'letter':             姓名汉语拼音首字母
    - 'email':              邮箱
    - 'phone':              手机号
    - 'account':            账号
    - 'photo':              头像
    - 'workUnit':           工作单位
    - 'position':           职位
    - 'isUse':              是否启用：0-否；1-启用；
    - 'role':               家长角色（1-爸爸;2-妈妈;3-爷爷;4-奶奶;5-其他）
    - 'isDefault':          是否为主家长（0-否；1-是；） 主家长只能有一个
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "count":3,
        "parentList":[
            {
                "id":1,
                "uid":1,
                "parentId":1,
                "parentName":"zhansansan",
                "letter":"Z",
                "email":"123@mxx.com",
                "phone":"15123212352",
                "account":"15123212352",
                "photo":"http://wxy.maixuexi.cn/img.jpg",
                "workUnit":"某某公司",        
                "position":"工程师",  
                "isUse":1,             
                "role":"1",              
                "isDefault":"1"
            },
            {
                "id":1,
                "uid":1,
                "parentId":1,
                "parentName":"zhansansan",
                "letter":"Z",
                "email":"123@mxx.com",
                "phone":"15123212352",
                "account":"15123212352",
                "photo":"http://wxy.maixuexi.cn/img.jpg",
                "workUnit":"某某公司",        
                "position":"工程师",  
                "isUse":1,             
                "role":"1",              
                "isDefault":"1"
            }
        ]
    }
}
```
#### 2.14.2 新增指定学生家长接口
> 需要token验证
>首先需要选择某个学生，给予特定学生新增家长（只允许一次添加一个家长），家长手机在系统中是唯一的，不能重复，添加时需检查user表中是否存在如果存在就获取此家长信息，直接与该学生绑定即可，需要判断是否已经和该学生绑定过，新增成功后即可创建家长与学生的绑定关系，默认创建第一个家长为主家长账号，新增其他家长isDefault值为0，可以通过设定接口修改某一个家长账号为主账号；需要同时插入parent表和parent_student关联表的数据；初始isUse字段缺省为启用状态
>手机号、邮箱、账号必须同时系统唯一，否则不允许添加成功，三项选一项填即可；
``` json
Url:        /mstudy/student/parent?token=[token]
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'studentId':          学生id
    - 'token':              token
    - 'parentName':         家长姓名
    - 'letter':             姓名汉语拼音首字母
    - 'email':              邮箱   （手机号、邮箱、账号必须填至少一个）
    - 'phone':              手机号   （手机号、邮箱、账号必须填至少一个）
    - 'account':            账号   （手机号、邮箱、账号必须填至少一个）
    - 'photo':              头像
    - 'workUnit':           工作单位
    - 'position':           职位
    - 'role':               家长角色（1-爸爸;2-妈妈;3-爷爷;4-奶奶;5-其他）
实例：
{
    "studentId":23,
    "parentName":"张强",     
    "letter":"A",          
    "phone":"15235256525",   
    "email":"15235256@qq.525",    
    "account":"15235256525",             
    "workUnit":"咚咚咚某某公司",        
    "position":"工程师",  
    "photo":"url",                  
    "role":1
}
Response:  
    - 'status'：    0->ok
                    1->学生id不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```

#### 2.14.3 修改指定学生家长接口
> 需要token验证
``` json
Url:        /mstudy/student/parent?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'id':                 学生家长关联表id
    - 'studentId':          学生id
    - 'token':              token
    - 'parentName':         家长姓名
    - 'letter':             姓名汉语拼音首字母
    - 'email':              邮箱   （手机号、邮箱、账号必须填至少一个）
    - 'phone':              手机号   （手机号、邮箱、账号必须填至少一个）
    - 'account':            账号   （手机号、邮箱、账号必须填至少一个）
    - 'photo':              头像
    - 'workUnit':           工作单位
    - 'position':           职位
    - 'role':               家长角色（1-爸爸;2-妈妈;3-爷爷;4-奶奶;5-其他）
实例：
{
    "id":2,
    "studentId":23,
    "parentName":"张强",     
    "letter":"A",             
    "workUnit":"咚咚咚某某公司",        
    "position":"工程师",  
    "photo":"url",                  
    "role":1
}
Response:  
    - 'status'：    0->ok
                    1->数据id不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```
#### 2.14.4 删除指定学生家长接口（此接口删除，和2.14.8重复）
> 需要token验证
``` json
Url:        /mstudy/student/parent?token=[token]
Method:     DELETE
Header:     Content-type:application/json
Parameter:
    - 'id':                 学生家长关联表id
    - 'token':              token
实例：
{
    "id":[1,2]
}
Response:  
    - 'status'：    0->ok
                    1->数据id不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```
#### 2.14.5 设定指定学生家长停用（启用）功能接口
> 需要token验证
``` json
Url:        /mstudy/student/parent/use/set?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'id':                 学生家长关联表id
    - 'isUse':              是否启用：0-否；1-启用；
    - 'token':              token
实例：
{
    "isUse":1,
    "id":[1,2,3]
}
Response:  
    - 'status'：    0->ok
                    1->数据id不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```

#### 2.14.6 指定学生家长为主家长账号接口
>需要token验证
``` json
Url:        /mstudy/student/parent/default/set?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'id':                 学生家长关联表id
    - 'isDefault':          是否为主家长（0-否；1-是；） 学生主家长只能有一个
    - 'token':              token
实例：
{
    "id":1,
    "isDefault":1
}
Response:  
    - 'status'：    0->ok
                    1->数据id不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```

#### 2.14.7 查询指定家长的学生列表接口
> 需要token验证
``` json
Url:        /mstudy/parent/student?parentId=[parentId]&type=[type]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'parentId':           家长id    ---必填参数
    - 'type':               类型：0-返回所有有效的数据，1-返回所有数据，不传默认为返回所有有效的数据
    - 'token':              token
Response:  
    - 'status'：    0->ok
                    1->家长id不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
    - 'id':                 学生家长关联表id
    - 'uid':                学生uid
    - 'studentId':          学生id
    - 'studentName':        学生姓名
    - 'letter':             姓名汉语拼音首字母
    - 'email':              邮箱
    - 'phone':              手机号
    - 'account':            账号
    - 'photo':              头像
    - 'number':             学号
    - 'studentCode':        学籍号
    - 'isUse':              是否启用：0-否；1-启用；
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "count":3,
        "studentList":[
            {
                "id":1,
                "uid":1,
                "studentId":1,
                "studentName":"zhansansan",
                "letter":"Z",
                "email":"123@mxx.com",
                "phone":"15123212352",
                "account":"15123212352",
                "photo":"http://wxy.maixuexi.cn/img.jpg",
                "number":"01928293",
                "studentCode":"18373839238393",
                "isUse":1
            },
            {
                "id":1,
                "uid":1,
                "studentId":1,
                "studentName":"zhansansan",
                "letter":"Z",
                "email":"123@mxx.com",
                "phone":"15123212352",
                "account":"15123212352",
                "photo":"http://wxy.maixuexi.cn/img.jpg",
                "number":"01928293",
                "studentCode":"18373839238393",
                "isUse":1
            }
        ]
    }
}
```
#### 2.14.8 删除指定家长学生关联关系接口
> 需要token验证
``` json
Url:        /mstudy/parent/student?token=[token]
Method:     DELETE
Header:     Content-type:application/json
Parameter:
    - 'id':                 学生家长关联表id
    - 'token':              token
实例：
{
    "id":[1,2]
}
Response:  
    - 'status'：    0->ok
                    1->数据id不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```

#### 2.14.9 创建指定家长学生关联关系接口
> 需要token验证
``` json
Url:        /mstudy/parent/student/relation?token=[token]
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'parentId':           家长id
    - 'studentId':          学生id
    - 'role':               家长角色
    - 'token':              token
实例：
{
    "parentId":1,
    "studentId":2,             
    "role":1
}
Response:  
    - 'status'：    0->ok
                    1->学生id不存在
                    2->家长id不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```

### 2.15 标签管理相关
>标签管理作为系统通用标签管理，分为公共标签和私有标签，公共标签只能由超级管理员来维护，并且，其他学校均可查看，但不允许编辑及删除操作；私有标签为学校自己私有的标签，每个学校均可自行创建自己的标签，只能自己管理；
>标签有标签类型：tag_type字段：1-阶段；2-年级；3-课程；4-其他；1、2、3属于公共标签类型，新增该类型下的标签只能由系统超级管理员（idType=2）处理；4-其他任何学校可以创建该类型下的标签；

#### 2.15.1 开放性公有标签根据标签id和类型获取关联标签查询接口
>该接口不需要token验证
>该接口获取的所有数据都为有效启用的
>可应用于根据阶段获取该阶段的年级列表
``` json
Url:        /mstudy/tag/public?tagId=[tagId]&tagType=[tagType]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'tagId':      标签id   ---选填项
    - 'tagType':    标签类型：1-阶段；2-年级；3-课程；4-其他；--必填项 传0则获取所有的开放标签
Response:  
    - 'status'：    0->ok
                    1->标签类型不存在
                    2->标签id不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
    - 'tagId':      标签id  
    - 'tagName':    标签名字  
    - 'tagExplain': 标签说明  
    - 'tagType':    标签类型：1-阶段；2-年级；3-课程；4-其他；
    - 'parentTagId':父级标签id  
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "count":10,
        "tagList":[
            {
                "tagId":1,
                "tagName":"小学",
                "tagExplain":"说明",
                "tagType":1,
                "parentTagId":2
            },
            {
                "tagId":1,
                "tagName":"小学",
                "tagExplain":"说明",
                "tagType":1,
                "parentTagId":2
            }
        ]
    }
}
```
#### 2.15.1.1 获取开放性公有性质阶段年级标签集合接口
>该接口不需要token验证
>该接口获取的所有数据都为有效启用的
>可应用于根据阶段获取该阶段的年级列表
``` json
Url:        /mstudy/tag/public/stage/grade
Method:     GET
Header:     Content-type:application/json
Parameter:
    - '无':      
Response:  
    - 'status'：    0->ok
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
    - 'count':           标签数量  
    - 'stageList':       阶段标签json 
    - 'tagId':           标签id   
    - 'tagName':         标签名字    
    - 'tagExplain':      标签说明  
    - 'tagType':         标签类型：1-阶段；2-年级；3-课程；4-其他；
    - 'parentTagId':     父级标签id  
    - 'gradeList':       年级标签列表  
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "count":10,
        "stageList":[
            {
                "tagId":1,
                "tagName":"小学",
                "tagExplain":"说明",
                "tagType":1,
                "parentTagId":2,
                "count":2,
                "gradeList":[
                    {
                        "tagId":2,
                        "tagName":"一年级",
                        "tagExplain":"说明",
                        "tagType":2,
                        "parentTagId":1,
                    },
                    {
                        "tagId":2,
                        "tagName":"一年级",
                        "tagExplain":"说明",
                        "tagType":2,
                        "parentTagId":1,
                    }
                ]
            },
            {
                "tagId":1,
                "tagName":"小学",
                "tagExplain":"说明",
                "tagType":1,
                "parentTagId":2,
                "count":2,
                "gradeList":[
                    {
                        "tagId":2,
                        "tagName":"一年级",
                        "tagExplain":"说明",
                        "tagType":2,
                        "parentTagId":1,
                    },
                    {
                        "tagId":2,
                        "tagName":"一年级",
                        "tagExplain":"说明",
                        "tagType":2,
                        "parentTagId":1,
                    }
                ]
            }
        ]
    }
}
```

#### 2.15.2 标签管理列表查询接口
> 需要token验证
> 需要从token中获取sid并获取相应的标签，其中isPublic字段标记为1则为公共标签，全部学校可以获取，如果标记为0则为私有，则只能自己学校获取到；
> 公共标签只有超级管理员才能创建，普通学校管理员只能创建私有标签；
> 公有标签有阶段、年级、课程、其他标签类型，私有标签只有其他类型
``` json
Url:        /mstudy/tag?isPublic=[isPublic]&tagType=[tagType]&isUse=[isUse]&page=[page]&size=[size]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'tagType':            标签类型：标签类型：1-阶段；2-年级；3-课程；4-其他；   ---必填
    - 'isPublic':           是否为公共标签：0-否；1-是； ---必填
    - 'isUse':              是否启用：0-否；1-是；    不填则默认为启用
    - 'page':               分页参数，不填则默认返回第一页
    - 'size':               每页显示数据量，不填则默认每页12条
    - 'token':              token
Response:  
    - 'status'：    0->ok
                    1->标签类型不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
    - 'tagId':      标签id  
    - 'tagName':    标签名字  
    - 'tagExplain': 标签说明  
    - 'tagType':    标签类型：1-阶段；2-年级；3-课程；4-其他；
    - 'parentTagId':父级标签id  
    - 'isPublic':   是否为公共标签：0-否；1-是；  
    - 'isUse':      是否启用：0-否；1-是；  
    - 'createTime': 创建时间 
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "page":1,
        "size":10,
        "count":10,
        "tagList":[
            {
                "tagId":1,
                "tagName":"一年级",
                "tagExplain":"说明",
                "tagType":1,
                "isPublic":1,
                "parentTagId":2,
                "isUse":1,
                "createTime":"2016-12-22 12:12:22"
            },
            {
                "tagId":1,
                "tagName":"一年级",
                "tagExplain":"说明",
                "tagType":1,
                "isPublic":1,
                "parentTagId":2,
                "isUse":1,
                "createTime":"2016-12-22 12:12:22"
            }
        ]
    }
}
```

#### 2.15.3 新增标签接口
> 需要token验证
> 只有超级管理员（idType=2）能够对公共标签进行管理（增删改）；
> 普通管理员（idType=1）只能够创建isPublic=0并且tagType=4的标签；
> 标签名机构+公有标签下唯一
``` json
Url:        /mstudy/tag?token=[token]
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'tagType':            标签类型：标签类型：1-阶段；2-年级；3-课程；4-其他； ---必填
    - 'isPublic':           是否为公共标签：0-否；1-是； ---必填
    - 'tagName':            标签名字
    - 'tagExplain':         标签说明
    - 'parentTagId':        父级标签id,支持多选
    - 'token':              token
实例：
{
    "isPublic":0,
    "tagType":4,
    "tagName":"这是一个自定义标签",
    "tagExplain":"对这个标签的解释说明",
    "parentTagId":[1,2,3]
}
Response:  
    - 'status'：    0->ok
                    1->父级标签已存在
                    2->标签名字在该机构下已存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```

#### 2.15.4 修改标签接口
> 需要token验证
``` json
Url:        /mstudy/tag?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'tagId':              标签id
    - 'tagType':            标签类型：标签类型：1-阶段；2-年级；3-课程；4-其他； ---必填
    - 'isPublic':           是否为公共标签：0-否；1-是； ---必填
    - 'tagName':            标签名字
    - 'tagExplain':         标签说明
    - 'parentTagId':        父级标签id,支持多选
    - 'token':              token
实例：
{
    "tagId":1,
    "isPublic":0,
    "tagType":4,
    "tagName":"这是一个自定义标签",
    "tagExplain":"对这个标签的解释说明",
    "parentTagId":18
}
Response:  
    - 'status'：    0->ok
                    1->父级标签已存在
                    2->标签名字在该机构下已存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```

#### 2.15.5 删除标签接口
> 需要token验证
> 被删除标签如果有子标签则不允许删除
``` json
Url:        /mstudy/tag?token=[token]
Method:     DELETE
Header:     Content-type:application/json
Parameter:
    - 'tagId':              标签id
    - 'token':              token
实例：
{
    "tagId":[1,2,3]
}
Response:  
    - 'status'：    0->ok
                    1->标签不存在
                    2->该标签下存在子标签
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```


#### 2.15.6 教师标签列表查询接口
> 需要token验证
``` json
Url:        /mstudy/tag/teacher?tid=[tid]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'tid':                教师id
    - 'token':              token
Response:  
    - 'status'：    0->ok
                    1->教师id不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
    - 'id':         教师标签关联表id  
    - 'tagId':      标签id  
    - 'tagName':    标签名字  
    - 'tagExplain': 标签说明  
    - 'tagType':    标签类型：1-阶段；2-年级；3-课程；4-其他；
    - 'isPublic':   是否为公共标签：0-否；1-是；
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "count":5,
        "tagList":[
            {
                "id":1,
                "tagId":1,
                "tagName":"aaa",
                "tagExplain":"bbb",
                "isPublic":1,
                "tagType":2
            },
            {
                "id":2,
                "tagId":1,
                "tagName":"小学",
                "tagExplain":"说明",
                "tagType":1,
                "count":2,
                "childList":[
                    {
                        "id":3,
                        "tagId":2,
                        "tagName":"一年级",
                        "tagExplain":"说明",
                        "tagType":2,
                        "parentTagId":1,
                    },
                    {
                        "id":4,
                        "tagId":2,
                        "tagName":"一年级",
                        "tagExplain":"说明",
                        "tagType":2,
                        "parentTagId":1,
                    }
                ]
            },
            {
                "id":5,
                "tagId":1,
                "tagName":"中学",
                "tagExplain":"说明",
                "tagType":1,
                "count":2,
                "childList":[
                    {
                        "id":6,
                        "tagId":2,
                        "tagName":"一年级",
                        "tagExplain":"说明",
                        "tagType":2,
                        "parentTagId":1,
                    },
                    {
                        "id":7,
                        "tagId":2,
                        "tagName":"一年级",
                        "tagExplain":"说明",
                        "tagType":2,
                        "parentTagId":1,
                    }
                ]
            }
        ]
    }
}
```

#### 2.15.7 教师标签新增接口
> 需要token验证
``` json
Url:        /mstudy/tag/teacher?token=[token]
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'tagId':              标签id
    - 'tid':                教师
    - 'token':              token
实例：
{
    "tagId":[1,2],
    "tid":[3,5]
}
Response:  
    - 'status'：    0->ok
                    1->标签id不存在
                    2->教师不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```

#### 2.15.8 教师标签删除接口 
> 需要token验证
``` json
Url:        /mstudy/tag/teacher?token=[token]
Method:     DELETE
Header:     Content-type:application/json
Parameter:
    - 'id':                 标签教师关联表id
    - 'token':              token
实例：
{
    "id":[1,3]
}
Response:  
    - 'status'：    0->ok
                    1->标签id不存在
                    2->教师不存在
                    11->token is invalid
                    12->token is timeout
                    13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"ok"
    }
}
```



### 9 批量导入相关接口
#### 9.1 学生批量导入接口
> 需要token验证


#### 9.2 学生家长批量导入接口
> 需要token验证


#### 9.3 教师批量导入接口
> 需要token验证



