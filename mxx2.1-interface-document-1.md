[toc]

## 麦学习2.1版本接口

### 5 班级作业相关接口


#### 5.1 教师布置一份作业接口

> 用于教师后台作业管理发布作业

``` json
Url:         /mstudy/work?token=[token]
Method:      POST
Header:      Content-type:application/json
Parameter:
    - 'pid':           试卷id
    - 'workName':      作业名字
    - 'displayTime':   作业通知显示时间
    - 'startTime':     作业开始时间
    - 'endTime':       作业结束时间
    - 'showAnswerType':   答案解析查看方式:1-交卷后立即查看；2-作业结束后查看
    - 'durationTime':     作业时长
    - 'isMixed':      客观题是否乱序:0-否；1-是；
    - 'isWholeWork' :  是否全班作业:0-否；1-是；
    - 'cids':           作业布置的班级相关信息
    - 'students':         班级学生的ID数组
    - 'ismake ':     是否布置：0-未布置 ; 1-布置；
实例：
{
    "pid": 1,
    "workName":"作业名字",
    "displaytime":"2016-12-22 13:40:32",
    "startTime": "2016-12-22 13:40:32",
    "endTime": "2016-12-22 13:40:32",
    "showAnswerType":1,
    "durationTime":7200,
    "isMixed":1,
    "isWholeWork":1,
    "cids":[
        {
            "cid":1,
            "students":[
                {
                    "studentId":1,
                    "isMake":1
                },
                {
                    "studentId":2,
                    "isMake":1
                }
            ]
        },
        {
            "cid":2,
            "students":[
                {
                    "studentId":3,
                    "isMake":1
                },
                {
                    "studentId":4,
                    "isMake":1
                }
            ]
        }
    ]    
}
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误

```
> **返回结果示例：**
``` json
{    
    "status":0,
    "data":{
        "info":"OK"
    }
}
```

####5.2  查询班级作业列表接口
> 可用于学生端助教身份
> 可用于教师获取班级作业
> 获取教师自己所在班级的作业列表，需要判断班级中（除下自己布置的）其他作业的布置教师的isShow字段是否为允许，如不允许则不获取该作业
``` json
Url:         /mstudy/work/class/list?cid=[cid]&page=[page]&size=[size]&token=[token]
Method:      GET
Header:      Content-type:application/json
Parameter:
    - 'cid':        班级id
    - 'page':       分页参数，不填则为第一页；
    - 'size':       每页显示数据量，不填则为每页12条；
    - 'token':      token；
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
    - 'count':      返回结果数量
    - 'workList':   作业的详情json数组
    - 'workName' :  作业名
    - 'workId' :    作业id
    - 'tid' :       教师id
    - 'pid' :       试卷id
    - 'displayTime': 显示时间
    - 'startTime':  开始时间
    - 'endTime':    结束时间
    - 'showAnswerType': 答案解析查看方式:1-交卷后立即查看；2-作业结束后查看
    - 'durationTime': 作业时长 
    - 'isMixed':    客观题是否乱序:0-否；1-是；
    - 'isWholeWork': 是否全班作业:0-否；1-是；
    - 'dataStatus':  是否删除:-1-删除；1-正常；
    - 'createTime': 初始化时间
    - 'updateTime': 最近更新时间
    - 'workStatus': 班级作业状态：（0-未开始；1-准备中；2-进行中；3-批改中；4-长期作业中；5-客观题自动批改；6-结束；7-已撤销） 
    - 'checkStartTime': 老师批改开始时间
    - 'checkEndTime': 老师批改结束时间
    - 'submitNumber': 作业提交人数
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "count":2,
        "page":2,
        "size":12,
        "workList": [
            {
                "workId":659,
                "workName":"当。。。。。",
                "tid":187,
                "cid":177,
                "pid":511,
                "displayTime":"2016-12-22 13:40:32",
                "startTime":"2016-12-22 13:40:32",
                "endTime":"2016-12-22 13:40:32",
                "showAnswerType":1,
                "durationTime":0,
                "isMixed":2,
                "isWholeWork":1,
                "workStatus":4,
                "createTime":"2016-12-22 13:40:32",
                "updateTime":"2016-12-22 13:40:32",
                "checkStartTime": "2016-12-22 13:40:32",
                "checkEndTime": "2016-12-22 13:40:32",
                "submitNumber":0
            },
            {
                "workId":659,
                "workName":"当。。。。。",
                "tid":187,
                "cid":177,
                "pid":511,
                "displayTime":"2016-12-22 13:40:32",
                "startTime":"2016-12-22 13:40:32",
                "endTime":"2016-12-22 13:40:32",
                "showAnswerType":1,
                "durationTime":0,
                "isMixed":2,
                "isWholeWork":1,
                "workStatus":4,
                "createTime":"2016-12-22 13:40:32",
                "updateTime":"2016-12-22 13:40:32",
                "checkStartTime": "2016-12-22 13:40:32",
                "checkEndTime": "2016-12-22 13:40:32",
                "submitNumber":0
            }
        ]
    }
}
```

#### 5.3 查询教师作业列表接口
>需要token验证
>用于教师后台作业管理

``` json
Url:         /mstudy/work/teacher/list?tid=[tid]&page=[page]&size=[size]&token=[token]
Method:      GET
Header:      Content-type:application/json
Parameter:
    - 'tid':        教师id
    - 'page':       分页参数，不填则为第一页；
    - 'size':       每页显示数据量，不填则为每页12条；
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
    - 'count':      返回结果数量
    - 'workList':   作业的详情json数组
    - 'workName' :  作业名
    - 'workId' :    作业id
    - 'tid' :       教师id
    - 'cid ':        班级id
    - 'className':       班级名
    - 'pid' :       试卷id
    - 'displayTime': 显示时间
    - 'startTime':  开始时间
    - 'endTime':    结束时间
    - 'showAnswerType': 答案解析查看方式:1-交卷后立即查看；2-作业结束后查看
    - 'durationTime': 作业时长 
    - 'isMixed':    客观题是否乱序:0-否；1-是；
    - 'isWholeWork': 是否全班作业:0-否；1-是；
    - 'dataStatus':  是否删除:-1-删除；1-正常；
    - 'createTime': 初始化时间
    - 'updateTime': 最近更新时间
    - 'workStatus': 班级作业状态：（0-未开始；1-准备中；2-进行中；3-批改中；4-长期作业中；5-客观题自动批改；6-结束；7-已撤销） 
    - 'checkStartTime': 老师批改开始时间
    - 'checkEndTime': 老师批改结束时间
    - 'submitNumber': 作业提交人数
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "count":2,
        "page":2,
        "size":12,
        "workList": [
            {
                "workId":659,
                "workName":"当。。。。。",
                "tid":187,
                "cid":177,
                "className":"班级名字",
                "pid":511,
                "displayTime":"2016-12-22 13:40:32",
                "startTime":"2016-12-22 13:40:32",
                "endTime":"2016-12-22 13:40:32",
                "showAnswerType":1,
                "durationTime":0,
                "isMixed":2,
                "isWholeWork":1,
                "workStatus":4,
                "createTime":"2016-12-22 13:40:32",
                "updateTime":"2016-12-22 13:40:32",
                "checkStartTime": "2016-12-22 13:40:32",
                "checkEndTime": "2016-12-22 13:40:32",
                "submitNumber":0
            },
            {
                "workId":659,
                "workName":"当。。。。。",
                "tid":187,
                "cid":177,
                "className":"班级名字",
                "pid":511,
                "displayTime":"2016-12-22 13:40:32",
                "startTime":"2016-12-22 13:40:32",
                "endTime":"2016-12-22 13:40:32",
                "showAnswerType":1,
                "durationTime":0,
                "isMixed":2,
                "isWholeWork":1,
                "workStatus":4,
                "createTime":"2016-12-22 13:40:32",
                "updateTime":"2016-12-22 13:40:32",
                "checkStartTime": "2016-12-22 13:40:32",
                "checkEndTime": "2016-12-22 13:40:32",
                "submitNumber":0
            }
        ]
    }
 }
```

#### 5.4 根据作业名搜索教师作业列表接口
>需要验证token
>用于教师后台作业管理

