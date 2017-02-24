[toc]

## 3.麦学习2.1版本接口说明

@(千校云)

> 标签： 麦学习2.1版本接口

---

### 文档更新说明
> 更新时间：2016-12-19

### 页面说明
> 



### 3 班级管理相关接口

#### 3.1 查询指定班级详细信息接口
> 查询班级信息
> 需要使用Token验证查询者身份
> 返回值新增ownerTid(班级拥有教师tid),teacherName(班级拥有教师名字)，变更joinTime字段为takeOverTime；
``` json
Url:        /mstudy/class/info?cid=[cid]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - `cid`:      查询的班级cid
    - 'token':    token
Response:
    - `status`： 0->OK 
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
    - 'createTid'：  创建教师tid
    - 'ownerTid'：   班级拥有教师tid
    - 'teacherName'：班级拥有教师名字
    - 'sid'：        学校id
    - 'className':   班级名称
    - 'maxNumber':   班级人数上限
    - 'password':    班级密码
    - 'sort':        指定排序
    - 'remarkName':  班级备注
    - 'qrCode':      班级二维码
    - 'takeOverTime':新老师接管班级时间
    - 'userNumber':  班级人数统计
    - 'dataStatus':  状态：-1-删除；1-正常；
    - 'createTime':  创建时间
    - 'updateTime':  更新时间
    - 'cid':         班级id
```

> **返回结果示例：**

``` json
{
    "status":1,
    "data":{
        "cid":1,
        "sid":1,
        "createTid":1,
        "ownerTid":1,
        "teacherName":"kkk",
        "className":"mstudy",
        "maxNumber":1,
        "password":"passwd",
        "remarkName":"study",
        "qrCode":"此处是二维码",
        "sort":1,
        "takeOverTime":"2016-8-9 00:00:00",
        "userNumber":1,
        "dataStatus":1,
        "createTime":"2016-8-9 00:00:00",
        "updateTime":"2016-8-9 00:00:00"
    }
}
```
#### 3.2 初始化一个班级接口
> 教师后台\机构后台 创建班级 (需同时写入class_teacher关联信息)
> 需要使用Token验证查询者身份

``` json
Url:        /mstudy/class?token=[token]
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'createTid'：  创建者tid
    - 'className':   班级名称
    - 'maxNumber':   班级人数上限
    - 'password':    班级密码
    - 'remarkName':  班级备注
    - 'token'：      token
 
示例：
{
    "createTid":1,
    "className":"mstudy",
    "maxNumber":1,
    "password":"passwd",
    "remarkName":"study"
}
Response:
    - `status`：0->is OK
                1->tid不存在
               11->token is invalid
               12->token is timeout
               13->Illegal interface calls, not power use interface
```

> **返回结果示例：**

``` json
{
    "status": "0",
    "data":{
        "info":"ok",
        "cid":1
    }
}
```

#### 3.3 查询指定学生用户班级列表接口
> 用于学生端在当前机构下的班级列表
> 需要使用Token验证查询者身份
> 需要从token中获取sid，查询该学生在该机构下的班级列表
> 返回值新增ownerTid(班级拥有教师tid),teacherName(班级拥有教师名字)，变更joinTime字段为takeOverTime；
``` json
Url:        /mstudy/student/class?studentId=[studentId]&page=[page]&size=[size]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'studentId': 学生id ---必填项
    - 'token':     token
    - 'page'：     page对应的分页编号数字，当page＝0或者page为空时，返回第一页
    - 'size'：     size对应的每页数据条数，当size＝0或者size为空时，默认一页返回12条
Response:
    - 'status'： 0->OK
                1-> 学生id不存在 
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
    - 'createTid'：  创建者教师tid
    - 'ownerTid'：   班级拥有者教师tid
    - 'teacherName'：班级拥有者教师姓名
    - 'sid'：        学校id
    - 'cid':         班级id
    - 'className':   班级名称
    - 'maxNumber':   班级人数上限
    - 'userNumber':  班级人数统计
    - 'password':    班级密码
    - 'remarkName':  班级备注
    - 'qrCode':      班级二维码
    - 'sort':        指定排序
    - 'dataStatus':  状态（-1-删除；1-正常；）
    - 'takeOverTime':    新老师接管班级时间
    - 'createTime':  创建时间
    - 'updateTime':  更新时间
```

> **返回结果示例：**

``` json
{
    "status": 0,
    "data": {
        "page":0,   
        "size":14,   
        "count":30,
        "classList": [
            {
                "createTid":1,
                "ownerTid":1,
                "teacherName":"name",
                "sid":1,
                "cid":1,
                "className":"mstudy",
                "maxNumber":1,
                "userNumber":1,
                "password":"passwd",
                "remarkName":"study",
                "qrCode":"此处是二维码",
                "sort":1,
                "dataStatus":1,
                "takeOverTime":"2016-8-9 00:00:00",
                "createTime":"2016-8-9 00:00:00",
                "updateTime":"2016-8-9 00:00:00"
            },
            {
                "createTid":1,
                "ownerTid":1,
                "teacherName":"name",
                "sid":1,
                "cid":1,
                "className":"mstudy",
                "maxNumber":1,
                "userNumber":1,
                "password":"passwd",
                "remarkName":"study",
                "qrCode":"此处是二维码",
                "sort":1,
                "dataStatus":1,
                "takeOverTime":"2016-8-9 00:00:00",
                "createTime":"2016-8-9 00:00:00",
                "updateTime":"2016-8-9 00:00:00"
            }
        ]
    }
}
```
#### 3.4 查询指定助教班级列表接口
> 用于学生端助教身份查询在当前机构下班级列表
> 需要使用Token验证查询者身份
> 返回值新增ownerTid(班级拥有教师tid),teacherName(班级拥有教师名字)，变更joinTime字段为takeOverTime；
``` json
Url:        /mstudy/tutor/class?uid=[uid]&userType=[userType]&page=[page]&size=[size]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'uid':       用户uid ---必填项
    - 'userType':  用户类型（1-教师；2-家长；3-学生） ---必填项
    - 'token':     token
    - 'page'：     page对应的分页编号数字，当page＝0或者page为空时，返回第一页
    - 'size'：     size对应的每页数据条数，当size＝0或者size为空时，默认一页返回12条
Response:
    - 'status'： 0->OK
                 4-> 用户类型不存在 
                 5-> 用户uid 不存在    
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
    - 'createTid'：  创建者教师tid
    - 'ownerTid'：   班级拥有者教师tid
    - 'teacherName'：班级拥有者教师姓名
    - 'sid'：        学校id
    - 'cid':         班级id
    - 'className':   班级名称
    - 'maxNumber':   班级人数上限
    - 'userNumber':  班级人数统计
    - 'password':    班级密码
    - 'remarkName':      班级备注
    - 'qrCode':      班级二维码
    - 'sort':        指定排序
    - 'dataStatus':  状态（-1-删除；1-正常；）
    - 'joinTime':    新老师接管班级时间
    - 'createTime':  创建时间
    - 'updateTime':  更新时间
```

> **返回结果示例：**

``` json
{
    "status": 0,
    "data": {
        "page":0,   
        "size":14,   
        "count":30,
        "classList": [
            {
                "createTid":1,
                "ownerTid":1,
                "teacherName":"name",
                "sid":1,
                "cid":1,
                "className":"mstudy",
                "maxNumber":1,
                "userNumber":1,
                "password":"passwd",
                "remarkName":"study",
                "qrCode":"此处是二维码",
                "sort":1,
                "dataStatus":1,
                "takeOverTime":"2016-8-9 00:00:00",
                "createTime":"2016-8-9 00:00:00",
                "updateTime":"2016-8-9 00:00:00"
            },
            {
                "createTid":1,
                "ownerTid":1,
                "teacherName":"name",
                "sid":1,
                "cid":1,
                "className":"mstudy",
                "maxNumber":1,
                "userNumber":1,
                "password":"passwd",
                "remarkName":"study",
                "qrCode":"此处是二维码",
                "sort":1,
                "dataStatus":1,
                "takeOverTime":"2016-8-9 00:00:00",
                "createTime":"2016-8-9 00:00:00",
                "updateTime":"2016-8-9 00:00:00"
            }
        ]
    }
}
```
#### 3.5 查询指定机构班级列表接口
> 用于在当前机构下所有有效的班级列表
> 需要使用Token验证查询者身份
> 需要说明
> sid参数需要默认和token中sid一致，如果是平台管理员（idType=2）则可以不一致；
> 返回值新增ownerTid(班级拥有教师tid),teacherName(班级拥有教师名字)，变更joinTime字段为takeOverTime；
``` json
Url:        /mstudy/class/list?sid=[sid]&page=[page]&size=[size]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'sid':        学校id ---必填项
    - 'token':      token
    - 'page'：     page对应的分页编号数字，当page＝0或者page为空时，返回第一页
    - 'size'：     size对应的每页数据条数，当size＝0或者size为空时，默认一页返回12条
Response:
    - 'status'：0->OK
                1-> sid不存在    
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
    - 'createTid'：  创建者教师tid
    - 'ownerTid'：   班级拥有者教师tid
    - 'teacherName'：班级拥有者教师姓名
    - 'sid'：        学校id
    - 'cid':         班级id
    - 'className':   班级名称
    - 'maxNumber':   班级人数上限
    - 'userNumber':  班级人数统计
    - 'password':    班级密码
    - 'remarkName':      班级备注
    - 'qrCode':      班级二维码
    - 'sort':        指定排序
    - 'dataStatus':  状态（-1-删除；1-正常；）
    - 'takeOverTime':    新老师接管班级时间
    - 'createTime':  创建时间
    - 'updateTime':  更新时间
```

> **返回结果示例：**

``` json
{
    "status": 0,
    "data": {
        "page":0,   
        "size":14,   
        "count":30,
        "classList": [
            {
                "createTid":1,
                "ownerTid":1,
                "teacherName":"name",
                "sid":1,
                "cid":1,
                "className":"mstudy",
                "maxNumber":1,
                "userNumber":1,
                "password":"passwd",
                "remarkName":"study",
                "qrCode":"此处是二维码",
                "sort":1,
                "dataStatus":1,
                "takeOverTime":"2016-8-9 00:00:00",
                "createTime":"2016-8-9 00:00:00",
                "updateTime":"2016-8-9 00:00:00"
            },
            {
                "createTid":1,
                "ownerTid":1,
                "teacherName":"name",
                "sid":1,
                "cid":1,
                "className":"mstudy",
                "maxNumber":1,
                "userNumber":1,
                "password":"passwd",
                "remarkName":"study",
                "qrCode":"此处是二维码",
                "sort":1,
                "dataStatus":1,
                "takeOverTime":"2016-8-9 00:00:00",
                "createTime":"2016-8-9 00:00:00",
                "updateTime":"2016-8-9 00:00:00"
            }
        ]
    }
}
```
#### 3.6 删除班级接口
> 用于教师后台班级管理
> 需要使用Token验证查询者身份
> 支持（单、多）
``` json
Url:        /mstudy/class?token=[token]
Method:     DELETE
Header:     Content-type:application/json
Parameter:
    - 'cid'：          cid --必填
    - 'token'：        token
示例：
{
    "cid":[1,2]
}
Response:
    - `status`：0->is OK
                1->删除失败，cid不存在
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
####3.7 修改班级信息接口
> 用于教师班级管理
> 需要使用Token验证查询者身份

``` json
Url:        /mstudy/class?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'cid'：        cid--必填项
    - 'token'：      token--必填项
    - 'className':   班级名称
    - 'maxNumber':   班级人数上限
    - 'password':    班级密码
    - 'remarkName':  班级备注名
示例：
{
    "cid":1,
    "className":"mstudy",
    "maxNumber":1,
    "password":"passwd",
    "remarkName":"study"
}
Response:
    - 'status'：0->OK
                7->无效的班级id
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

####3.8 根据班级名称搜索班级接口
> 用于学生端加入班级
> 需要使用Token验证查询者身份
> 返回值新增ownerTid(班级拥有教师tid),teacherName(班级拥有教师名字)，变更joinTime字段为takeOverTime；
``` json
Url:        /mstudy/class/search?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'token'：      token
    - 'page'：       分页参数，不传默认为第一页
    - 'size'：       每页数据量，不传则默认12条
    - 'className':   班级名称

示例：
{
    "className":"Andy",
    "page":1,
    "size":12
}
Response:
    - 'status'：0->OK
                1->找不到相关班级
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
    - 'createTid'：  创建者教师tid
    - 'ownerTid'：   班级拥有者教师tid
    - 'teacherName'：班级拥有者教师姓名
    - 'sid'：        学校id
    - 'cid':         班级id
    - 'className':   班级名称
    - 'maxNumber':   班级人数上限
    - 'userNumber':  班级人数统计
    - 'needPassword':加入班级是否需要班级密码：0-否；1-是；
    - 'remarkName':  班级备注
    - 'qrCode':      班级二维码
    - 'sort':        指定排序
    - 'isJoin':      是否加入班级：0-未加入；1-已加入；
    - 'dataStatus':  状态（-1-删除；1-正常；）
    - 'takeOverTime':新老师接管班级时间
    - 'createTime':  创建时间
    - 'updateTime':  更新时间
```

> **返回结果示例：**

``` json
{
    "status": 0,
    "data": {
        "page":0,   
        "size":14,   
        "count":30,
        "classList": [
            {
                "createTid":1,
                "ownerTid":1,
                "teacherName":"name",
                "sid":1,
                "cid":1,
                "className":"mstudy",
                "maxNumber":1,
                "userNumber":1,
                "needPassword":0,
                "remarkName":"study",
                "qrCode":"jhijhkj099889",
                "sort":1,
                "isJoin":1,
                "dataStatus":1,
                "takeOverTime":"2016-8-9 00:00:00",
                "createTime":"2016-8-9 00:00:00",
                "updateTime":"2016-8-9 00:00:00"
            },
            {
                "createTid":1,
                "ownerTid":1,
                "teacherName":"name",
                "sid":1,
                "cid":1,
                "className":"mstudy",
                "maxNumber":1,
                "userNumber":1,
                "needPassword":0,
                "remarkName":"study",
                "qrCode":"此处是二维码",
                "sort":1,
                "isJoin":1,
                "dataStatus":1,
                "takeOverTime":"2016-8-9 00:00:00",
                "createTime":"2016-8-9 00:00:00",
                "updateTime":"2016-8-9 00:00:00"
            }
        ]
    }
}
```
####3.8.1 根据班级id获取班级基本信息接口
> 用于学生端\教师后台班级管理或者加入班级时展示班级
> 需要使用Token验证查询者身份
> 该接口会返回是否需要班级密码字段
> 返回值新增用户是否在该班级里的字段（isJoin）:0-未加入；1-已加入；用于学生搜索到班级获取班级详情时判断是否需要加入班级；教师角色暂不用返回
``` json
Url:        /mstudy/class/public/info?cid=[cid]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'token'：      token
    - 'cid'：        班级id
Response:
    - 'status'：0->OK
                1->找不到相关班级
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
    - 'createTid'：  创建者教师tid
    - 'ownerTid'：   班级拥有者教师tid
    - 'teacherName'：班级拥有者教师姓名
    - 'sid'：        学校id
    - 'cid':         班级id
    - 'className':   班级名称
    - 'maxNumber':   班级人数上限
    - 'userNumber':  班级人数统计
    - 'needPassword':加入班级是否需要班级密码：0-否；1-是；
    - 'remarkName':  班级备注
    - 'qrCode':      班级二维码
    - 'sort':        指定排序
    - 'isJoin':      是否加入班级：0-未加入；1-已加入；
    - 'dataStatus':  状态（-1-删除；1-正常；）
    - 'takeOverTime':新老师接管班级时间
    - 'createTime':  创建时间
    - 'updateTime':  更新时间
```

> **返回结果示例：**

``` json
{
    "status":0,
    "data":{
        "createTid":1,
        "ownerTid":1,
        "teacherName":"name",
        "sid":1,
        "cid":1,
        "className":"mstudy",
        "maxNumber":1,
        "userNumber":1,
        "needPassword":0,
        "remarkName":"study",
        "qrCode":"此处是二维码",
        "sort":1,
        "isJoin":1,
        "dataStatus":1,
        "takeOverTime":"2016-8-9 00:00:00",
        "createTime":"2016-8-9 00:00:00",
        "updateTime":"2016-8-9 00:00:00"
    }
}
```

#### 3.9 查询指定班级指定用户基本信息接口
>用于教师后台班级学生管理
> 需要使用Token验证查询者身份

``` json
Url:        /mstudy/class/student?cid=[cid]&studentId=[studentId]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'token'：             token--必填项
    - 'studentId'：         学生id-必填项
    - 'cid':                班级id-必填项
Response:
    - 'status'：0->OK
                1-cid不存在   
                9- studentId不存在 
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
    - 'studentId'：     学生id
    - 'studentName'：   学生姓名
    - 'phone'：         学生电话
    - 'number'：        学生学号
    - 'remarkName'：    学生班级备注姓名
    - 'photo'：         学生头像
    - 'className':      班级名
    - 'link':           邀请链接
    - 'joinTime':       加入时间
    - 'dataStatus':     学生状态：（-1-踢出；1-正常；2-黑名单；）
    - 'createTime':     创建时间
    - 'updateTime':     更新时间
    - 'cid':            班级id
```

> **返回结果示例：**

``` json
{
    "status": 0,
    "data": {
        "cid":1,
        "studentId":1,
        "studentName":"Andy",
        "photo":"www.maixuexi.cn/sss.jpg",
        "phone":"15152525235",
        "number":"123456",
        "remarkName":"Andy",
        "className":"1班Andy",
        "link":"http://mstudy.me/xxx.png",
        "joinTime":"2016-8-9 00:00:00",
        "dataStatus":1,
        "createTime": "2016-8-9 00:00:00",
        "updateTime": "2016-8-9 00:00:00"
    }
}
```
#### 3.10 查询指定班级所有正常用户列表接口
> 用于教师后台班级学生管理
> 需要使用Token验证查询者身份

``` json
Url:        /mstudy/class/student/list?cid=[cid]&page=[page]&size=[size]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'token'：     token--必填项
    - 'cid':        班级id-必填项
    - 'page'：      page对应的分页编号数字，当page＝0或者page为空时，返回第一页
    - 'size'：      size对应的每页数据条数，当size＝0或者size为空时，默认一页返回12条

Response:
    - 'status'： 0->OK
                1-cid不存在   
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
    - 'studentId'：     学生id
    - 'studentName'：   学生姓名
    - 'phone'：         学生电话
    - 'number'：        学生学号
    - 'className':      班级名
    - 'link':           邀请链接
    - 'joinTime':       加入时间
    - 'dataStatus':     学生状态：（-1-踢出；1-正常；2-黑名单；）
    - 'createTime':     创建时间
    - 'updateTime':     更新时间
    - 'cid':            班级id
```

> **返回结果示例：**

``` json
{
    "status":0,
    "data":{
        "page":0,   
        "size":14,   
        "count":3,
        "studentList": [
            {
                "cid":1,
                "studentId":1,
                "studentName":"Andy",
                "photo":"www.maixuexi.cn/sss.jpg",
                "phone":"15152525235",
                "number":"123456",
                "remarkName":"Andy",
                "className":"1班Andy",
                "link":"http://mstudy.me/xxx.png",
                "joinTime":"2016-8-9 00:00:00",
                "dataStatus":1,
                "createTime": "2016-8-9 00:00:00",
                "updateTime": "2016-8-9 00:00:00"
            },
            {
                "cid":1,
                "studentId":1,
                "studentName":"Andy",
                "photo":"www.maixuexi.cn/sss.jpg",
                "phone":"15152525235",
                "number":"123456",
                "remarkName":"Andy",
                "className":"1班Andy",
                "link":"http://mstudy.me/xxx.png",
                "joinTime":"2016-8-9 00:00:00",
                "dataStatus":1,
                "createTime": "2016-8-9 00:00:00",
                "updateTime": "2016-8-9 00:00:00"
            }
        ]
    }
}
```
####3.11 根据姓名搜索指定班级的班级学生接口
>用于教师后台班级学生管理
> 需要使用Token验证查询者身份