``` json
Url:         /mstudy/work/teacher/name?page=[page]&size=[size]&token=[token]
Method:      PUT
Header:      Content-type:application/json
Parameter:
    - 'cid':        班级id
    - 'workName':   作业名
    - 'page':       分页参数，不填则为第一页；
    - 'size':       每页显示数据量，不填则为每页12条；
示例
{
    "cid":1,
    "workName":"考试"
}   
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
    - 'count':      返回结果数量
    - 'workList':   作业的详情json数组
    - 'workName' :  作业名
    - 'workId' :    作业id
    - 'tid' :       教师id
    - 'cid ':        班级id
    - 'className':       班级名
    - 'pid' :       试卷id
    - 'displayTime': 显示时间
    - 'startTime':  开始时间
    - 'endTime':    结束时间
    - 'showAnswerType': 答案解析查看方式:1-交卷后立即查看；2-作业结束后查看
    - 'durationTime': 作业时长 
    - 'isMixed':    客观题是否乱序:0-否；1-是；
    - 'isWholeWork': 是否全班作业:0-否；1-是；
    - 'dataStatus':  是否删除:-1-删除；1-正常；
    - 'createTime': 初始化时间
    - 'updateTime': 最近更新时间
    - 'workStatus': 班级作业状态：（0-未开始；1-准备中；2-进行中；3-批改中；4-长期作业中；5-客观题自动批改；6-结束；7-已撤销） 
    - 'checkStartTime': 老师批改开始时间
    - 'checkEndTime': 老师批改结束时间
    - 'submitNumber': 作业提交人数
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "count":2,
        "page":2,
        "size":12,
        "workList": [
            {
                "workId":659,
                "workName":"当。。。。。",
                "tid":187,
                "cid":177,
                "className":"班级名字",
                "pid":511,
                "displayTime":"2016-12-22 13:40:32",
                "startTime":"2016-12-22 13:40:32",
                "endTime":"2016-12-22 13:40:32",
                "showAnswerType":1,
                "durationTime":0,
                "isMixed":1,
                "isWholeWork":1,
                "workStatus":4,
                "createTime":"2016-12-22 13:40:32",
                "updateTime":"2016-12-22 13:40:32",
                "checkStartTime": "2016-12-22 13:40:32",
                "checkEndTime": "2016-12-22 13:40:32",
                "submitNumber":0
            },
            {
                "workId":659,
                "workName":"当。。。。。",
                "tid":187,
                "cid":177,
                "className":"班级名字",
                "pid":511,
                "displayTime":"2016-12-22 13:40:32",
                "startTime":"2016-12-22 13:40:32",
                "endTime":"2016-12-22 13:40:32",
                "showAnswerType":1,
                "durationTime":0,
                "isMixed":0,
                "isWholeWork":1,
                "workStatus":4,
                "createTime":"2016-12-22 13:40:32",
                "updateTime":"2016-12-22 13:40:32",
                "checkStartTime": "2016-12-22 13:40:32",
                "checkEndTime": "2016-12-22 13:40:32",
                "submitNumber":0
            }
        ]
    }
 }
```

#### 5.5 删除指定作业接口

>用于教师后台作业管理
>支持单，批量操作
``` json
Url:         /mstudy/work?token=[token]
Method:      DELETE
Header:      Content-type:application/json
Parameter:
    - 'workId':        作业id数组
Response:
例如
{
  "workId":[1,2,3]
}
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
                    
    - 'data':        返回的详细JSON数据
    - 'info':        返回结果
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"OK"
    }
 }
```
#### 5.6 撤销/取消撤销指定班级作业接口

>用于教师后台作业管理
>作业在任何状态下都可以被撤销，被撤销的作业不能够进行其他任何操作；
>取消撤销功能需要接口重新调用状态机计算作业状态；
>支持单，批量操作

``` json
Url:         /mstudy/work/is/revoke?token=[token]
Method:      PUT
Header:      Content-type:application/json
Parameter:
    - 'workId':        作业id数组
    - 'isRevoke':      是否撤销作业:0-否；1-是；
Response:
例如
{
  "workId":[1,2,3],
  "isRevoke":1
}
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
                    
    - 'data':        返回的详细JSON数据
    - 'info':        返回结果
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"OK"
    }
 }
```
#### 5.7 修改班级作业接口

>用于教师后台作业管理

``` json
Url:         /mstudy/work?token=[token]
Method:      PUT
Header:      Content-type:application/json
Parameter:
    - 'workId':       作业id
    - 'workName':     作业名
    - 'displayTime':  作业通知显示时间
    - 'startTime':    作业开始时间
    - 'endTime':      作业结束时间
    - 'showAnswerType': 答案解析查看方式:1-交卷后立即查看；2-作业结束后查看
    - 'durationTime': 作业时长 
    - 'isMixed':      客观题是否乱序:0-否；1-是；
    - 'isWholeWork':  是否全班作业:0-否；1-是；
    - 'ismake ':     是否布置：0-未布置 ; 1-布置；

实例：
{
    "workId":665,
    "workName":"let me see",
    "displayTime":"2016-12-22 13:40:32",
    "startTime":"2016-12-22 13:40:32",
    "endTime":"2016-12-22 13:40:32",
    "showAnswerType":1,
    "durationTime":1,
    "isMixed":2,
    "isWholeWork":1,
    "cids":[
        {
            "cid":1,
            "students":[
                {
                    "studentId":1,
                    "isMake":1
                },
                {
                    "studentId":2,
                    "isMake":1
                }
            ]
        }
    ]
}  

Response:

    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
                    
    - 'data':        返回的详细JSON数据
    - 'info':        返回结果
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "info":"OK"
    }
 }
```

#### 5.8 教师获取班级作业详情接口

>用于教师后台作业管理

``` json
Url:         /mstudy/work/class?workId=[workId]&token=[token]
Method:      GET
Header:      Content-type:application/json
Parameter:
    - 'workId':        作业id
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
                    
    - 'workId':       作业id
    - 'workName':      作业名
    - 'pid':          试卷id
    - 'paperName':    试卷名字
    - 'tid':          布置作业的教师ID
    - 'displayTime':  作业通知显示时间
    - 'startTime':    作业开始时间
    - 'endTime':      作业结束时间
    - 'showAnswerType': 答案解析查看方式:1-交卷后立即查看；2-作业结束后查看
    - 'durationTime': 作业时长 
    - 'isMixed':      客观题是否乱序:0-否；1-是；
    - 'isWholeWork':  是否全班作业:0-否；1-是；
    - 'workstatus':   作业状态（0-未开始；1-准备中；2-进行中；3-批改中；4-长期作业中；5-客观题自动批改；6-结束；7-已撤销）
    - 'className'    班级名
    - 'cid':         班级id
    - 'students':    学生集合
    - `createTime` : 初始化时间
    - `updateTime` : 最近更新时间
    - 'class':       班级信息
    - 'ismake ':     是否布置：0-未布置 ; 1-布置；
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "workId":665,
        "workName":"let me see",
        "tid":24,
        "pid":517,
        "paperName":"517",
        "displayTime":"2016-12-22 13:40:32",
        "startTime":"2016-12-22 13:40:32",
        "endTime":"2016-12-22 13:40:32",
        "showAnswerType":1,
        "durationTime":1,
        "isMixed":2,
        "isWholeWork":1,
        "workStatus":4,
        "createTime":"2016-12-22 13:40:32",
        "updateTime":"2016-12-22 13:40:32",
        "class":{
            "className":"呵呵呵",
            "cid":1,
            "students":[
                {
                    "studentId": 1,
                    "studentName":"sss",
                    "remarkName":"班级备注名",
                    "isMake":1
                },
                {
                    "studentId": 1,
                    "studentName":"sss",
                    "remarkName":"班级备注名",
                    "isMake":1
                }
            ]
        }
    }
}

```