``` json
Url:        /mstudy/class/student/search?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'token'：       token--必填项
    - 'studentName'： 学生名字
    - 'cid':          班级-必填项
    - 'page'：      page对应的分页编号数字，当page＝0或者page为空时，返回第一页
    - 'size'：      size对应的每页数据条数，当size＝0或者size为空时，默认一页返回12条

示例：
{
    "studentName":"Andy",
    "cid":1,
    "page":1,
    "size":12
}
Response:
    - 'status'：0->OK
                1-cid不存在
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
    - 'studentId'：     学生id
    - 'studentName'：   学生姓名
    - 'className':      班级名
    - 'link':           邀请链接
    - 'joinTime':       加入时间
    - 'dataStatus':     学生状态：（-1-踢出；1-正常；2-黑名单；）
    - 'createTime':     创建时间
    - 'updateTime':     更新时间
    - 'cid':            班级id
```

> **返回结果示例：**

``` json
{
    "status":0,
    "data":{
        "page":0,   
        "size":14,   
        "count":3,
        "studentList": [
            {
                "cid":1,
                "studentId":1,
                "studentName":"Andy",
                "photo":"www.maixuexi.cn/sss.jpg",
                "remarkName":"Andy",
                "className":"1班Andy",
                "link":"http://mstudy.me/xxx.png",
                "joinTime":"2016-8-9 00:00:00",
                "dataStatus":1,
                "createTime": "2016-8-9 00:00:00",
                "updateTime": "2016-8-9 00:00:00"
            },
            {
                "cid":1,
                "studentId":1,
                "studentName":"Andy",
                "photo":"www.maixuexi.cn/sss.jpg",
                "remarkName":"Andy",
                "className":"1班Andy",
                "link":"http://mstudy.me/xxx.png",
                "joinTime":"2016-8-9 00:00:00",
                "dataStatus":1,
                "createTime": "2016-8-9 00:00:00",
                "updateTime": "2016-8-9 00:00:00"
            }
        ]
    }
}
```

####3.12 初始化班级学生接口
> 用于学生端/教师后台扫码加入班级
> 需要使用Token验证查询者身份
> 考虑是否需要一个根据二维码获取班级详情的接口
> (首次加入所在班级的机构,需要在student表增加在该机构下的学籍数据)该场景应用于开放式班级的情况，目前暂不需要
``` json
Url:        /mstudy/class/student?token=[token]
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'studentId'：     学生id
    - 'password':       班级密码
    - 'cid':            班级id
    - 'token'：         token
 
示例：
{
    "cid":1,
    "studentId":2,
    "password":"123456"
}

Response:
    - `status`：0->is OK
                1->cid不存在
                2->班级密码不正确
                3->学生id不存在
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
####3.13 修改班级学生信息接口
> 用于教师后台班级学生管理
> 需要使用Token验证查询者身份
``` json
Url:        /mstudy/class/student?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'studentId'：      学生id
    - 'studentName'：    学生姓名
    - 'nickname':        呢称
    - 'link':            邀请链接
    - 'cid':             班级id
    - 'token'：          token
示例：
{
    "cid":1,
    "studentId":2,
    "remarkName":"Andyming"
}

Response:
    - 'status'：0->OK
                1-cid不存在
                1-学生id不存在
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
#### 3.14 班级黑名单学生管理接口
> 用于教师后台班级学生管理，用户可以被拉入黑名单，如果被恢复的话就会标记删除该用户，如果解除黑名单，则该用户会被标记为踢出，需要重新加入班级才能变成正常状态
> 需要使用Token验证查询者身份
``` json
Url:        /mstudy/class/student/blackList?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'cid'：      cid --必填
    - 'studentId': 学生id--必填
    - 'dataStatus': 用户状态:（-1:踢出；1:正常；2:黑名单；）
    - 'token'：    token
示例：
{
    "cid":1,
    "studentId":1,
    "dataStatus":3
}
Response:
    - 'status'：0->is OK
                1-cid不存在
                2-studentId不存在 
               11->token is invalid
               12->token is timeout
               13->Illegal interface calls, not power use interface
```
> **返回结果示例：**
``` json
{
    "status": "0",
    "data":{
        "info":"ok"
    }
}
```
#### 3.15 班级黑名单学生列表查询接口
> 用于教师后台班级学生管理
> 需要使用Token验证查询者身份
``` json
Url:        /mstudy/class/student/blackList?cid=[cid]&page=[page]&size=[size]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'token'：     token--必填项
    - 'cid':        班级id-必填项
    - 'page'：      page对应的分页编号数字，当page＝0或者page为空时，返回第一页
    - 'size'：      size对应的每页数据条数，当size＝0或者size为空时，默认一页返回12条
    - 
Response:
    - 'status'： 0->OK
                1-cid不存在   
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
    - 'studentId'：     学生id
    - 'studentName'：   学生姓名
    - 'className':      班级名
    - 'link':           邀请链接
    - 'joinTime':       加入时间
    - 'dataStatus':     学生状态：（-1-踢出；1-正常；2-黑名单；）
    - 'createTime':     创建时间
    - 'updateTime':     更新时间
    - 'cid':            班级id
```

> **返回结果示例：**

``` json
{
    "status":0,
    "data":{
        "page":0,   
        "size":14,   
        "count":3,
        "studentList": [
            {
                "cid":1,
                "studentId":1,
                "studentName":"Andy",
                "photo":"www.maixuexi.cn/sss.jpg",
                "remarkName":"Andy",
                "className":"1班Andy",
                "link":"http://mstudy.me/xxx.png",
                "joinTime":"2016-8-9 00:00:00",
                "dataStatus":-1,
                "createTime": "2016-8-9 00:00:00",
                "updateTime": "2016-8-9 00:00:00"
            },
            {
                "cid":1,
                "studentId":1,
                "studentName":"Andy",
                "photo":"www.maixuexi.cn/sss.jpg",
                "remarkName":"Andy",
                "className":"1班Andy",
                "link":"http://mstudy.me/xxx.png",
                "joinTime":"2016-8-9 00:00:00",
                "dataStatus":-1,
                "createTime": "2016-8-9 00:00:00",
                "updateTime": "2016-8-9 00:00:00"
            }
        ]
    }
}
```

#### 3.16 查询班级教师列表接口
> 用于教师后台班级管理
> 需要使用Token验证查询者身份
>可考虑之后加入教师科目标签

``` json
Url:        /mstudy/class/teacher/list?cid=[cid]&page=[page]&size=[size]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'token'：     token--必填项
    - 'cid':        班级id-必填项
    - 'page'：      page对应的分页编号数字，当page＝0或者page为空时，返回第一页
    - 'size'：      size对应的每页数据条数，当size＝0或者size为空时，默认一页返回12条
Response:
    - 'status'： 0->OK
                1-cid不存在   
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
    - 'cid': 班级id
    - 'tid': 教师id
    - 'sid': 机构id
    - 'teacherName': 教师姓名
    - 'phone': 教师手机号
    - 'email': 教师邮箱
    - 'account': 教师账号
    - 'uid': 教师的用户id
    - 'photo': 教师头像
    - 'isOwner': 是否是班级创建者：0-否；1-是；
    - 'isCheck': 布置作业是否允许其他人批改：0-否；1-是；
    - 'isShow': 布置作业是否仅自己可见：0-否；1-是；
    - 'isAgree': 邀请状态 0-有邀请通知;1-已接收邀请;2-拒绝邀请;3-未被邀请状态;
    - 'dataStatus': 状态（-1-删除；1-正常；）
    - 'createTime': 创建时间
    - 'updateTime': 最近更新时间
```

> **返回结果示例：**

``` json
{
    "status":0,
    "data":{
        "count":132,
        "page":13,
        "size":13,
        "teacherList":[
            {
                "sid":1,
                "cid":1,
                "uid":56,
                "tid":56,
                "teacherName":"56",
                "phone":"152352652",
                "email":"1523@qq.52652",
                "account":"1523ss",
                "photo":"56",
                "isOwner":0,
                "isShow":0,
                "isCheck":1,
                "isAgree":0,
                "dataStatus":1, 
                "createTime":"2016-08-09 00:00:00",
                "updateTime":"2016-08-09 00:00:00"
            },
            {
                "sid":1,
                "cid":1,
                "uid":56,
                "tid":56,
                "teacherName":"56",
                "phone":"152352652",
                "email":"1523@qq.52652",
                "account":"1523ss",
                "photo":"56",
                "isOwner":0,
                "isShow":0,
                "isCheck":1,
                "isAgree":0,
                "dataStatus":1, 
                "createTime":"2016-08-09 00:00:00",
                "updateTime":"2016-08-09 00:00:00"
            }
        ] 
    } 
}
```

#### 3.16.1 根据教师账号搜索教师信息接口
> 用于教师后台班级管理
> 需要使用Token验证查询者身份
> 该接口只能用于精确搜索，不能模糊查询
> 只能搜索自己当前登录机构下的教师账号

``` json
Url:        /mstudy/teacher/info?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'token'：     token--必填项
    - 'phone':      手机账号
    - 'email'：     邮箱账号
    - 'account'：   用户名账号
Response:
    - 'status'：0->OK
                1->账号不存在   
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
    - 'tid': 教师id
    - 'uid': 用户id
    - 'teacherName': 教师姓名
    - 'phone': 教师手机号
    - 'email': 教师邮箱
    - 'account': 教师账号
    - 'photo': 教师头像
```

> **返回结果示例：**

``` json
{
    "status":0,
    "data":{
        "uid":13,
        "tid":13,
        "teacherName":"56",
        "phone":"152352652",
        "email":"1523@qq.52652",
        "account":"1523ss",
        "photo":"56"
    }
}
```

####3.17 邀请教师加入班级接口
> 用于教师后台班级教师管理
> 需要使用Token验证查询者身份

``` json
Url:        /mstudy/class/teacher/invite?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'cid': 班级id
    - 'tid': 教师id
    - 'token'：         token
    - 'page'：      page对应的分页编号数字，当page＝0或者page为空时，返回第一页
    - 'size'：      size对应的每页数据条数，当size＝0或者size为空时，默认一页返回12条
示例：
{
    "cid":72,
    "tid":56
}

Response:
    - 'status'：0->OK
                1-cid不存在
                2-tid不存在
                3-该教师已被邀请加入班级，不能重复申请
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
```

> **返回结果示例：**

``` json
{
    "status":0,
    "data": {
        "info":"ok"
    }
}
```

####3.18 被邀请教师同意/拒绝邀请操作接口
> 用于教师后台班级教师管理
> 需要使用Token验证查询者身份
> 此接口只需要控制（同意）1和（拒绝）2，如果是拒绝则接口需要将状态变为3（未被邀请状态）

``` json
Url:        /mstudy/class/teacher/reply?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'cid':      班级id
    - 'isAgree':  邀请状态 0-有邀请通知;1-已接收邀请;2-拒绝邀请;3-未被邀请状态;
    - 'token'：         token
示例：
{
    "cid":72,
    "isAgree":1,
}

Response:
    - 'status'：0->OK
                1-cid不存在
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

####3.18.1 权限设置班级教师删除接口
> 用于教师后台班级教师管理
> 需要使用Token验证查询者身份
> 班级拥有者（判定标准：isOwner:0-否，1-是）有权限删除目标班级的其他教师，非班级拥有者仅可以让自己退出目标班级；
``` json
Url:        /mstudy/class/teacher/delete?token=[token]
Method:     DELETE
Header:     Content-type:application/json
Parameter:
    - 'cid':      班级id    ---必填
    - 'tid':      教师id    ---必填
    - 'token'：   token
示例：
{
    "cid":72,
    "tid":1,
}

Response:
    - 'status'：0->OK
                1-目标cid不存在
                2-目标tid不存在
                3-用户不是班级拥有者，无法删除班级其他教师 
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

#### 3.19 获取教师角色拥有的班级列表查询接口
> 定义为我的班级
> 获取班级教师关联表中tid+isOwner为1的班级cid列表
> 班级教师为所有已接受邀请的教师
> 需要使用Token验证查询者身份

``` json
Url:        /mstudy/teacher/class/owner?tid=[tid]&page=[page]&size=[size]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'token'：            token--必填项
    - 'tid':               教师id-必填项
    - 'page'：      page对应的分页编号数字，当page＝0或者page为空时，返回第一页
    - 'size'：      size对应的每页数据条数，当size＝0或者size为空时，默认一页返回12条
Response:
    - 'status'： 0->OK
                1-cid不存在   
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
    - 'cid': 班级id
    - 'className': 班级名字
    - 'maxNumber': 班级允许加入最大人数
    - 'userNumber': 班级实际现有人数
    - 'password': 班级密码
    - 'createTid': 创建者教师id
    - 'teacherName': 创建者教师名字
    - 'teacherList': 教师列表
    - 'tid': 教师id
    - 'isOwner': 是否为班级拥有者：0-否；1-是；
```

> **返回结果示例：**

``` json
{
    "status":0,
    "data":{
        "count":12,
        "page":1,
        "size":12,
        "classList":[
            {
                "cid":1,
                "className":"班级名称",
                "maxNumber":20,
                "userNumber":12,
                "password":"123456",
                "createTid":1,
                "teacherName":"创建者名字",
                "teacherList":[
                    {
                        "tid":1,
                        "teacherName":"123",
                        "isOwner":1
                    },
                    {
                        "tid":1,
                        "teacherName":"123",
                        "isOwner":0
                    }
                ]
            },
            {
                "cid":1,
                "className":"班级名称",
                "maxNumber":20,
                "userNumber":12,
                "password":"123456",
                "createTid":1,
                "teacherName":"创建者名字",
                "teacherList":[
                    {
                        "tid":1,
                        "teacherName":"123",
                        "isOwner":1
                    },
                    {
                        "tid":1,
                        "teacherName":"123",
                        "isOwner":0
                    }
                ]
            }
        ]
    }
}
```

#### 3.20 获取教师加入的公共班级列表查询接口
> 用于教师后台班级管理
> 教师非班级拥有者的班级列表
> 需要使用Token验证查询者身份
> 如果isAgree=0,则需要调用同意与否的接口来操作，在此之前不能进入班级；
> 如果isAgree=1,则为同意状态，可以点击进入班级查看详情及进一步操作；
> 如果isAgree=2,则为拒绝状态，该列表不需要获取
> 如果isAgree=3,则为未被邀请状态，该列表不需要获取
``` json
Url:        /mstudy/teacher/class/public?tid=[tid]&page=[page]&size=[size]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'token'：            token--必填项
    - 'tid':               教师id-必填项
    - 'page'：      page对应的分页编号数字，当page＝0或者page为空时，返回第一页
    - 'size'：      size对应的每页数据条数，当size＝0或者size为空时，默认一页返回12条
Response:
    - 'status'：0->OK
                1-cid不存在   
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
    - 'cid': 班级id
    - 'className': 班级名字
    - 'maxNumber': 班级允许加入最大人数
    - 'userNumber': 班级实际现有人数
    - 'password': 班级密码
    - 'createTid': 创建者教师id
    - 'teacherName': 创建者教师名字
    - 'isAgree': 邀请状态 0-有邀请通知;1-已接收邀请;2-拒绝邀请;3-未被邀请状态;
    - 'teacherList': 教师列表
    - 'tid': 教师id
    - 'isOwner': 是否为班级拥有者：0-否；1-是；
```

> **返回结果示例：**

``` json
{
    "status":0,
    "data":{
        "count":12,
        "page":1,
        "size":12,
        "classList":[
            {
                "cid":1,
                "className":"班级名称",
                "maxNumber":20,
                "userNumber":12,
                "password":"123456",
                "createTid":1,
                "teacherName":"创建者名字",
                "isAgree":0,
                "teacherList":[
                    {
                        "tid":1,
                        "teacherName":"123",
                        "isOwner":1
                    },
                    {
                        "tid":1,
                        "teacherName":"123",
                        "isOwner":0
                    }
                ]
            },
            {
                "cid":1,
                "className":"班级名称",
                "maxNumber":20,
                "userNumber":12,
                "password":"123456",
                "createTid":1,
                "teacherName":"创建者名字",
                "isAgree":0,
                "teacherList":[
                    {
                        "tid":1,
                        "teacherName":"123",
                        "isOwner":1
                    },
                    {
                        "tid":1,
                        "teacherName":"123",
                        "isOwner":0
                    }
                ]
            }
        ]
    }
}
```

#### 3.21 教师通过班级名字搜索班级接口
> 用于教师后台班级管理
> 需要使用Token验证查询者身份
``` json
Url:        /mstudy/teacher/class/search?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'token'：     token--必填项
    - 'className':  班级名字
    - 'tid':        教师id
    - 'type':       查找的类型：0-我拥有的班级；1-公共班级；2-我的公共班级+我拥有的班级；
    - 'page'：      page对应的分页编号数字，当page＝0或者page为空时，返回第一页
    - 'size'：      size对应的每页数据条数，当size＝0或者size为空时，默认一页返回12条
实例：
{
    "className":"开始的看法",
    "tid":1,
    "type":1,
    "page":1,
    "size":12
}
Response:
    - 'status'：0->OK
                1-搜索的班级不存在   
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
    - 'cid': 班级id
    - 'className': 班级名字
    - 'maxNumber': 班级允许加入最大人数
    - 'userNumber': 班级实际现有人数
    - 'password': 班级密码
    - 'createTid': 创建者教师id
    - 'teacherName': 创建者教师名字
    - 'isAgree': 邀请状态 0-有邀请通知;1-已接收邀请;2-拒绝邀请;3-未被邀请状态;
    - 'teacherList': 教师列表
    - 'tid': 教师id
    - 'isOwner': 是否为班级拥有者：0-否；1-是；
```

> **返回结果示例：**

``` json
{
    "status":0,
    "data":{
        "count":12,
        "page":1,
        "size":12,
        "classList":[
            {
                "cid":1,
                "className":"班级名称",
                "maxNumber":20,
                "userNumber":12,
                "password":"123456",
                "createTid":1,
                "teacherName":"创建者名字",
                "isAgree":0,
                "teacherList":[
                    {
                        "tid":1,
                        "teacherName":"123",
                        "isOwner":1
                    },
                    {
                        "tid":1,
                        "teacherName":"123",
                        "isOwner":0
                    }
                ]
            },
            {
                "cid":1,
                "className":"班级名称",
                "maxNumber":20,
                "userNumber":12,
                "password":"123456",
                "createTid":1,
                "teacherName":"创建者名字",
                "isAgree":0,
                "teacherList":[
                    {
                        "tid":1,
                        "teacherName":"123",
                        "isOwner":1
                    },
                    {
                        "tid":1,
                        "teacherName":"123",
                        "isOwner":0
                    }
                ]
            }
        ]
    }
}
```

####3.22 班级拥有权限转给指定教师接口
>用于教师后台班级学生管理
> 需要使用Token验证查询者身份
> 需要验证token中用户是否为目标cid的拥有者，如果是才允许；
> 权限转让后，需要设置转让者为该班级的普通教师；
``` json
Url:        /mstudy/class/teacher/owner?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'cid': 需要转的班级id 
    - 'tid': 接收班级的教师tid
    - 'token'： token
示例：
{
    "cid":92,
    "tid":24
}

Response:
    - 'status'：0->OK
                1->cid不存在
                2->教师tid不存在
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
####3.23 班级教师权限设置接口
> 用于教师后台班级学生管理
> 需要使用Token验证查询者身份
> 每个班级教师均可以设置自己在该班级的权限

``` json
Url:        /mstudy/class/teacher/set?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'cid':      班级id 必选项
    - 'isCheck':  布置作业是否允许其他人批改：0-否；1-是；
    - 'isShow':   布置作业是否仅自己可见：0-否；1-是；
    - 'token'：   token