#### 5.9 获取学生新作业列表接口
> 返回值中包含客观题是否乱序的参数，作为前端组卷是否乱序的参考；
> 用于学生端
> 需要token验证
``` json
Url:         /mstudy/work/student/new?studentId=[studentId]&page=[page]&size=[size]&token=[token]
Method:      GET
Header:      Content-type:application/json
Parameter:
    - 'studentId':  学生id
    - 'page':       分页参数，不填则为第一页；
    - 'size':       每页显示数据量，不填则为每页12条；
    - 'token':      token
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
                    
    - 'id':          学生作业id
    - 'count':       返回结果数量
    - 'workId':      作业id
    - 'pid':         试卷id
    - 'workName':    作业名
    - 'cid ':        班级id
    - 'className':   班级名
    - 'isMixed':     客观题是否乱序:0-否；1-是；
    - 'tid':         布置作业的教师ID
    - 'teacherName':  教师姓名
    - 'startTime':    作业开始时间
    - 'endTime':      作业结束时间
    - 'durationTime': 作业规定作答时长
    - 'uWorkStatus' :  用户作业状态（0-新作业；1-作业中；2-待批改；3-已完成；4-未作答；5-已撤销；）
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "count":2,
        "page":2,
        "size":12,
        "workList":[
            {
                "id":664, 
                "workId":664, 
                "pid":664, 
                "workName":"let me see", 
                "cid":166, 
                "className":"武汉一中22班", 
                "startTime":"2016-12-22 13:40:32",
                "endTime":"2016-12-22 13:40:32",
                "durationTime":1,
                "isMixed":1,
                "tid":1,
                "teacherName":"1",
                "uWorkStatus":1
            },
            {
                "id":664, 
                "workId":664, 
                "pid":664, 
                "workName":"let me see", 
                "cid":166, 
                "className":"武汉一中22班", 
                "startTime":"2016-12-22 13:40:32",
                "endTime":"2016-12-22 13:40:32",
                "durationTime":1,
                "isMixed":0,
                "tid":1,
                "teacherName":"1",
                "uWorkStatus":1
            }
        ]
    }        
}

```
#### 5.10 获取学生微课作业列表接口
> 需要token验证身份
> 用于学生后台
> 如果微课作业是已完成的作业则需要判断是否返回统计信息，依据为：如果班级作业属性为长期作业，答案解析查看方式为交卷立即查看，则交卷就可以查看报告统计报告，否则需要转换为有限期作业；如果是有限期作业，需要根据答案解析查看方式来确定是交卷后立即查看报告，还是需要班级作业结束后才能查看作业统计报告；
``` json
Url:         /mstudy/work/student/video?studentId=[studentId]&page=[page]&size=[size]&token=[token]
Method:      GET
Header:      Content-type:application/json
Parameter:
    - 'studentId':  学生id
    - 'page':       分页参数，不填则为第一页；
    - 'size':       每页显示数据量，不填则为每页12条；
    - 'token':      token
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
    - 'id':          学生作业id
    - 'count':       返回结果数量
    - 'workId':      作业id
    - 'pid':         试卷id
    - 'workName':    作业名
    - 'cid ':        班级id
    - 'className':   班级名
    - 'tid':         布置作业的教师ID
    - 'teacherName': 教师姓名
    - 'startTime':   作业开始时间
    - 'endTime':     作业结束时间
    - 'durationTime': 作业规定答题时长
    - 'showAnswerType': 答案解析查看方式:1-交卷后立即查看；2-作业结束后查看
    - 'uWorkStatus': 用户作业状态（0-新作业；1-作业中；2-待批改；3-已完成；4-未作答；5-已撤销；）
    - 'workStatus':  班级作业状态（0-未开始；1-准备中；2-进行中；3-批改中；4-长期作业中；5-客观题自动批改；6-结束；7-已撤销）
    - 'score':       学生该场作业得分
    - 'rank':        学生该作业得分班级内排名（由高到低排）
    - 'totalNumber': 该场作业学生总人数
    - 'uDurationTime':学生该作业实际耗时
    - 'avgScore':    该场作业该班平均得分
    - 'maxScore':    该场作业该班最高得分
    - 'minScore':    该场作业该班最低得分
    - 'totalScore':  该场作业总分
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "count":2,
        "page":2,
        "size":12,
        "workList":[
            {
                "id":664, 
                "workId":664, 
                "pid":664, 
                "workName":"let me see", 
                "cid":166, 
                "className":"武汉一中22班", 
                "tid":1,
                "teacherName":"1",
                "startTime":"2016-12-22 13:40:32",
                "endTime":"2016-12-22 13:40:32",
                "durationTime":0,
                "showAnswerType":1,
                "uWorkStatus":1,
                "workStatus":1,
                "score":1,
                "rank":1,
                "totalNumber":1,
                "uDurationTime":1,
                "avgScore":1,
                "maxScore":1,
                "minScore":1,
                "totalScore":1
            },
            {
                "id":664, 
                "workId":664, 
                "pid":664, 
                "workName":"let me see", 
                "cid":166, 
                "className":"武汉一中22班", 
                "tid":1,
                "teacherName":"1",
                "startTime":"2016-12-22 13:40:32",
                "endTime":"2016-12-22 13:40:32",
                "durationTime":0,
                "showAnswerType":1,
                "uWorkStatus":1,
                "workStatus":1,
                "score":1,
                "rank":1,
                "totalNumber":1,
                "uDurationTime":1,
                "avgScore":1,
                "maxScore":1,
                "minScore":1,
                "totalScore":1
            }
        ]
    }
}
```

#### 5.11 获取学生成绩报告作业列表接口
> 需要token验证
> 用于学生后台
> 如果微课作业是已完成的作业则需要判断是否返回统计信息，依据为：如果班级作业属性为长期作业，答案解析查看方式为交卷立即查看，则交卷就可以查看报  告统计报告，否则需要转换为有限期作业；如果是有限期作业，需要根据答案解析查看方式来确定是交卷后立即查看报告，还是需要班级作业结束后才能查看作业统计报告；
``` json
Url:         /mstudy/work/student/report?studentId=[studentId]&page=[page]&size=[size]&token=[token]
Method:      GET
Header:      Content-type:application/json
Parameter:
    - 'studentId':  学生id
    - 'page':       分页参数，不填则为第一页；
    - 'size':       每页显示数据量，不填则为每页12条；
    - 'token':      获取学生uid
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
    - 'id':          学生作业id
    - 'count':       返回结果数量
    - 'workId':      作业id
    - 'pid':         试卷id
    - 'workName':    作业名
    - 'cid ':        班级id
    - 'className':   班级名
    - 'tid':         布置作业的教师ID
    - 'teacherName': 教师姓名
    - 'startTime':   作业开始时间
    - 'endTime':     作业结束时间
    - 'durationTime': 作业规定答题时长
    - 'showAnswerType': 答案解析查看方式:1-交卷后立即查看；2-作业结束后查看
    - 'uWorkStatus': 用户作业状态（0-新作业；1-作业中；2-待批改；3-已完成；4-未作答；5-已撤销；）
    - 'workStatus':  班级作业状态（0-未开始；1-准备中；2-进行中；3-批改中；4-长期作业中；5-客观题自动批改；6-结束；7-已撤销）
    - 'score':       学生该场作业得分
    - 'rank':        学生该作业得分班级内排名（由高到低排）
    - 'totalNumber': 该场作业学生总人数
    - 'uDurationTime':学生该作业实际耗时
    - 'avgScore':    该场作业该班平均得分
    - 'maxScore':    该场作业该班最高得分
    - 'minScore':    该场作业该班最低得分
    - 'totalScore':  该场作业总分
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "count":2,
        "page":2,
        "size":12,
        "workList":[
            {
                "id":664, 
                "workId":664, 
                "pid":664, 
                "workName":"let me see", 
                "cid":166, 
                "className":"武汉一中22班", 
                "tid":1,
                "teacherName":"1",
                "startTime":"2016-12-22 13:40:32",
                "endTime":"2016-12-22 13:40:32",
                "durationTime":0,
                "showAnswerType":1,
                "uWorkStatus":1,
                "workStatus":1,
                "score":1,
                "rank":1,
                "totalNumber":1,
                "uDurationTime":1,
                "avgScore":1,
                "maxScore":1,
                "minScore":1,
                "totalScore":1
            },
            {
                "id":664, 
                "workId":664, 
                "pid":664, 
                "workName":"let me see", 
                "cid":166, 
                "className":"武汉一中22班", 
                "tid":1,
                "teacherName":"1",
                "startTime":"2016-12-22 13:40:32",
                "endTime":"2016-12-22 13:40:32",
                "durationTime":0,
                "showAnswerType":1,
                "uWorkStatus":1,
                "workStatus":1,
                "score":1,
                "rank":1,
                "totalNumber":1,
                "uDurationTime":1,
                "avgScore":1,
                "maxScore":1,
                "minScore":1,
                "totalScore":1
            }
        ]
    }
}
```

#### 5.12 获取班级的用户新作业列表接口
> 需要token验证
> 用于学生后台
> 新增作答时长字段
``` json
Url:         /mstudy/work/class/student/new?cid=[cid]&studentId=[studentId]&page=[page]&size=[size]&token=[token]
Method:      GET
Header:      Content-type:application/json
Parameter:
    - 'cid':        班级id
    - 'studentId':  学生id
    - 'page':       分页参数，不填则为第一页；
    - 'size':       每页显示数据量，不填则为每页12条；
    - 'token':      token
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误                   
    - 'id':          学生作业id
    - 'count':       返回结果数量
    - 'workId':      作业id
    - 'pid':         试卷id
    - 'workName':    作业名
    - 'cid ':        班级id
    - 'className':   班级名
    - 'tid':         布置作业的教师ID
    - 'teacherName':  教师姓名
    - 'startTime':    作业开始时间
    - 'endTime':      作业结束时间
    - 'durationTime': 作业规定作答时长
    - 'uWorkStatus':  用户作业状态（0-新作业；1-作业中；2-待批改；3-已完成；4-未作答；5-已撤销；）
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "count":2,
        "page":2,
        "size":12,
        "workList":[
            {
                "id":664, 
                "workId":664, 
                "pid":664, 
                "workName":"let me see", 
                "cid":166, 
                "className":"武汉一中22班", 
                "startTime":"2016-12-22 13:40:32",
                "endTime":"2016-12-22 13:40:32",
                "durationTime":1,
                "tid":1,
                "teacherName":"1",
                "uWorkStatus":1
            },
            {
                "id":664, 
                "workId":664, 
                "workName":"let me see", 
                "cid":166, 
                "className":"武汉一中22班", 
                "startTime":"2016-12-22 13:40:32",
                "endTime":"2016-12-22 13:40:32",
                "durationTime":1,
                "tid":1,
                "teacherName":"1",
                "uWorkStatus":1
            }
        ]
    }        
}

```