示例：
{
    "cid":92,
    "isShow":1,
    "isCheck":1
}
Response:
    - 'status'：0->OK
                1-cid不存在
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
#### 3.24 查询班级教师的助教信息接口
> 学生端/教师后台助教管理
> 需要使用Token验证查询者身份

``` json
Url:        /mstudy/class/teacher/tutor?cid=[cid]&tid=[tid]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'token'： token--必填项
    - 'cid':    班级id
    - 'tid':    教师id
Response:
    - 'status'：0->OK
                1->cid不存在
                2->tid不存在   
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
    - 'tutorList' :   助教列表
    - 'count' :       助教数量
    - 'toturId' :     助教id
    - 'cid' :         班级id
    - 'tid' :         教师id
    - 'uid' :         用户id
    - 'userType' :    用户类型（1-教师；2-家长；3-学生）
    - 'userName' :    用户名字
    - 'qcCode' :      二维码
    - 'createTime' :  创建时间
    - 'updateTime' :  更新时间    
```

> **返回结果示例：**

``` json
{
    "status":0,
    "data":{
        "count":2,
        "tutorList":[
            {
                "toturId":1,
                "cid":1,
                "tid":2,
                "uid":2,
                "userType":1,
                "userName":"张三",
                "qcCode":"skldjflaksd",
                "createTime":"2016-12-29 12:12:20",
                "updateTime":"2016-12-29 12:12:20"
            },
            {
                "toturId":1,
                "cid":1,
                "tid":2,
                "uid":2,
                "userType":1,
                "userName":"张三",
                "qcCode":"skldjflaksd",
                "createTime":"2016-12-29 12:12:20",
                "updateTime":"2016-12-29 12:12:20"
            }
        ]       
    }
}
```
####3.25 生成班级助教二维码接口
> 学生端/教师后台助教管理
> 需要使用Token验证查询者身份

``` json
Url:        /mstudy/class/tutor?token=[token]
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'cid':            班级id 必填参数
    - 'token'：         token
示例：
{
    "cid":1
}
Response:
    - 'status'：0->OK
                1-cid不存在
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
    - 'toturId' :     助教id
    - 'cid' :         班级id
    - 'tid' :         教师id
    - 'qcCode' :      二维码
    - 'createTime' :  创建时间
    - 'updateTime' :  更新时间 
```

> **返回结果示例：**

``` json
{
    "status":0,
    "data":{
        "toturId":1,
        "cid":1,
        "tid":2,
        "qcCode":"skldjflaksd",
        "createTime":"2016-12-29 12:12:20",
        "updateTime":"2016-12-29 12:12:20"
    }
}
```

####3.26 修改指定班级的助教接口
> 学生端/教师后台助教管理
> 需要使用Token验证查询者身份

``` json
Url:        /mstudy/class/tutor?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'toturId': 助教id 必填参数
    - 'uid': 用户id（1-教师；2-家长；3-学生）
    - 'userType':用户类型（1-教师；2-家长；3-学生）
    - 'token'： token 必填参数
示例：
{
    "toturId":1,
    "uid":1,
    "userType":1
}
Response:
    - 'status'：0->OK
                1->toturId不存在
                1->toturId已被绑定
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

####3.27 解除用户班级助教身份接口
> 学生端/教师后台助教管理
> 需要使用Token验证查询者身份

``` json
Url:        /mstudy/class/tutor/remove?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'toturId':  助教id 必填参数
    - 'token'：   token 必填参数
示例：
{
    "toturId":[1,2]
}
Response:
    - 'status'：0->OK
                1->toturId不存在
                1->toturId已被绑定
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

####3.28 删除班级助教数据接口
> 学生端/教师后台助教管理
> 需要使用Token验证查询者身份

``` json
Url:        /mstudy/class/tutor?token=[token]
Method:     DELETE
Header:     Content-type:application/json
Parameter:
    - 'toturId':  助教id 必填参数
    - 'token'：   token 必填参数
示例：
{
    "toturId":[1,2]
}
Response:
    - 'status'：0->OK
                1->还有助教与其绑定，请解绑后删除
                1->toturId已被绑定
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

### 4 试卷管理相关接口
>试题表需要考虑加一个试题材料关联表，目前不需要

####4.1.1 普通试题新增接口
> 用于教师后台录题 (与试题复合题相关操作需要传试卷pid , 未敲定的试题保存在Redis)
> 需要使用Token验证查询者身份
> 解析之后可以考虑新增知识点之类的name类型，可以选择知识点标签
> 可用于新增复合题的子题
``` json
Url:        /mstudy/test/question?token=[token]
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'pid'：  试卷id  --必填
    - 'qType': 试题类型:1-客观题；2-主观题；  --必填
    - 'qScore': 试题分数
    - 'mid':    材料题id
    - 'token'：token 必填项
 
示例：
{
    "pid":1,
    "qType":1,
    "qScore":1,
    "mid":1
}

Response:
    - 'status'：0->is OK
                1->pid不存在
               11->token is invalid
               12->token is timeout
               13->Illegal interface calls, not power use interface
    - 'qid'：       试题id
    - 'qContent'：  试题内容
    - 'mid'：       材料id
    - 'choices'：   试题选项：分为客观题（字符串依次对应的选项是0,1,2,3,...）/主观题（为[[1答题类型，2答题类型],[1分值，2分值]]）
    - 'qType'：     试题类型:1-客观题；2-主观题；  --必填
    - 'qAnswer'：   试题答案：分为客观题（正确答案从0开始依次递增：[正确答案0，正确答案1]）/主观题（分别是["第一个答案","第二个答案","第三个答案"]）
    - 'qResolve'：  试题解析：结构为{"标签1":"解析内容","标签2":"解析内容"}
    - 'createTime'：创建时间
    - 'updateTime'：更新时间
```

> **返回结果示例：**

``` json

当qType=1（客观题）时返回结果：
{
    "status":0,
    "data":{
        "qid":1,
        "mid":2,
        "qContent":"这里是内容",
        "choices":["111","222","22523",""],
        "qType":1,
        "qAnswer":[1],
        "qScore":0,
        "qResolve":{"xkxkd":"skdfkasjd","xkxkd":"skdfkasjd"},
        "createTime":"2016-12-28 12:12:20",
        "updateTime":"2016-12-28 12:12:20"
    }
}

当qType=2（主观题）时返回结果：
{
    "status":0,
    "data":{
        "qid":1,
        "mid":2,
        "qContent":"这里是内容",
        "choices":[[1,2],[2,3]],
        "qType":2,
        "qAnswer":["111","222","22523",""],
        "qScore":0,
        "qResolve":{"xkxkd":"skdfkasjd","xkxkd":"skdfkasjd"},
        "createTime":"2016-12-28 12:12:20",
        "updateTime":"2016-12-28 12:12:20"
    }
}

```
####4.1.2 复合材料题新增接口
> 用于教师后台录题 (与试题复合题相关操作需要传试卷pid , 未敲定的试题保存在Redis)
> 需要使用Token验证查询者身份
> 解析之后可以考虑新增知识点之类的name类型，可以选择知识点标签
``` json
Url:        /mstudy/test/material?token=[token]
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'pid'：   试卷id  --必填
    - 'token'： token 必填项
 
示例：
{
    "pid":1
}

Response:
    - 'status'：0->is OK
                1->pid不存在
               11->token is invalid
               12->token is timeout
               13->Illegal interface calls, not power use interface
    - 'mid'：       材料id
    - 'createTid'： 创建者教师id
    - 'mContent'：  材料内容
    - 'createTime'：创建时间
    - 'updateTime'：更新时间
```

> **返回结果示例：**

``` json
{
    "status":0,
    "data":{
        "mid":2,
        "createTid":2,
        "mContent":"这里是内容",
        "createTime":"2016-12-28 12:12:20",
        "updateTime":"2016-12-28 12:12:20"
    }
}
```

#### 4.2.1 普通试题详情查看接口
> 需要使用Token验证查询者身份

``` json
Url:        /mstudy/test/question?qid=[qid]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'token'：   token--必填项
    - 'qid' :     试题id
   
Response:
    - 'status'：0->OK  
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
    - 'qid'：       试题id
    - 'createTid'： 创建者tid
    - 'qDigest'：   试题唯一hash值
    - 'qContent'：  试题内容
    - 'mid'：       材料id
    - 'mContent'：  材料内容
    - 'choices'：   试题选项：分为客观题（字符串依次对应的选项是0,1,2,3,...）/主观题（为[[1答题类型，2答题类型],[1分值，2分值]]）
    - 'qType'：     试题类型:1-客观题；2-主观题；  --必填
    - 'qAnswer'：   试题答案：分为客观题（正确答案从0开始依次递增：[正确答案0，正确答案1]）/主观题（分别是["第一个答案","第二个答案","第三个答案"]）
    - 'qResolve'：  试题解析：结构为{"标签1":"解析内容","标签2":"解析内容"}
    - 'createTime'：创建时间
    - 'updateTime'：更新时间
```

> **返回结果示例：**

``` json
{
    "status":0,
    "data":{
        "qid":1,
        "createTid":2,
        "qDigest":"sdsfdadfasdfsdfsadfsdfsdfsdfsdf",
        "mid":2,
        "qType":1,
        "qContent":"这里是内容",
        "choices":["111","222","22523",""],
        "qAnswer":[1],
        "qScore":0,
        "qResolve":{"xkxkd":"skdfkasjd","xkxkd":"skdfkasjd"},
        "createTime":"2016-12-28 12:12:20",
        "updateTime":"2016-12-28 12:12:20"
	}      
}
```
#### 4.2.2 材料试题详情查看接口
> 需要使用Token验证查询者身份

``` json
Url:        /mstudy/test/material?mid=[mid]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'token'：   token--必填项
    - 'mid' :     材料试题id
   
Response:
    - 'status'：0->OK  
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
    - 'mid'：       试题id
    - 'createTid'： 创建者tid
    - 'mDigest'：   试题唯一hash值
    - 'mContent'：  材料试题内容
    - 'createTime'：创建时间
    - 'updateTime'：更新时间