#### 5.13 获取班级的用户已完成作业列表接口
> 需要token验证
> 用于学生后台
> 如果微课作业是已完成的作业则需要判断是否返回统计信息，依据为：如果班级作业属性为长期作业，答案解析查看方式为交卷立即查看，则交卷就可以查看报告统计报告，否则需要转换为有限期作业；如果是有限期作业，需要根据答案解析查看方式来确定是交卷后立即查看报告，还是需要班级作业结束后才能查看作业统计报告；
``` json
Url:         /mstudy/work/class/student/finish?cid=[cid]&studentId=[studentId]&page=[page]&size=[size]&token=[token]
Method:      GET
Header:      Content-type:application/json
Parameter:
    - 'cid':        班级id
    - 'studentId':  学生id
    - 'page':       分页参数，不填则为第一页；
    - 'size':       每页显示数据量，不填则为每页12条；
    - 'token':      token
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
    - 'id':          学生作业id
    - 'count':       返回结果数量
    - 'workId':      作业id
    - 'pid':         试卷id
    - 'workName':    作业名
    - 'cid ':        班级id
    - 'className':   班级名
    - 'tid':         布置作业的教师ID
    - 'teacherName': 教师姓名
    - 'startTime':   作业开始时间
    - 'endTime':     作业结束时间
    - 'durationTime': 作业规定答题时长
    - 'showAnswerType': 答案解析查看方式:1-交卷后立即查看；2-作业结束后查看
    - 'uWorkStatus': 用户作业状态（0-新作业；1-作业中；2-待批改；3-已完成；4-未作答；5-已撤销；）
    - 'workStatus':  班级作业状态（0-未开始；1-准备中；2-进行中；3-批改中；4-长期作业中；5-客观题自动批改；6-结束；7-已撤销）
    - 'score':       学生该场作业得分
    - 'rank':        学生该作业得分班级内排名（由高到低排）
    - 'totalNumber': 该场作业学生总人数
    - 'uDurationTime':学生该作业实际耗时
    - 'avgScore':    该场作业该班平均得分
    - 'maxScore':    该场作业该班最高得分
    - 'minScore':    该场作业该班最低得分
    - 'totalScore':  该场作业总分
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "count":2,
        "page":2,
        "size":12,
        "workList":[
            {
                "id":664, 
                "workId":664, 
                "pid":664, 
                "workName":"let me see", 
                "cid":166, 
                "className":"武汉一中22班", 
                "tid":1,
                "teacherName":"1",
                "startTime":"2016-12-22 13:40:32",
                "endTime":"2016-12-22 13:40:32",
                "durationTime":0,
                "showAnswerType":1,
                "uWorkStatus":1,
                "workStatus":1,
                "score":1,
                "rank":1,
                "totalNumber":1,
                "uDurationTime":1,
                "avgScore":1,
                "maxScore":1,
                "minScore":1,
                "totalScore":1
            },
            {
                "id":664, 
                "workId":664, 
                "pid":664, 
                "workName":"let me see", 
                "cid":166, 
                "className":"武汉一中22班", 
                "tid":1,
                "teacherName":"1",
                "startTime":"2016-12-22 13:40:32",
                "endTime":"2016-12-22 13:40:32",
                "durationTime":0,
                "showAnswerType":1,
                "uWorkStatus":1,
                "workStatus":1,
                "score":1,
                "rank":1,
                "totalNumber":1,
                "uDurationTime":1,
                "avgScore":1,
                "maxScore":1,
                "minScore":1,
                "totalScore":1
            }
        ]
    }
}
```



#### 5.14 获取班级的用户撤销作业列表接口
> 需要token验证
> 用于学生后台
``` json
Url:         /mstudy/work/class/student/revoke?cid=[cid]&studentId=[studentId]&page=[page]&size=[size]&token=[token]
Method:      GET
Header:      Content-type:application/json
Parameter:
    - 'cid':        班级id
    - 'studentId':  学生id
    - 'page':       分页参数，不填则为第一页；
    - 'size':       每页显示数据量，不填则为每页12条；
    - 'token':      token
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
    - 'id':          学生作业id
    - 'count':       返回结果数量
    - 'workId':      作业id
    - 'pid':         试卷id
    - 'workName':    作业名
    - 'cid ':        班级id
    - 'className':   班级名
    - 'tid':         布置作业的教师ID
    - 'teacherName':  教师姓名
    - 'startTime':    作业开始时间
    - 'endTime':      作业结束时间
    - 'durationTime': 作业规定作答时长
    - 'uWorkStatus':  用户作业状态（0-新作业；1-作业中；2-待批改；3-已完成；4-未作答；5-已撤销；）
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data":{
        "count":2,
        "page":2,
        "size":12,
        "workList":[
            {
                "id":664, 
                "workId":664, 
                "pid":664, 
                "workName":"let me see", 
                "cid":166, 
                "className":"武汉一中22班", 
                "startTime":"2016-12-22 13:40:32",
                "endTime":"2016-12-22 13:40:32",
                "durationTime":1,
                "tid":1,
                "teacherName":"1",
                "uWorkStatus":1
            },
            {
                "id":664, 
                "workId":664, 
                "workName":"let me see", 
                "cid":166, 
                "className":"武汉一中22班", 
                "startTime":"2016-12-22 13:40:32",
                "endTime":"2016-12-22 13:40:32",
                "durationTime":1,
                "tid":1,
                "teacherName":"1",
                "uWorkStatus":1
            }
        ]
    }        
}

```

#### 5.15 获取班级作业作答信息接口（废弃）
> 引用8.3 获取指定的作业作答情况基本信息接口
>用于学生端
>此接口应用场景需要确定

``` json
Url:         /mstudy/work/info?workId=[workId]&token=[token]
Method:      GET
Header:      Content-type:application/json
Parameter:
    - 'workId':        作业id
    - 'token':         token
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
                    
    - 'data':         返回的详细JSON数据
    - 'count':        返回结果数量
    - 'workId':       作业id
    - 'workName':     作业名
    - 'submitNumber': 提交人数
    - 'startTime':    作业开始时间
    - 'endTime':      作业结束时间
    - 'showAnswerType': 答案解析查看方式:1-交卷后立即查看；2-作业结束后查看
    - 'durationTime':   作业时长 
    - 'objNumber':    主观题数量
    - 'subNumber':    客观题数量
    - 'objScore':     主观题分数
    - 'subScore':     客观题分数
    - 'avgScore' :    平均分
    - 'maxScore':     最高分
    - 'minScore':     最低分
    
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
		"workId":665,
		"workName":"let me see",
		"durationTime":0,
		"startTime":"2016-12-22 13:40:32",
		"endTime":"2016-12-22 13:40:32",
        "showAnswerType":1,
		"submitNumber":0,
		"objNumber":1,
		"subNumber":1,
		"objScore":0.0,
		"subScore":0.0,
		"avgScore":0.0,
		"maxScore":0.0            
    }
          
 }
```
### 6 用户作业相关接口

#### 6.1 获取指定用户指定作业基础信息接口
>需要token验证
>可用于获取学生作业作答的基本信息；
``` json
Url:         /mstudy/userwork?studentId=[studentId]&workId=[workId]&isbegin=[isBegin]&token=[token]
Method:      GET
Header:      Content-type:application/json
Para meter:
    - 'studentId':  用户id
    - 'workId':     作业id
    - 'isBegin':    作业是否开始计时：0-否；1-是；
    - 'token':      获取学生uid
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
    - 'workId':     作业id
    - 'showAnswerType': 答案解析查看方式:1-交卷后立即查看；2-作业结束后查看
    - 'studentId':  学生id
    - 'isMixed':    客观题是否混合选项：0-否；1-是； 
    - 'mixed':      客观题混合选项（前端根据该参数生成具体客观题的顺序）    
    - 'durationTime': 学生的答题时长 
    - 'getTime':    取作业时间
    - 'submitTime': 交卷时间
    - 'nowTime':    当前时间
    - 'retrievetime': 重新获取试卷时间
    - 'uWorkStatus':用户作业状态（0-新作业；1-作业中；2-待批改；3-已完成；4-未作答；5-已撤销；）
    - 'workStatus':  班级作业状态（0-未开始；1-准备中；2-进行中；3-批改中；4-长期作业中；5-客观题自动批改；6-结束；7-已撤销）
    - 'createTime': 初始化时间 
    - 'updateTime': 最近更新时间
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "studentId":1,
        "workId":1,
        "isMixed":1,
        "mixed":[1,4,3,2],
        "durationTime":7200,
        "nowTime":"2016-12-27 12:12:12",
        "retrievetime":"2016-12-27 12:12:12",
        "getTime":"2016-12-22 13:40:32",
        "submitTime":"2016-12-22 13:40:32",
        "uWorkStatus":1,
        "workStatus":1,
        "showAnswerType":1,
        "createTime":"2016-12-22 13:40:32",
        "updateTime":"2016-12-22 13:40:32" 
    }       
}
```

#### 6.2 初始化指定用户指定作业记录接口
>需要token验证
>用于学生端 (开始答题)
>初始化的时候需要在取作业的时候根据作业客观题是否混排的选项生成学生自己的答题顺序；
>用于学生新作业状态时的场景，当用户选择开始答题会调用该接口开始做题，写入getTime和mixed参数；
``` json
Url:         /mstudy/userwork?token=[token]
Method:      PUT
Header:      Content-type:application/json
Parameter:
    - 'token':      token
    - 'id':         学生的作业id
    - 'mixed':      学生作业的混排选项，是试题顺序的混排内容，参照为作业的试卷试题顺序；
例如:
{
    "id":1,
    "mixed":[1,2,3,4,5,6]
}
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data":{
        "info":"OK"
    }
}
```


#### 6.3 批量初始化指定用户指定作业记录接口
>需要token验证
>用于学生端 (开始答题)
>初始化的时候需要在取作业的时候根据作业客观题是否混排的选项生成学生自己的答题顺序；
``` json
Url:         /mstudy/userwork/batch?token=[token]
Method:      PUT
Header:      Content-type:application/json
Parameter:
    - 'token':          token
    - 'mixedList':      混排选项json
    - 'id':             学生的作业id
    - 'mixed':          学生作业的混排选项，是试题顺序的混排内容，参照为作业的试卷试题顺序；
例如:
{
    "mixedList":[
        {   
            "id":1,
            "mixed":[1,2,3,4,5,6]
        },
        {   
            "id":2,
            "mixed":[1,2,3,4,5,6]
        }
    ]
}
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data":{
        "info":"OK"
    }
 }
```