```

> **返回结果示例：**

``` json
{
    "status":0,
    "data":{
        "mid":1,
        "createTid":2,
        "mDigest":"sdsfdadfasdfsdfsadfsdfsdfsdfsdf",
        "mContent":"这里是内容",
        "createTime":"2016-12-28 12:12:20",
        "updateTime":"2016-12-28 12:12:20"
	}      
}
```

#### 4.3.1 普通试题修改接口
> 用于教师后台录题修改试题，需要区分主观和客观题型；
> 需要使用Token验证查询者身份
``` json
Url:        /mstudy/test/question?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'qid'：       试题id
    - 'qContent'：  试题内容
    - 'choices'：   试题选项：分为客观题（字符串依次对应的选项是0,1,2,3,...）/主观题（为[[1答题类型，2答题类型],[1分值，2分值]]）
    - 'qType'：     试题类型:1-客观题；2-主观题；  --必填
    - 'qAnswer'：   试题答案：分为客观题（正确答案从0开始依次递增：[正确答案0，正确答案1]）/主观题（分别是["第一个答案","第二个答案","第三个答案"]）
    - 'qResolve'：  试题解析：结构为{"标签1":"解析内容","标签2":"解析内容"}
示例：
{
    "qid":1,
	"qContent":"这里是内容",
	"choices":["111","222","22523",""],
	"qAnswer":[1],
	"qScore":0,
	"qResolve":{"xkxkd":"skdfkasjd","xkxkd":"skdfkasjd"}
}
Response:
    - 'status'：0->OK
                1->qid不存在
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

#### 4.3.2 材料试题修改接口
> 用于教师后台录题修改试题，需要区分主观和客观题型；
> 需要使用Token验证查询者身份
``` json
Url:        /mstudy/test/material?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'mid'：       试题id
    - 'mContent'：  试题内容
示例：
{
    "mid":1,
	"mContent":"这里是内容"
}
Response:
    - 'status'：0->OK
                1->mid不存在
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

####  4.4.1  试卷试题删除接口
> 用于教师后台录题删除试题
> 需要使用Token验证查询者身份
> 该删除会删除redis中的试题及更新试卷struct结构中的json
> 新增mid参数，删除复合题子题的情况下会传该参数
``` json
Url:        /mstudy/test/paper/question?token=[token]
Method:     DELETE
Header:     Content-type:application/json
Parameter:
    - 'qid':  试题id 
    - 'mid':  试题材料id（需要删除复合题下面的子题的时候需要传该参数） 
    - 'pid':  试卷id 必填项
    - 'token'：        token
删除试卷试题示例：
{
    "pid":1,
    "qid":1,
    "mid":1
}
Response:
    - `status`：0->is OK
                1->pid不存在
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

####  4.4.2  试卷材料及试题删除接口
> 用于教师后台录题删除试题
> 需要使用Token验证查询者身份
> 该删除会删除redis中的试题材料及更新试卷struct结构中的json
> 暂时不支持删除多个材料题的
``` json
Url:        /mstudy/test/paper/material?token=[token]
Method:     DELETE
Header:     Content-type:application/json
Parameter:
    - 'mid':  材料id 
    - 'pid':  试卷id 必填项
    - 'token'：        token
删除试卷材料及材料包含的试题示例：
{
    "mid":2,
    "pid":1
}
Response:
    - `status`：0->is OK
                6->删除失败，无效的mid
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


#### 4.5  查询指定试卷信息接口
> 查询Paper表试卷信息
> 需要使用Token验证查询者身份
> 试卷隐藏状态是为已布置作业的试卷而又需要修改试卷做的标记，学生可以正常作答，教师无法编辑源试卷；
> 编辑中的试卷结构中的试题及材料id需要在redis中获取相应的试题json,而已敲定的试卷则需要根据对应的mysqlid获取试卷结构；
``` json
Url:        /mstudy/test/paper?pid=[pid]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'token'：     token   --必填项
    - 'pid' :       试卷id  --必填项
Response:
    - 'status'： 0->OK
                1-cid不存在   
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
	- 'pid' : 		试卷id
	- 'createTid': 	创建教师id
	- 'ownerTid': 	试卷所属教师id
    - 'teacherName':试卷所属教师姓名
	- 'paperName': 	试卷名称
	- 'paperStructJson': 试卷结构
	- 'qid': 		试题id
	- 'mid': 		材料题id
	- 'stem': 		材料题子题id集合
	- 'objNumber': 	客观题题数量
	- 'subNumber': 	主观题数量
	- 'objScore': 	客观题分数
	- 'subScore': 	主观题分数
	- 'word': 		word的md5
	- 'parentPid': 	试卷父id
	- 'copyCode': 	拷贝码
	- 'isVideo': 	是否包含视频题：0-不包含；1-包含；
	- 'isFinish':  	编辑状态：0-编辑中；1-已敲定；
	- 'dataStatus' 	试卷状态：（-1-删除；1-正常；2-隐藏；）
	- 'createTime': 初始化时间
	- 'updateTime': 最近更新时间
                
```
> **返回结果示例：**

``` json
{
    "status": 0,
    "data" :{
        "pid":1,
		"createTid":1,
		"ownerTid":1,
        "teacherName":"李思思",
        "paperName":"英语练习",
		"paperStructJson":[
			{
				"qid":1
			},
			{
				"mid":1,
				"stem":[1,2,3,4]
			}
		],
		"objNumber":20,
		"subNumber":0,
		"objScore":0,
		"subScore":0,
		"word":"3039c459a93202892c6a939abbfa68216e41463d",
		"parentPid":2,
		"copyCode":"3039c459a932",
		"isVideo":1,
		"isFinish":1,
		"dataStatus":1,
		"createTime":"2016-12-29 12:20:12",
		"updateTime":"2016-12-29 12:20:12"
	}         
}
```
#### 4.6 新增新试卷接口
> 用于录题新增试卷
> 需要使用Token验证查询者身份

``` json
Url:        /mstudy/test/paper?token=[token]
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'token'：       token必填项
    - 'paperName'：   试卷名字
 
示例：
{
	"paperName":"试卷名称"
}

Response:
    - 'status'：0->is OK
               11->token is invalid
               12->token is timeout
               13->Illegal interface calls, not power use interface
```

> **返回结果示例：**

``` json
{
    "status":0,
    "data":{
        "pid":1
    }
}
```


####4.7 修改指定试卷接口

> 需要使用Token验证查询者身份

``` json
Url:        /mstudy/test/paper?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'token'：      	token--必填项
    - 'pid'：      		试卷id
    - 'paperName'：     试卷名字
    - 'paperStructJson'：   试卷结构
    - 'qid'：      		试题id
    - 'mid'：      		材料id
    - 'stem'：      	材料子题id集合
    - 'word'：      	word地址
示例：
{
	"pid":1,
	"paperName":"英语练习",
	"paperStructJson":[
		{
			"qid":1
		},
		{
			"mid":1,
			"stem":[1,2,3,4]
		}
	],
	"word":"xxxx" 
}
Response:
    - 'status'：0->OK
                7->试卷id不存在
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

####4.8 敲定试卷接口
> 用于录题敲定试卷 (此接口较为复杂,需要将Redis里的试题保存到MySql,注意试题属性MD5在MySql中不重复,同时要生成试题版本）
> 试题id在敲定之前引用的是redis中的id，敲定之后需要将redis的试题存入mysql中，并创建相应的mysql-redis关联，并且根据hash值维护试题版本；
> 需要使用Token验证查询者身份
> 敲定时需要接口检测试卷中所包含试题的完整性；
``` json
Url:        /mstudy/test/paper/confirm?token=[token]
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'pid':  	试卷id 	必填项
    - 'token'：	token 	必填项
 
示例：
{
	"pid":[1,2,3]
}

Response:
    - 'status'：0->is OK
                2->pid不存在
               11->token is invalid
               12->token is timeout
               13->Illegal interface calls, not power use interface
               16->pid不存在  
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

####4.9 将试卷转为编辑接口
> 试题id在转为编辑之前引用的是mysql中的试题及材料id，转为编辑之后需要根据mysql的id找到redis中对应的试题及材料并生成新的redis-id,最后组成新的试卷结构并更新至编辑中的试卷结构；
> 需要使用Token验证查询者身份
> 试卷转为编辑的时候需要检验该试卷是否已被应用，如果已被应用，则需要将该试卷的状态设置为2（dataStatus:-1删除；1：正常；-2：隐藏）,并且新增一个试卷pid,并且将新的试卷isFinsh设置为0；如果该试卷未被应用，则只用将该试卷的isFinsh字段设置为0即可；
``` json
Url:        /mstudy/test/paper/edit?token=[token]
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'pid': 	试卷id  必填项
    - 'token'：	token 必填项
 
示例：
{
	"pid":[1,2,3]
}

Response:
    - 'status'：0->is OK
                2->pid不存在
               11->token is invalid
               12->token is timeout
               13->Illegal interface calls, not power use interface
               16->pid不存在  
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
#### 4.10  试卷预览接口
> 试卷预览接口在录题、获取试卷、批改相关功能用到 (查询除试卷包括试题的所有信息)
> 需要使用Token验证查询者身份

``` json
Url:        /mstudy/test/paper/preview?pid=[pid]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'token'：         token--必填项
    - 'pid': 			试卷id  必填项
Response:
    - 'status'： 0->OK
                1-pid不存在   
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
	- 'pid' : 		试卷id
	- 'createTid': 	创建教师id
	- 'ownerTid': 	试卷所属教师id
    - 'teacherName':试卷所属教师姓名
	- 'paperName': 	试卷名称
	- 'paperStructJson': 试卷结构
	- 'qid': 		试题id
	- 'mid': 		材料题id
	- 'stem': 		材料题子题id集合
	- 'objNumber': 	客观题题数量
	- 'subNumber': 	主观题数量
	- 'objScore': 	客观题分数
	- 'subScore': 	主观题分数
	- 'word': 		word的md5
	- 'parentPid': 	试卷父id
	- 'copyCode': 	拷贝码
	- 'isVideo': 	是否包含视频题：0-不包含；1-包含；
	- 'isfinish':  	编辑状态：0-编辑中；1-已敲定；
	- 'dataStatus' 	试卷状态：（-1-删除；1-正常；2-隐藏；）
	- 'createTime': 初始化时间
	- 'updateTime': 最近更新时间
    - 'qContent'：  试题内容
    - 'choices'：   试题选项：分为客观题（字符串依次对应的选项是1,2,3,...）/主观题（为[[1答题类型，2答题类型],[1分值，2分值]]）
    - 'qType'：     试题类型:1-客观题；2-主观题；  --必填
    - 'qAnswer'：   试题答案：分为客观题（正确答案从1开始依次递增：[正确答案1，正确答案2]）/主观题（分别是["第一个答案","第二个答案","第三个答案"]）
    - 'qResolve'：  试题解析：结构为{"标签1":"解析内容","标签2":"解析内容"}
    - 'mDigest'：   试题唯一hash值
    - 'mContent'：  材料试题内容
```
> **返回结果示例：**

``` json
{
    "status":0,
    "data":{
		"paper":{
			"pid":1,
			"createTid":1,
			"ownerTid":1,
            "teacherName":"张三",
			"paperName":"英语练习",
			"paperStructJson":[
				{
					"qid":1
				},
				{
					"mid":1,
					"stem":[2,3,4,5]
				},
                {
                    "mid":2,
                    "stem":[6,7]
                }
			],
			"objNumber":20,
			"subNumber":0,
			"objScore":0,
			"subScore":0,
			"word":"3039c459a93202892c6a939abbfa68216e41463d",
			"parentPid":2,
			"copyCode":"3039c459a932",
			"isVideo":1,
			"isFinish":1,
			"dataStatus":1,
			"createTime":"2016-12-29 12:20:12",
			"updateTime":"2016-12-29 12:20:12"
		},
		"questions":[
			{
				"qid":1,
				"createTid":2,
				"qDigest":"sdsfdadfasdfsdfsadfsdfsdfsdfsdf",
				"qType":1,
				"qContent":"这里是内容",
				"choices":["111","222","22523",""],
				"qAnswer":[1],
				"qScore":0,
				"qResolve":{"xkxkd":"skdfkasjd","xkxkd":"skdfkasjd"},
				"createTime":"2016-12-28 12:12:20",
				"updateTime":"2016-12-28 12:12:20"
			},
			{
				"mid":1,
				"createTid":2,
				"mDigest":"sdsfdadfasdfsdfsadfsdfsdfsdfsdf",
				"mContent":"这里是内容",
				"createTime":"2016-12-28 12:12:20",
				"updateTime":"2016-12-28 12:12:20"
			}
		],
		"stem":{
            "1":[                    #代表mid值
    			{
    				"qid":2,
    				"createTid":2,
    				"qDigest":"sdsfdadfasdfsdfsadfsdfsdfsdfsdf",
    				"mid":2,
    				"qType":1,
    				"qContent":"这里是内容",
    				"choices":["111","222","22523",""],
    				"qAnswer":[1],
    				"qScore":0,
    				"qResolve":{"xkxkd":"skdfkasjd","xkxkd":"skdfkasjd"},
    				"createTime":"2016-12-28 12:12:20",
    				"updateTime":"2016-12-28 12:12:20"
    			},
    			{
    				"qid":3,
    				"createTid":2,
    				"qDigest":"sdsfdadfasdfsdfsadfsdfsdfsdfsdf",
    				"mid":2,
    				"qType":2,
    				"qContent":"这里是内容",
    				"choices":[[1,2],[2,3]],
    				"qAnswer":["111","222","22523","00"],
    				"qScore":0,
    				"qResolve":{"xkxkd":"skdfkasjd","xkxkd":"skdfkasjd"},
    				"createTime":"2016-12-28 12:12:20",
    				"updateTime":"2016-12-28 12:12:20"
    			},
                {
                    "qid":4,
                    "createTid":2,
                    "qDigest":"sdsfdadfasdfsdfsadfsdfsdfsdfsdf",
                    "mid":2,
                    "qType":1,
                    "qContent":"这里是内容",
                    "choices":["111","222","22523",""],
                    "qAnswer":[1],
                    "qScore":0,
                    "qResolve":{"xkxkd":"skdfkasjd","xkxkd":"skdfkasjd"},
                    "createTime":"2016-12-28 12:12:20",
                    "updateTime":"2016-12-28 12:12:20"
                },
                {
                    "qid":5,
                    "createTid":2,
                    "qDigest":"sdsfdadfasdfsdfsadfsdfsdfsdfsdf",
                    "mid":2,
                    "qType":1,
                    "qContent":"这里是内容",
                    "choices":["111","222","22523",""],
                    "qAnswer":[1],
                    "qScore":0,
                    "qResolve":{"xkxkd":"skdfkasjd","xkxkd":"skdfkasjd"},
                    "createTime":"2016-12-28 12:12:20",
                    "updateTime":"2016-12-28 12:12:20"
                }
		    ],
            "2":[                 #代表mid值
                {
                    "qid":6,
                    "createTid":2,
                    "qDigest":"sdsfdadfasdfsdfsadfsdfsdfsdfsdf",
                    "mid":2,
                    "qType":1,
                    "qContent":"这里是内容",
                    "choices":["111","222","22523",""],
                    "qAnswer":[1],
                    "qScore":0,
                    "qResolve":{"xkxkd":"skdfkasjd","xkxkd":"skdfkasjd"},
                    "createTime":"2016-12-28 12:12:20",
                    "updateTime":"2016-12-28 12:12:20"
                },
                {
                    "qid":7,
                    "createTid":2,
                    "qDigest":"sdsfdadfasdfsdfsadfsdfsdfsdfsdf",
                    "mid":2,
                    "qType":2,
                    "qContent":"这里是内容",
                    "choices":[[1,2],[2,3]],
                    "qAnswer":["111","222","22523","00"],
                    "qScore":0,
                    "qResolve":{"xkxkd":"skdfkasjd","xkxkd":"skdfkasjd"},
                    "createTime":"2016-12-28 12:12:20",
                    "updateTime":"2016-12-28 12:12:20"
                }
            ]
        }
    }         
}
```
#### 4.11  试卷删除接口
> 用于教师后台试卷管理
> 需要使用Token验证查询者身份
> 如果试卷已经被应用于作业，则试卷删除会被标记为隐藏状态；
``` json
Url:        /mstudy/test/paper?token=[token]
Method:     DELETE
Header:     Content-type:application/json
Parameter:
   - 'pid': 试卷id ---必填项
示例：
{
    "pid":[1,2]
}
Response:
    - 'status'：	0->is Ok
				   1->pid不存在
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

####4.12 试卷复制接口
>用于教师后台试卷管理
> 需要使用Token验证查询者身份

``` json
Url:        /mstudy/test/paper/copy?token=[token]
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'token'：      token       --必填项
    - 'copyCode':    试卷拷贝码  --必填项
    - 'code':        图片验证码  --必填项
示例：
{
	"copyCode":"141e3e951861bf7",
	"code":"1417",
    "codeKey":"adfasd1w417"
}
Response:
    - 'status'：0->OK
               1-> 拷贝码不存在
               2-> 图片验证码错误
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

####4.13 查询教师的试卷列表接口
> 需要使用Token验证查询者身份
``` json
Url:        /mstudy/test/paper/teacher?page=[page]&size=[size]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'token'：      token--必填项
    - 'page':        分页信息，不传则默认第一页
    - 'size':        每页显示数据量，不传默认12条
Response:
    - 'status'：0->OK
                1->未搜索到包含该名字的试卷
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
	- 'pid' : 		试卷id
	- 'createTid': 	创建教师id
	- 'ownerTid': 	试卷所属教师id
	- 'paperName': 	试卷名称
	- 'paperStructJson': 试卷结构
	- 'qid': 		试题id
	- 'mid': 		材料题id
	- 'stem': 		材料题子题id集合
	- 'objNumber': 	客观题题数量
	- 'subNumber': 	主观题数量
	- 'objScore': 	客观题分数
	- 'subScore': 	主观题分数
	- 'word': 		word的md5
	- 'parentPid': 	试卷父id
	- 'copyCode': 	拷贝码
	- 'isVideo': 	是否包含视频题：0-不包含；1-包含；
	- 'isfinish':  	编辑状态：0-编辑中；1-已敲定；
	- 'dataStatus' 	试卷状态：（-1-删除；1-正常；2-隐藏；）
	- 'createTime': 初始化时间
	- 'updateTime': 最近更新时间
```
> **返回结果示例：**

``` json
{
    "status": 0,
    "data": {
        "page":1,
		"size":12,
		"count":1,
		"paperList":[
			{
				"pid":1,
				"createTid":1,
				"ownerTid":1,
				"paperName":"英语练习",
				"paperStructJson":[
					{
						"qid":1
					},
					{
						"mid":1,
						"stem":[1,2,3,4]
					}
				],
				"objNumber":20,
				"subNumber":0,
				"objScore":0,
				"subScore":0,
				"word":"3039c459a93202892c6a939abbfa68216e41463d",
				"parentPid":2,
				"copyCode":"3039c459a932",
				"isVideo":1,
				"isFinish":1,
				"dataStatus":1,
				"createTime":"2016-12-29 12:20:12",
				"updateTime":"2016-12-29 12:20:12"
			},
			{
				"pid":1,
				"createTid":1,
				"ownerTid":1,
				"paperName":"英语练习",
				"paperStructJson":[
					{
						"qid":1
					},
					{
						"mid":1,
						"stem":[1,2,3,4]
					}
				],
				"objNumber":20,
				"subNumber":0,
				"objScore":0,
				"subScore":0,
				"word":"3039c459a93202892c6a939abbfa68216e41463d",
				"parentPid":2,
				"copyCode":"3039c459a932",
				"isVideo":1,
				"isFinish":1,
				"dataStatus":1,
				"createTime":"2016-12-29 12:20:12",
				"updateTime":"2016-12-29 12:20:12"
			}
		]
    }
}
```

####4.14 根据试卷名称搜索教师试卷接口
> 用于教师后台试卷管理
> 需要使用Token验证查询者身份

``` json
Url:        /mstudy/test/paper/search?token=[token]
Method:     PUT
Header:     Content-type:application/json
Parameter:
    - 'token'：      token--必填项
    - 'paperName':   试卷名称
    - 'page':        分页信息，不传则默认第一页
    - 'size':        每页显示数据量，不传默认12条
示例：
{
	"paperName":"数学作业",
	"page":1,
	"size":12
}
Response:
    - 'status'：0->OK
                1->未搜索到包含该名字的试卷
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
	- 'pid' : 		试卷id
	- 'createTid': 	创建教师id
	- 'ownerTid': 	试卷所属教师id
	- 'paperName': 	试卷名称
	- 'paperStructJson': 试卷结构
	- 'qid': 		试题id
	- 'mid': 		材料题id
	- 'stem': 		材料题子题id集合
	- 'objNumber': 	客观题题数量
	- 'subNumber': 	主观题数量
	- 'objScore': 	客观题分数
	- 'subScore': 	主观题分数
	- 'word': 		word的md5
	- 'parentPid': 	试卷父id
	- 'copyCode': 	拷贝码
	- 'isVideo': 	是否包含视频题：0-不包含；1-包含；
	- 'isfinish':  	编辑状态：0-编辑中；1-已敲定；
	- 'dataStatus' 	试卷状态：（-1-删除；1-正常；2-隐藏；）
	- 'createTime': 初始化时间
	- 'updateTime': 最近更新时间
```
> **返回结果示例：**

``` json
{
    "status": 0,
    "data": {
        "page":1,
		"size":12,
		"count":1,
		"paperList":[
			{
				"pid":1,
				"createTid":1,
				"ownerTid":1,
				"paperName":"英语练习",
				"paperStructJson":[
					{
						"qid":1
					},
					{
						"mid":1,
						"stem":[1,2,3,4]
					}
				],
				"objNumber":20,
				"subNumber":0,
				"objScore":0,
				"subScore":0,
				"word":"3039c459a93202892c6a939abbfa68216e41463d",
				"parentPid":2,
				"copyCode":"3039c459a932",
				"isVideo":1,
				"isFinish":1,
				"dataStatus":1,
				"createTime":"2016-12-29 12:20:12",
				"updateTime":"2016-12-29 12:20:12"
			},
			{
				"pid":1,
				"createTid":1,
				"ownerTid":1,
				"paperName":"英语练习",
				"paperStructJson":[
					{
						"qid":1
					},
					{
						"mid":1,
						"stem":[1,2,3,4]
					}
				],
				"objNumber":20,
				"subNumber":0,
				"objScore":0,
				"subScore":0,
				"word":"3039c459a93202892c6a939abbfa68216e41463d",
				"parentPid":2,
				"copyCode":"3039c459a932",
				"isVideo":1,
				"isFinish":1,
				"dataStatus":1,
				"createTime":"2016-12-29 12:20:12",
				"updateTime":"2016-12-29 12:20:12"
			}
		]
    }
}
```

#### 4.15 查询教师试题版本列表接口
> 用于教师后台试题版本获取
> 需要使用Token验证查询者身份
> 查询自己的试题版本
> 过程为：试卷编辑过程中会查询版本，传入qid参数为redis中临时qid，通过临时表中的对应表可以获取到在mysql中对应的源试题qid，接口需要在redis中获取对应mysql的qid，然后根据该qid在mysql版本表中获取试题版本列表，获取的试题版本列表为mysql中的qid

``` json
Url:        /mstudy/teacher/test/question/version?qid=[qid]&pid=[pid]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'token'：       	token	  --必填项
    - 'qid': 			试题id    --必填项
    - 'pid': 			试卷id    --必填项
   
Response:
    - 'status'：0->OK
                11->qid不存在  
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
	- 'count': 			试题数量 
	- 'sourceQid': 		源试题id 
	- 'versions': 		试题版本列表
	- 'id': 	        试题材料版本表id  
	- 'isActive': 		是否为当前启用试题：0-否；1-是； 
	- 'qid': 			试题id 
	- 'createTime': 	版本创建时间 
```
> **返回结果示例：**

``` json
{
    "status": 0,
    "data":{
		"count":12,
		"sourceQid":12,
		"versions":[
			{
				"id":1,
				"isActive":1,
				"qid":1,
				"createTime":"2016-12-30 12:00:01"
			},
			{
				"id":1,
				"isActive":0,
				"qid":1,
				"createTime":"2016-12-30 12:00:01"
			}
		]
    }         
}
```

#### 4.16 查询教师材料题版本列表接口
> 用于教师后台试题版本获取
> 需要使用Token验证查询者身份
> 查询自己的试题版本
> 过程为：试卷编辑过程中会查询版本，传入mid参数为redis中临时mid，通过临时表中的对应表可以获取到在mysql中对应的源试题mid，接口需要在redis中获取对应mysql的mid，然后根据该mid在mysql版本表中获取试题版本列表，获取的试题版本列表为mysql中的mid

``` json
Url:        /mstudy/teacher/test/material/version?mid=[mid]&pid=[pid]&token=[token]
Method:     GET
Header:     Content-type:application/json
Parameter:
    - 'token'：       	token	  --必填项
    - 'mid': 			材料题id  --必填项
    - 'pid': 			试卷id    --必填项
   
Response:
    - 'status'：0->OK
                11->qid不存在  
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
	- 'count': 			试题数量 
	- 'sourceMid': 		源材料题id 
	- 'versions': 		试题版本列表 
	- 'id': 	        试题材料版本表id 
	- 'isActive': 		是否为当前启用试题：0-否；1-是； 
	- 'mid': 			材料题id 
	- 'createTime': 	版本创建时间 
```
> **返回结果示例：**

``` json
{
    "status": 0,
    "data":{
		"count":12,
		"sourceMid":12,
		"versions":[
			{
				"id":1,
				"isActive":1,
				"mid":1,
				"createTime":"2016-12-30 12:00:01"
			},
			{
				"id":1,
				"isActive":0,
				"mid":1,
				"createTime":"2016-12-30 12:00:01"
			}
		]
    }         
}
```
#### 4.17 删除教师试题或材料题指定版本接口
> 需要使用Token验证查询者身份
``` json
Url:        /mstudy/teacher/test/qm/version?token=[token]
Method:     DELETE
Header:     Content-type:application/json
Parameter:
    - 'token'：       	token	  --必填项
    - 'id': 			试题材料版本表id    --必填项

实例：
{
	"id":[1,2]
}	 
Response:
    - 'status'：0->OK
                11->id不存在  
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

#### 4.18 启用教师试题或材料题指定版本接口
> 需要使用Token验证查询者身份
> 启用某一个版本的过程为：前端传入参数为mysql中的id(mid/qid)和将要被替换的id(redis中的mid/qid),接口要在敲定区中找到相应的redis中的id并且获取试题json，最后在redis临时区创建临时的id并关联这个试卷；
> 该接口可以启用材料或者试题新版本
``` json
Url:        /mstudy/teacher/test/qm/version/use?token=[token]
Method:     POST
Header:     Content-type:application/json
Parameter:
    - 'token'：       	token	            --必填项
    - 'pid': 			试卷id              --必填项
    - 'qid': 			要启用的试题id      --必填项(qid\mid二选一)
    - 'mid': 			要启用的材料id      --必填项(qid\mid二选一)
    - 'pastQid':        被替换的试题id      --必填项(pastQid\pastMid二选一)
    - 'pastMid':        被替换的材料id      --必填项(pastQid\pastMid二选一)

实例：
{
	"pid":1,
	"qid":1,
	"mid":2,
    "pastQid":3,
    "pastMid":4
}	 
Response:
    - 'status'：0->OK
                1->qid不存在  
                2->mid不存在  
                3->pid不存在 
                4->pastQid不存在  
                5->pastMid不存在  
                11->token is invalid
                12->token is timeout
                13->Illegal interface calls, not power use interface
    - 'qid'：       试题id
    - 'qContent'：  试题内容
    - 'mid'：       材料id
    - 'mContent'：  材料内容
    - 'choices'：   试题选项：分为客观题（字符串依次对应的选项是1,2,3,...）/主观题（为[[1答题类型，2答题类型],[1分值，2分值]]）
    - 'qType'：     试题类型:1-客观题；2-主观题；  --必填
    - 'qAnswer'：   试题答案：分为客观题（正确答案从1开始依次递增：[正确答案1，正确答案2]）/主观题（分别是["第一个答案","第二个答案","第三个答案"]）
    - 'qResolve'：  试题解析：结构为{"标签1":"解析内容","标签2":"解析内容"}
    - 'createTime'：创建时间
    - 'updateTime'：更新时间
```
> **返回结果示例：**

``` json

>当qType=1（客观题）时返回结果：
{
    "status":0,
    "data":{
        "qid":1,
        "mid":2,
        "qContent":"这里是内容",
        "choices":["111","222","22523",""],
        "qType":1,
        "qAnswer":[1],
        "qScore":0,
        "qResolve":{"xkxkd":"skdfkasjd","xkxkd":"skdfkasjd"},
        "createTime":"2016-12-28 12:12:20",
        "updateTime":"2016-12-28 12:12:20"
    }
}

>当qType=2（主观题）时返回结果：
{
    "status":0,
    "data":{
        "qid":1,
        "mid":2,
        "qContent":"这里是内容",
        "choices":[[1,2],[2,3]],
        "qType":2,
        "qAnswer":["111","222","22523",""],
        "qScore":0,
        "qResolve":{"xkxkd":"skdfkasjd","xkxkd":"skdfkasjd"},
        "createTime":"2016-12-28 12:12:20",
        "updateTime":"2016-12-28 12:12:20"
    }
}

>当为材料题的时候返回结果：
{
    "status":0,
    "data":{
        "mid":2,
        "mContent":"这里是内容",
        "createTime":"2016-12-28 12:12:20",
        "updateTime":"2016-12-28 12:12:20"
    }
}


```







