#### 6.4 结束指定用户指定作业接口
>需要token验证
>用于学生端 (交卷)
``` json
Url:         /mstudy/userwork/submit?token=[token]
Method:      PUT
Header:      Content-type:application/json
Parameter:
    - 'token':         token
    - 'id':            学生作业表id
例如:
{
	"id":1
}
Response:
    - 'status':     0->OK 操作成功;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
                    4 你已提交过试卷
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

#### 6.5 查询指定用户指定作业指定试题的答题及批改记录接口
>需要token验证
>用于学生端/教师后台批改
``` json
Url:         /mstudy/answer?studentId=[studentId]&workId=[workId]&qid=[qid]&token=[token]
Method:      GET
Header:      Content-type:application/json
Parameter:
    - 'studentId':  用户id
    - 'workId':     作业id  
    - 'qid':        试题id
    - 'token':      获取学生uid
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
    - 'studentId':   用户id
    - 'studentName': 学生姓名
    - 'remarkName':  学生班级备注名
    - 'photo':       学生头像
    - 'workId':      作业id  
    - 'qid' :        试题id
    - 'qType':       试题类型：1-客观题；2-主观题；
    - 'answer':      用户答案:分为主观题["1","2"]和客观题（[1]）的类型  
    - 'submitTime':  该试题提交时间 
    - 'answerDuration':  该试题实际答题时长  
    - 'tid':         批改教师
    - 'tutorId':     批改助教id
    - 'correctType':  批改类型：0-人工批改；1-计算机辅助批改；
    - 'correcterType': 批改人教师
    - 'correcterUid': 批改人uid
    - 'correct':     批改内容：1-填空（text）；2-文本区域（textarea）；3-图片(picture)；4音频(audio)；
    - 'correctTime': 批改时间
    - 'score':       得分
    - 'qOrder':      主观题小题序号：从1开始
    - 'isRight':     客观题是否答对：0-默认<未答>；1-答对；2-答错；
    - 'correctType': 批改类型：0-人工批改；1-计算机辅助批改；
    - 'dataStatus':  状态:（-1-删除；1-正常；）
    - 'createTime':  初始化时间
    - 'updateTime':  最近更新时间
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "studentId":1,
        "studentName":"张三",
        "remarkName":"张大仙",
        "photo":"www.kkk.com/lll.jpg",
        "workId":1,
        "qid":1,
        "qType":1,
        "answer":["1","","","2"],
		"submitTime":"2016-12-27 12:00:21",
        "answerDuration":10,
        "tid":1,
        "tutorId":10,
        "correct":[                       #试题的批改内容
			{
                "qOrder":1,
				"score":1,                #试题的子题的得分
                "correctType":1,
                "correcterType":1,
                "correcterUid",
                "correct":{               #试题的子题的批改内容
                    "text":"这样来点东西",
                    "textarea":"这样来点东西",
                    "picture":"www.kkk.com/pic.jpg",
                    "audio":"www.kkk.com/pic.mp3"
                }
            },
            {
                "qOrder":2,
                "score":1,                #试题的子题的得分
                "correctType":1,
                "correcterType":1,
                "correcterUid",
                "correct":{               #试题的子题的批改内容
                    "text":"这样来点东西",
                    "textarea":"这样来点东西",
                    "picture":"www.kkk.com/pic.jpg",
                    "audio":"www.kkk.com/pic.mp3"
                }
            }
		],
        "correctTime":"2016-12-27 12:21:23",
        "score":5.0,                      #试题的得分
        "isRight":1,
        "correctType":[1,0],
        "createTime":"2016-12-22 13:40:32",
        "updateTime":"2016-12-22 13:40:32"            
    }       
 }
```

#### 6.6 指定用户指定作业指定试题的答题提交接口
>需要token验证
>单题提交 (如果是客观题,需要判断正确错误)
>支持单、多题模式

``` json
Url:         /mstudy/answer/submit?token=[token]
Method:      PUT
Header:      Content-type:application/json
Parameter:
    - 'token':      token
    - 'studentId':  学生id
    - 'workId':     作业id  
    - 'qid' :       试题id
    - 'answer'：    答案     
    - 'submitTime': 提交时间 
    - 'qType':      试题类型：1-客观题；2-主观题；
实例:
{
	"studentId":1,
	"workId":1,
	"answer":[
		{
			"qid":1,
			"answer":[0,2],
			"submitTime":"2016-12-27 12:00:21",
			"qtype":1
		},
		{
			"qid":2,
			"answer":["1","2"],
			"submitTime":"2016-12-27 12:00:21",
			"qtype":2
		}
	]
}
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
   
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data":{
        "info":"OK"
    }
 }
```


#### 6.6.1 指定用户指定作业指定试题的答题(单题)提交接口
>需要token验证
>单题提交(如果是客观题,需要判断正确错误)
``` json
Url:         /mstudy/answer/submit/single?token=[token]
Method:      PUT
Header:      Content-type:application/json
Parameter:
    - 'token':      token
    - 'studentId':  学生id
    - 'workId':     作业id  
    - 'qid' :       试题id
    - 'answer'：    答案     
    - 'qOrder':     主观题小题序号：从1开始
    - 'qType':      试题类型：1-客观题；2-主观题；
实例(客观题):
{
    "studentId":1,
    "workId":1,
    "qid":1,
    "qtype":1,
    "answer":[0,2]
}
实例(主观题):
{
    "studentId":1,
    "workId":1,
    "qid":1,
    "qtype":2,
    "qOrder":1,
    "answer":"1"
}
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
   
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data":{
        "info":"OK"
    }
 }
```


#### 6.7 查询指定用户指定作业的答题及批改记录
>需要token验证
>用于学生端/教师后台批改
>已答试题的答题详情
>考虑是否需要分页
``` json
Url:         /mstudy/answer/user/work?studentId=[studentId]&workId=[workId]&token=[token]
Method:      GET
Header:      Content-type:application/json
Parameter:
    - 'studentId':  学生id
    - 'workId':     作业id  
    - 'token':      token
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
    - 'count':       返回结果数量
    - 'studentId':   用户id
    - 'workId':      作业id  
    - 'qid' :        试题id
    - 'qType':       试题类型（1-客观题；2-主观题；）
    - 'answer':      用户答案 
    - 'submitTime':  提交时间 
    - 'answerDuration':  实际答题时长   
    - 'tid':         批改教师
    - 'tutorId':     批改助教id 
    - 'correctType':  批改类型：0-人工批改；1-计算机辅助批改；
    - 'correcterType': 批改人教师
    - 'correcterUid': 批改人uid
    - 'correct':     批改内容：1-填空（text）；2-文本区域（textarea）；3-图片(picture)；4音频(audio)；
    - 'correctTime': 批改时间
    - 'score':       得分
    - 'qOrder':      主观题小题序号：从1开始
    - 'isRight':     是否答对:0-默认<未答>；1-答对；2-答错；
    - 'correctType': 批改类型
    - 'createTime':  初始化时间
    - 'updateTime':  最近更新时间
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "count":12,
		"answerList":{
            1:{                             # 数字代表的意思为qid
                "studentId":1,
                "workId":1,
                "qid":1,
                "qType":1,
                "answer":[2],
                "submitTime":"2016-12-27 12:00:21",
                "answerDuration":10,
                "score":5.0,                     
                "isRight":1,
                "createTime":"2016-12-22 13:40:32",
                "updateTime":"2016-12-22 13:40:32"   
            },
            2:{
                "studentId":1,
                "workId":1,
                "qid":3,
                "qType":2,
                "answer":["1","","","2"],
                "submitTime":"2016-12-27 12:00:21",
                "answerDuration":10,
                "tid":10,
                "tutorId":10,
                "correct":[
                    {
                        "qOrder":1,
                        "score":1,
                        "correctType":1,
                        "correcterType":1,
                        "correcterUid",
                        "correct":{
                            "text":"这样来点东西",
                            "textarea":"这样来点东西",
                            "picture":"www.kkk.com/pic.jpg",
                            "audio":"www.kkk.com/pic.mp3"
                        }
                    },
                    {
                        "qOrder":2,
                        "score":1,
                        "correctType":1,
                        "correcterType":1,
                        "correcterUid",
                        "correct":{
                            "text":"这样来点东西",
                            "textarea":"这样来点东西",
                            "picture":"www.kkk.com/pic.jpg",
                            "audio":"www.kkk.com/pic.mp3"
                        }
                    }
                ],
                "correctTime":"2016-12-27 12:21:23",
                "score":5.0,
                "isRight":1,
                "correctType":[1,0],
                "createTime":"2016-12-22 13:40:32",
                "updateTime":"2016-12-22 13:40:32"   
            }
        }
    }       
 }
```

### 7 批改相关接口

#### 7.1 班级作业批改基础信息查询接口
>需要token验证；
>学生端/教师后台批改查询；
>用于教师批改时获取作业基本信息；
``` json
Url:         /mstudy/correct/base?workId=[workId]&token=[token]
Method:      GET
Header:      Content-type:application/json
Parameter:
    - 'token':      token
    - 'workId':     作业id
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
    - 'workName':   作业名字
    - 'cid':        班级ID
    - 'className':  班级名称
    - 'pid':        试卷id
    - 'startTime':  作业开始时间
    - 'endTime':    作业结束时间
    - 'durationTime':作业时长
    - 'allNumber':  作业布置人数
    - 'getNumber':  作业取卷人数
    - 'submitNumber': 作业交卷人数
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "workName":"作业名字",
        "cid":1,
		"className":"aaaa",
        "pid":1,
		"startTime":"2016-12-29 12:00:00",
		"endTime":"2017-1-4 12:00:00",
        "durationTime":200,
        "allNumber":2,
		"getNumber":3,
		"submitNumber":2
    }
 }
```

#### 7.1.1 班级作业学生基础信息查询接口
>需要token验证；
>学生端/教师后台批改查询；
>用于教师批改时获取该场作业的学生列表及提交情况、分数的基本信息；
``` json
Url:         /mstudy/correct/base/student?workId=[workId]&qid=[qid]&token=[token]
Method:      GET
Header:      Content-type:application/json
Parameter:
    - 'token':      token
    - 'workId':     作业id
    - 'qid':        试题id
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
    - 'count':      指定作业指定试题的学生列表
    - 'studentList': 学生列表
    - 'studentId': 学生id
    - 'studentName': 学生名字
    - 'remarkName': 学生班级备注名
    - 'photo': 学生头像
    - 'isSubmit': 是否作答：0-否；1-是
    - 'score': 学生该题得分
   
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data":{
        "count":12,
        "studentList":[
            {
                "studentId":1,
                "studentName":"张三",
                "remarkName":"张大仙",
                "photo":"www.kkk.com/lll.jpg",
                "isSubmit":1,
                "score":2
            },
            {
                "studentId":1,
                "studentName":"张三",
                "remarkName":"张大仙",
                "photo":"www.kkk.com/lll.jpg",
                "isSubmit":1,
                "score":2
            }
        ]
    }
 }
```

#### 7.2 批改媒体文件上传接口
>需要token验证
>学生端/教师后台批改媒体文件上传
>音频不需要宽高参数
>上传文件保存在resource目录下
>mini图片如果不设定宽高，默认宽高为66px*66px
>该接口图片会默认返回mini图片地址，原图地址为去掉图片名称前面的（mini_）访问即可
> 2017-2-23(luodibashi):update;
``` json
Url:         /mstudy/upload?w=[w]&h=[h]&type=[type]&voucher=[voucher]&token=[token]
Method:      POST
Header:      Content-Type:multipart/form-data
Parameter:
    - 'token':      token
    - 'voucher':    补全信息凭证
    - 'w':          图片缩略图宽度(如果宽、高参数全部都传，则按照参数生成，否则默认返回强制宽高)
    - 'h':          图片缩略图高度(如果宽、高参数全部都传，则按照参数生成，否则默认返回强制宽高)
    - 'type':       资源类型：1-图片；2-音频；
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "url":"http://maixuexi.cn/resource/mini_cade20170101001.jpg"
    }
 }
```

#### 7.3 媒体文件获取说明
>用于学生端/教师端媒体文件查看
>首次获取需要展示mini图片，点击后获取完整图片
>可直接输入图片名字获取图片信息
Url:         /mstudy/resource/fileName   

示例：
http://maixuexi.cn/resource/cade20170101001.jpg



#### 7.4 批改内容及分数提交接口（暂时不用）
> 需要token身份验证
> 处理整个试题的提交
> 批改人要么是助教，要么是教师
``` json
Url:         /mstudy/answer/correct?token=[token]
Method:      PUT
Header:      Content-Type:application/json
Parameter:
    - 'token':      token
    - 'studentId':  学生id
    - 'workId':     作业id  
    - 'tid':        批改教师id
    - 'uid':        批改助教uid
    - 'userType':   批改助教用户类型：1-教师；2-家长；3-学生；
    - 'correct':    批改内容：1-填空（text）；2-文本区域（textarea）；3-图片(picture)；4音频(audio)；
    - 'score':      试题得分
    - 'qOrder':      主观题小题序号：从1开始
    - 'correctTime': 批改时间
示例
{
	"studentId":1,
	"workId":1,
	"qid":1,
	"tid":1,
	"uid":20,
    "userType":3,
    "score":5.5,
	"correct":[
        {
            "qOrder":1,
            "score":1,
            "correct":{
                "text":"这样来点东西",
                "textarea":"这样来点东西",
                "picture":"www.kkk.com/pic.jpg",
                "audio":"www.kkk.com/pic.mp3"
            }
        },
        {
            "qOrder":2,
            "score":1,
            "correct":{
                "text":"这样来点东西",
                "textarea":"这样来点东西",
                "picture":"www.kkk.com/pic.jpg",
                "audio":"www.kkk.com/pic.mp3"
            }
        }
    ]
}
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data":{
        "info":"OK"
    }
 }
```

#### 7.5 获取指定作业指定试题指定学生被批改的详情接口（废弃）
>教师后台批改
>需要token验证
>使用6.5 查询指定用户指定作业指定试题的答题及批改记录接口
``` json
Url:         /mstudy/correct/student/question/work?workId=[workId]&qid=[qid]&studentId=[studentId]&token=[token]
Method:      GET
Header:      Content-type:application/json
Parameter:
    - 'token':      token
    - 'workId':     作业id  
    - 'qid':        试题id 
    - 'studentId':  学生id
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
    - 'count':       返回结果数量
    - 'studentId':   用户id
    - 'studentName': 学生姓名
    - 'remarkName':  学生班级备注名
    - 'photo':       学生头像
    - 'isSubmit':    是否作答：0-否；1-是；
    - 'workId':      作业id  
    - 'qid' :        试题id
    - 'qType':       试题类型（1-客观题；2-主观题；）
    - 'answer':      用户答案 
    - 'submitTime':  提交时间 
    - 'answerDuration':  实际答题时长单位s 
    - 'tid':         批改教师
    - 'tutorId':     批改助教id
    - 'correctType':  批改类型：0-人工批改；1-计算机辅助批改；
    - 'correcterType': 批改人教师
    - 'correcterUid': 批改人uid
    - 'correct':     批改内容：1-填空（text）；2-文本区域（textarea）；3-图片(picture)；4音频(audio)；
    - 'correctTime': 批改时间
    - 'score':       得分
    - 'qOrder':      主观题小题序号：从1开始
    - 'isRight':     是否答对
    - 'correctType': 批改类型
    - 'createTime':  初始化时间
    - 'updateTime':  最近更新时间
   
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data":{
        "workId": 1,
        "qid": 1,
        "qType": 1,
		"studentId":1,
        "studentName":"张三",
        "remarkName":"张大仙",
        "photo":"www.kkk.com/lll.jpg",
        "isSubmit":1,
		"answer":["1","","","2"],
		"submitTime":"2016-12-27 12:00:21",
		"answerDuration":10,
		"tid":0,
		"tutorId":10,
		"correct":[
            {
                "qOrder":1,
                "score":1,
                "correctType":1,
                "correcterType":1,
                "correcterUid",
                "correct":{
                    "text":"这样来点东西",
                    "textarea":"这样来点东西",
                    "picture":"www.kkk.com/pic.jpg",
                    "audio":"www.kkk.com/pic.mp3"
                }
            },
            {
                "qOrder":2,
                "score":1,
                "correctType":1,
                "correcterType":1,
                "correcterUid",
                "correct":{
                    "text":"这样来点东西",
                    "textarea":"这样来点东西",
                    "picture":"www.kkk.com/pic.jpg",
                    "audio":"www.kkk.com/pic.mp3"
                }
            }
        ],
		"correctTime":"2016-12-27 12:21:23",
		"score":5.0,
		"isRight":1,
		"correctType":[1,0],
		"createTime":"2016-12-22 13:40:32",
		"updateTime":"2016-12-22 13:40:32"
    }       
 }
```


#### 7.6 班级作业批改完成接口(大保存)
>教师后台批改完成
>大保存可以多次操作，每次大保存会计算一次统计报告
>需要验证token
``` json
Url:         /mstudy/work/correct/finish?token=[token]
Method:      PUT
Header:      Content-Type:application/json
Parameter:
    - 'token':      token
    - 'workId':     作业id
示例：
{
    "workId":1
}
Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data":{
        "info":"OK"
    }
 }
```

####7.7  根据批改记录查询(废弃)
>辅助批改 (之前批改接口会自动生成这些数据。此接口供测试用)
``` json
Url:         /mstudy/ischeck?workId=[workId]&qid=[qid]&token=[token]
Method:      GET
Header:      Content-type:application/json
Parameter:
    - 'token':      获取学生uid
    - 'studentId':        用户id
    - 'workId':        作业id  
    - 'qid' :        试题id
    - 'qType':      试题类型（1-客观题；2-主观题；）
    - 'answer':      用户答案
    - 'answerDuration':  实际答题时长  
    - 'tid':         批改教师
    - 'tutorId':     批改助教id
    - 'correct':     批改内容
    - 'correctTime': 批改时间
    - 'score':       得分
    - 'isRight':     是否答对
    - 'correctType': 批改类型
    - 'status': 状态 ( 1 -> 正常;2 -> 删除)
    - 'createTime':  初始化时间
    - 'updateTime': 最近更新时间

Response:
    - 'status':     0->OK 操作成功 ;
                    1 数据不存在或未知错误 ;
                    2 数据库操作不实际需求不符合错误
                    3 项目业务上的逻辑错误
                    
    - 'data':        返回的详细JSON数据
   
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data":[
        {
            "studentId": 1,
            "workId": 1,
            "qid": 1,
            "answer": "answer",
            "answerDuration": 0,
            "tid": 1,
            "correct": "{\"image\":\"http://mstudy.me/xxx.jpg\"}",
            "correctTime": "2016-12-22 13:40:32",
            "score": 5.5,
            "status": 1,
            "createTime": "2016-12-22 13:40:32",
            "updateTime": "2016-12-22 13:40:32"
        },
        {
            "studentId": 1,
            "workId": 1,
            "qid": 1,
            "answer": "answer",
            "answerDuration": 0,
            "tid": 1,
            "correct": "{\"image\":\"http://mstudy.me/xxx.jpg\"}",
            "correctTime": "2016-12-22 13:40:32",
            "score": 5.5,
            "status": 1,
            "createTime": "2016-12-22 13:40:32",
            "updateTime": "2016-12-22 13:40:32"
        }
    ]
 }
```

####7.8  主观题小题批改提交接口
>需要token验证；
>需要处理辅助批改功能；
>单空批改提交接口：处理一个主观题包含多个子题，每个子题的批改提交接口；
``` json
Url:         /mstudy/answer/correct/crosshead?token=[token]
Method:      PUT
Header:      Content-Type:application/json
Parameter:
    - 'token':      token
    - 'studentId':  学生id
    - 'workId':     作业id  
    - 'tid':        批改教师id
    - 'uid':        批改助教uid
    - 'userType':   批改助教用户类型：1-教师；2-家长；3-学生；
    - 'correct':    批改内容：1-填空（text）；2-文本区域（textarea）；3-图片(picture)；4音频(audio)；
    - 'score':      分数
    - 'qOrder':     主观题小题序号：从1开始
示例
{
    "studentId":1,
    "workId":1,
    "qid":1,
    "tid":1,
    "uid":20,
    "userType":3,
    "qOrder":1,
    "score":1,
    "correct":{
        "text":"这样来点东西",
        "textarea":"这样来点东西",
        "picture":"www.kkk.com/pic.jpg",
        "audio":"www.kkk.com/pic.mp3"
    }
}
 
Response:
    - 'status':     0->OK 操作成功 ;
                    1 参数不存在或未知错误 ;
                    2 数据库操作不实际,需求不符合错误
                    3 项目业务上的逻辑错误
``` 
> **返回结果示例：**
``` json
{
    "status": 0,
    "data":{
        "info":"OK"
    }
 }
```

### 8 统计报告相关

####8.1 获取指定的作业用户排名基本信息接口
>需要token验证
>考虑如果需要添加分页功能
``` json
Url:         /mstudy/work/rank?workId=[workId]&type=[type]&token=[token]
Method:      GET
Header:      Content-Type:application/json
Parameter:
    - 'token':      token
    - 'type':       类型：1-按分数排名（由高到低）；2-按答题时间排名（由少到多）
    - 'workId':     作业id
    
Response:
    - 'status':     0->OK 操作成功 ;
                    1 参数不存在或未知错误 ;
                    2 数据库操作不实际,需求不符合错误
                    3 项目业务上的逻辑错误
    - 'workId':     作业id
    - 'workName':   作业名
    - 'isCheck':    是否大保存：0-否；1-是；
    - 'count':      参与排名学生数量 
    - 'rankList':   排名json 
    - 'rank':       排名
    - 'studentId':  学生id
    - 'studentName' 学生姓名
    - 'remarkName': 学生班级备注名
    - 'photo':      头像
    - 'score':      学生得分
    - 'durationTime':学生答题时长
    - 'isAnswer':   是否作答：0-未作答；1-已作答；
   
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data":{
        "workId":367,
        "workName":"367",
        "count":22,
        "rankList":[
            {
                "rank":1,
                "studentId":18,
                "studentName":"心尖偏左",
                "remarkName":"心尖偏左",
                "photo":"https://avatars1.githubusercontent.com/u/10084510?v=3&s=40",
                "score":0,
                "durationTime":0,
				"isCheck":0,
                "isAnswer":1
            },
            {
                "rank":2,
                "studentId":18,
                "studentName":"心尖偏左",
                "remarkName":"心尖偏左",
                "photo":"https://avatars1.githubusercontent.com/u/10084510?v=3&s=40",
                "score":0,
                "durationTime":0,
				"isCheck":0,
                "isAnswer":1
            }
        ]
    }
}
```

#### 8.2 获取指定的作业相关题目答题统计信息接口
> 需要token验证
> 暂无分页后面需要考虑是否分页
``` json
Url:         /mstudy/work/question/answer?workId=[workId]&token=[token]
Method:      GET
Header:      Content-Type:application/json
Parameter:
    - 'token':      	token
    - 'workId':        	作业id
Response:
    - 'status':     0->OK 操作成功 ;
                    1 参数不存在或未知错误 ;
                    2 数据库操作不实际,需求不符合错误
                    3 项目业务上的逻辑错误
    - 'count':      试题总数
    - 'workId' :    作业id
    - 'workList':   作业信息json
    - 'qNumber':    试题序号 (为试卷中试题的序号，复合题材料不算一题)
    - 'qid':        试题id
    - 'mid':        复合试题关联材料id
    - 'qType':      试题类型：1-客观题；2-主观题； 
    - 'qScore':     试题分数
    - 'qAnswer':    试题答案（只有客观题才会有）
    - 'keyList':    选项或分数段展示json
    - 'rightNumber':客观题指定试题答对人数（仅客观题有该参数）
    - 'wrongNumber':客观题指定试题答错人数（仅客观题有该参数）
    - 'key':        如果客观题型：意思为选项序号（如：0(A)、1(B)、2(C)、3(D)）;如果主观题型：意思为分数（如该题分数为5分则默认给出0,5两个key值，如果有其他分值那么就再加其他分值和对应的value即可；）
    - 'value':      表示为key值对应的人数；
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "count":12,
        "workId":12,
		"workList":[      
			{
                "qNumber":"1",
                "qid":1,
                "qType":1,
                "qScore":5.0,
                "qAnswer":[1,2],
                "rightNumber":2,
                "wrongNumber":13,
                "keyList":[
                    {
                        "key":1,
                        "value":2
                    },
                    {
                        "key":2,
                        "value":3
                    }
                ]
            },
            {
                "qNumber":"2",
                "qid":2,
                "qType":2,
                "qScore":5.0,
                "keyList":[
                    {
                        "key":1,
                        "value":2
                    },
                    {
                        "key":2,
                        "value":3
                    },
                    {
                        "key":3,
                        "value":2
                    },
                    {
                        "key":4,
                        "value":3
                    },
                    {
                        "key":5,
                        "value":3
                    }
                ]
            },
            {
                "qNumber":"3-1",
                "qid":3,
                "mid":1,
                "qType":2,
                "qScore":5.0,
                "keyList":[
                    {
                        "key":1,
                        "value":2
                    },
                    {
                        "key":2,
                        "value":3
                    },
                    {
                        "key":3,
                        "value":2
                    },
                    {
                        "key":4,
                        "value":3
                    },
                    {
                        "key":5,
                        "value":3
                    }
                ]
            },
            {
                "qNumber":"3-2",
                "qid":4,
                "mid":1,
                "qType":2,
                "qScore":5.0,
                "keyList":[
                    {
                        "key":1,
                        "value":2
                    },
                    {
                        "key":2,
                        "value":3
                    },
                    {
                        "key":3,
                        "value":2
                    },
                    {
                        "key":4,
                        "value":3
                    },
                    {
                        "key":5,
                        "value":3
                    }
                ]
            }
		] 
	}   
}
```
#### 8.3 获取指定的作业作答情况基本信息接口
> 需要token验证
``` json
Url:         /mstudy/work/statistics?workId=[workId]&token=[token]
Method:      GET
Header:      Content-Type:application/json
Parameter:
    - 'token':          token
    - 'workId':         作业id(学生作业表)
Response:
    - 'status':     0->OK 操作成功 ;
                    1 参数不存在或未知错误 ;
                    2 数据库操作不实际,需求不符合错误
                    3 项目业务上的逻辑错误
    - 'workId':         作业id
    - 'workName':       作业名字  
    - 'tid':            布置作业教师tid
    - 'pid':            作业关联试卷id
    - 'displayTime':    作业显示时间
    - 'startTime':      作业开始时间 
    - 'endTime':        作业结束时间
    - 'showAnswerType': 答案解析查看方式:1-交卷后立即查看；2-作业结束后查看
    - 'durationTime':   作业时长 
    - 'isMixed':        客观题是否乱序:0-否；1-是；
    - 'isWholeWork':    是否全班作业:0-否；1-是；
    - 'workStatus':     班级作业状态（0-未开始；1-准备中；2-进行中；3-批改中；4-长期作业中；5-客观题自动批改；6-结束；7-已撤销）
    - 'minScore':       此次作业最低分
    - 'totalScore':     此次作业总分
    - 'maxScore':       此次作业最高分
    - 'avgScore':       此次作业平均分 
    - 'totalNumber':    此次作业布置总人数
    - 'submitNumber':   此次作业交卷人数
    - 'subNumber':      该套作业主观题数量
    - 'objNumber':      该套作业客观题数量
    - 'avgAnswerTime':  该套作业已交卷人平均用时，单位s
```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data":{
        "workId":327,
        "workName":"客观题加主观题复制111",
        "tid":1,
        "pid":2,
        "displayTime":"2017-1-12 12:12:00",
        "startTime":"2017-1-5 12:12:00",
        "endTime":"2017-1-12 12:12:00",
        "showAnswerType":1,
        "durationTime":100,
        "isMixed":100,
        "isWholeWork":1,
        "workStatus":3,
        "totalScore":1,
        "minScore":0,
        "maxScore":5,
        "avgScore":4,
        "totalNumber":18,
        "submitNumber":15,
        "subNumber":18,
        "objNumber":15,
        "avgAnswerTime":15
    }
}
```

#### 8.4 获取学生在班级中最近数次得分统计接口
> 需要token验证
``` json
Url:         /mstudy/student/class/score?studentId=[studentId]&cid=[cid]&number=[number]&token=[token]
Method:      GET
Header:      Content-Type:application/json
Parameter:
    - 'token':      token
    - 'studentId':  学生id
    - 'cid':        班级id
    - 'number':     需要获取最近几次得分的次数，如，5代为最近五次
Response:
    - 'status':     0->OK 操作成功 ;
                    1 参数不存在或未知错误 ;
                    2 数据库操作不实际,需求不符合错误
                    3 项目业务上的逻辑错误
    - 'number':     获取的得分次数
    - 'scoreList':  得分数据json
    - 'getTime':    作业获取时间
    - 'submitTime': 作业交卷时间
    - 'score':      我的该作业得分
    - 'avgScore':   该作业班级平均分数
    - 'totalScore': 该作业总分
    - 'workId':     作业id
    - 'workName':   作业名字
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "number":5,
        "scoreList":[
            {
                "getTime":"2016-12-29 12:12:00",
                "submitTime":"2016-12-31 12:12:00",
                "score":50,
                "avgScore":44,
                "totalScore":100,
                "workId":1,
                "workName":"第一次期末测评"
            },
            {
                "getTime":"2016-12-29 12:12:00",
                "submitTime":"2016-12-31 12:12:00",
                "score":50,
                "avgScore":44,
                "totalScore":100,
                "workId":1,
                "workName":"第一次期末测评"
            }
        ]
    }
}
```

#### 8.5 获取学生指定班级中的统计报告接口
> 需要token验证
> 前端界面上仅保留综合分和作业次数
``` json
Url:         /mstudy/student/class/report?studentId=[studentId]&cid=[cid]&token=[token]
Method:      GET
Header:      Content-Type:application/json
Parameter:
    - 'token':      token
    - 'studentId':  学生id
    - 'cid':        班级id
Response:
    - 'status':     0->OK 操作成功；
                    1 参数不存在或未知错误；
                    2 数据库操作不实际,需求不符合错误；
                    3 项目业务上的逻辑错误；
    - 'cid':        班级id
    - 'className':  班级名字
    - 'maxNumber':  班级人数上限
    - 'userNumber': 已加入班级人数
    - 'studentId':  学生id
    - 'studentName':学生姓名
    - 'remarkName': 学生班级备注名
    - 'photo':      学生照片
    - 'overallScore':学生综合分（算法暂时为：（得分1+得分2+...+得分N）/N）
    - 'overallRanking':学生综合分排名
    - 'answerQuestionNumber':学生班级答题总量
    - 'totalQuestionNumber':班级作业总题量
    - 'totalWorkNumber':    班级作业总次数
    - 'mineWorkNumber':     我作答的班级作业次数
```
> **返回结果示例：**
``` json
{
    "status":0,
    "data":{
        "cid":89,
        "className":"89",
        "maxNumber":20,
        "userNumber":15,
        "studentId":142,
        "studentName":"saj",
        "remarkName":"123",
        "photo":"url",
        "overallScore":50,
        "overallRanking":2,
        "answerQuestionNumber":100,
        "totalQuestionNumber":100,
        "totalWorkNumber":100,
        "mineWorkNumber":100,
    }
}

```

#### 8.6 获取学生指定班级指定作业详细统计接口
>需要token验证
>可使用5.13 获取班级的用户已完成作业列表接口获取学生已完成作业列表
>2017-02-21(luodibashi):新增qScore、isRight字段；
``` json
Url:         /mstudy/student/class/work/report?studentId=[studentId]&workId=[workId]&token=[token]
Method:      GET
Header:      Content-Type:application/json
Parameter:
    - 'token':          token
    - 'studentId':      学生id
    - 'workId':         作业id
Response:
    - 'status':     0->OK 操作成功 ;
                    1 参数不存在或未知错误;
                    2 数据库操作不实际,需求不符合错误
                    3 项目业务上的逻辑错误
    - 'studentId':      学生id
    - 'workId':         作业id
    - 'workName':       作业名字
    - 'uDurationTime':  学生答题时长
    - 'totalNumber':    该次作业总题量
    - 'userScore':      学生该次作业总分
    - 'rank':           学生该地作业排名
    - 'totalScore':     该次作业总分
    - 'avgScore':       该次作业平均分
    - 'maxScore':       该次作业最高分
    - 'minScore':       该次作业最低分
    - 'qNumber':        试题序号 (为试卷中试题的序号，复合题材料不算一题)
    - 'questionList':   试题json
    - 'qid':            试题id
    - 'mid':            试题关联的材料id
    - 'qType':          试题类型：1-客观题；2-主观题；
    - 'score':          学生该题得分
    - 'qScore':         该题总分
    - 'isRight':        学生是否答对了该题：0-否；1-是；（客观题专用，主观题不需要）

```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data":{
        "studentId":100,
        "workId":327,
        "workName":"客观题加主观题复制111",
        "uDurationTime":27,
        "totalNumber":27,
        "uesrScore":0,
        "rank":1,
        "totalScore":150,
        "avgScore":0,
        "maxScore":0,
        "minScore":0,
        "questionList":[
            {
                "qNumber":"1",
                "qid":1,
                "qType":1,
                "score":1,
                "qScore":1,
                "isRight":1
            },
            {
                "qNumber":"2-1",
                "qid":1,
                "mid":1,
                "qType":1,
                "score":1,
                "qScore":1,
                "isRight":1
            }
        ]
    }
}
```

#### 8.7 获取教师相关统计信息接口
>需要token验证
>用于教师后台首页统计报告
``` json
Url:         /mstudy/teacher/report?token=[token]
Method:      GET
Header:      Content-Type:application/json
Parameter:
    - 'token':          token
Response:
    - 'status':     0->OK 操作成功 ;
                    1 参数不存在或未知错误;
                    2 数据库操作不实际,需求不符合错误
                    3 项目业务上的逻辑错误
    - 'class':       教师班级相关
    - 'work':        教师作业相关
    - 'testPaper':   教师习题试卷相关

```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data":{
        "class":{
            "total":20,            #该教师班级总数
            "number":1,            #该教师的学生总数
            "average":20           #该教师平均每个班级的人数
        },
        "work":{
            "total":20,            #该教师布置作业总次数
            "number":1,            #交作业总人次
            "average":20           #该教师平均每次作业交卷人次
        },
        "testPaper":{
            "total":20,            #该教师作业习题总份数
            "number":1,            #该教师题目总数
            "average":20           #平均每套习题题目数量
        }
    }
}
```


#### 8.8 获取教师相关最近作业情况接口
>需要token验证
>用于教师后台首页显示最近作业情况
``` json
Url:         /mstudy/teacher/work/recently?page=[page]&size=[size]&token=[token]
Method:      GET
Header:      Content-Type:application/json
Parameter:
    - 'token':          token
    - 'page':           分页参数，不填则默认为第一页
    - 'size':           每页返回数据量，不填则默认为12条
Response:
    - 'status':     0->OK 操作成功 ;
                    1 参数不存在或未知错误;
                    2 数据库操作不实际,需求不符合错误
                    3 项目业务上的逻辑错误
    - 'count':          数据总量
    - 'workList':       作业json
    - 'workId':         作业id
    - 'workName':       作业名字
    - 'cid':            作业布置的班级id
    - 'className':      班级名字
    - 'startTime':      作业开始时间
    - 'submitNumber':   交卷总人数
    - 'avgScore':       平均分
    - 'maxScore':       最高分
    - 'minScore':       最低分

```
> **返回结果示例：**
``` json
{
    "status": 0,
    "data":{
        "count":252,
        "workList":[
            {
                "workId":1,
                "workName":"第一次期中考试摸底测试",
                "cid":1,
                "className":"高一三班",
                "startTime":"2017-01-20 12:12:00",
                "submitNumber":12,
                "avgScore":12,
                "maxScore":90,
                "minScore":1
            },
            {
                "workId":1,
                "workName":"第一次期中考试摸底测试",
                "cid":1,
                "className":"高一三班",
                "startTime":"2017-01-20 12:12:00",
                "submitNumber":12,
                "avgScore":12,
                "maxScore":90,
                "minScore":1
            }
        ]
    }
}
```