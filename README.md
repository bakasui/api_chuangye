# 主打图片翻译及境外路线规划的境外单人旅行助手-singular 2.0版本

### [点此查看diff链接【查看md文档变化请往下滑，点击加载差异】](https://gitee.com/bakasui/api_final/compare/b249bc3920ecf0392a4236bad293698da8b8f20b...e753ab92e1b954076112a771bdd8a25ca1a7d224)

### [ppt视频](https://v.qq.com/x/page/u3121dmfrqi.html)

---
### 迭代部分阐述

迭代二版本新增了利害相关分析和用户旅程地图、ESG考量，并且在原有段落增加了许多细节描述，整篇文档的结构相较于迭代一更为完整、翔实。

---
## 项目名称：singular

### 价值主张宣言

- **问题情境**：单人旅客因语言不通花费过多时间，内向旅客与陌生人问路时会感到尴尬。
- **解决方案**：本产品主要使用的API是有道智云图片翻译API，用户通过该APP就能够使用内置相机拍摄图片，实时翻译路标、菜单等信息，节省单人旅客因语言障碍需要花费的时间，同时也可以避免害羞旅客与陌生人沟通的尴尬。
- **人工智能概率性**：有道智云图片翻译API响应速度较快、精准度高，有比较优秀的人工智能概率性



## 问题表述与需求列表

### 问题表述

#### 用户使用情境

| 使用场景 | 任务 | 痛点 |
| -- | -- | -- | 
| 平手拿到景区地图，发现没有中文注释，无法看懂 | 使用APP拍照 | 实时翻译文字 |
| 平手不知道应该如何前往下一个景点 | 输入目的地并检索 | 针对目的地进行路线规划 |
| 平手逛商场时看到一个陌生的品牌，她想知道是什么 | 使用APP拍照 | 获取不熟悉的品牌信息 |

#### 用户画像

![用户画像](https://gitee.com/bakasui/api_final/raw/master/%E5%B0%8F%E5%B9%B3%E7%94%BB%E5%83%8F.jpg)

本APP为单人前往境外且有翻译外语需求的旅客而打造，使用的拍照翻译API具有一定的精确度，极大地节约了单人旅客花费在问路、使用搜索引擎查询时花费的时间，并且很好地解决内向旅客在境外出行时的窘迫情景。

### 需求列表
| 优先级 | 需求 | 智能加值 | API类型 | 
|  --  |  --  |  --  |  --  |
| 1 | 用户在旅行过程中实时拍照识别路标、菜单等信息 | 识别图片内文字并翻译 | 百度拍照翻译API |
| 2 | 用户输入目的地进行预先行程规划或安排当下的行程 | 根据目的地坐标规划路线 | 百度境外路线规划API |
| 3 | 在旅行过程中用户对不熟悉的品牌进行识别 | 识别图片颜色形状以判别 | 百度品牌LOGO识别API |

### 关键API的利害相关者分析

- **利害相关者图**

![lihai](https://gitee.com/bakasui/api_final/raw/master/%E5%88%A9%E5%AE%B3%E7%9B%B8%E5%85%B3.png)

- **ESG考量**

## 界面流程及关键智能交互

### 用户旅程地图

![lvcheng](https://gitee.com/bakasui/api_final/raw/master/%E7%94%A8%E6%88%B7%E6%97%85%E7%A8%8B.png)

### IDEO三要素&智能交互

| 要素 | 加值 |
| -- | -- |
| 商业可行性 | 用户群体稳定，有迭代发展前景 |
| 技术可行性 | 图片翻译与路线规划功能简洁明了，易于设计 |
| 用户可欲性 | 内向的境外出行旅客将对图片翻译功能有一定需求 |

### 产品原型交互图

<img src="https://gitee.com/bakasui/api_final/raw/master/%E4%BA%A4%E4%BA%92.png" width="60%"></img>
<img src="https://gitee.com/bakasui/api_final/raw/master/%E4%BA%A4%E4%BA%922.png" width="100%"></img>

## 数据流程及关键智能API使用

### 数据流程分析

![数据流程图](https://gitee.com/bakasui/api_final/raw/master/%E6%95%B0%E6%8D%AE%E6%B5%81%E7%A8%8B.jpg)

### IDEO三要素&数据流程

| 要素 | 加值 |
| -- | -- |
| 商业可行性 | APP使用的API调用价格都比较低，一般企业能够承担 |
| 技术可行性 | API接口易于连接，数据传输过程简明迅速 |
| 用户可欲性 | 用户简单的操作就能获得想要的数据 |

###代码及数据展示加值

1. **有道智云图片翻译API**

- 输入数据
```
# -*- coding: utf-8 -*-
import sys
import uuid
import requests
import base64
import hashlib

from imp import reload

reload(sys)

YOUDAO_URL = 'https://openapi.youdao.com/ocrtransapi' # 输入图片地址
APP_KEY = 'ID'
APP_SECRET = 'KEY'


def truncate(q):
    if q is None:
        return None
    size = len(q)
    return q if size <= 20 else q[0:10] + str(size) + q[size - 10:size]


def encrypt(signStr):
    hash_algorithm = hashlib.md5()
    hash_algorithm.update(signStr.encode('utf-8'))
    return hash_algorithm.hexdigest()


def do_request(data):
    headers = {'Content-Type': 'application/x-www-form-urlencoded'}
    return requests.post(YOUDAO_URL, data=data, headers=headers)


def connect():
    f = open(r'./shi.jpg', 'rb')  # 二进制方式打开图文件
    q = base64.b64encode(f.read()).decode('utf-8')  # 读取文件内容，转换为base64编码
    f.close()

    data = {}
    data['from'] = 'jp'
    data['to'] = 'en'
    data['type'] = '1'
    data['q'] = q
    salt = str(uuid.uuid1())
    signStr = APP_KEY + q + salt + APP_SECRET
    sign = encrypt(signStr)
    data['appKey'] = APP_KEY
    data['salt'] = salt
    data['sign'] = sign

    response = do_request(data)
    print(response.content)


if __name__ == '__main__':
    connect()
```
- 输出结果
![翻译结果](https://gitee.com/bakasui/api_final/raw/master/15499869-b0ae7f8b71a03fdb.png)

2. **百度境外路线规划API**
- 输入数据
```
#-*- coding:utf-8 -*-
 
import urllib.parse
import urllib.request
import json
 
key='62PM27fx1Fbhq1pOWdpRlWt2o9B8tL9O'

class locationXY:
        def __init__(self,x,y):
                self.x=x
                self.y=y


def 计算所有路线(origin,destionation):
        tactics_incity=0       
        data = urllib.parse.urlencode({'origin':'%s,%s'%(origin.y,origin.x),'destination':'%s,%s'%(destionation.y,destionation.x),'tactics_incity':tactics_incity,'ak':key})
        response = urllib.request.urlopen('http://api.map.baidu.com/direction/v2/transit?%s' % data)
        html = response.read()
        data = html.decode('utf-8')
        result = json.loads(data)
        #print(data)
        路线总数 = result['result']['total']
        if (result['status']==0):
                for x in range(路线总数):
                                if (result['status']==0):
                                        distance=result['result']['routes'][x]['distance']
                                        duration=result['result']['routes'][x]['duration']
                                        print('路线：%s,距离%s米，花费%s分钟'%(x,distance,duration/60))
        else:
                print('error : %d'%result['status']）
 

def main():
        l1 = locationXY(113.464838,23.111949) # 大沙东地铁站的坐标
        l2 = getLocation('御富科贸园b2座205-20') # 当前坐标
        #print("%s\n%s"%(l1.x,l1.y))
        计算所有路线(l1,l2)
        #l2 = 
if __name__ == '__main__':
        main()
```
- 输出结果

![ditu](https://gitee.com/bakasui/api_final/raw/master/QQ%E5%9B%BE%E7%89%8720200718215956.png)

3. **品牌LOGO识别API**
- 输入数据
```
# encoding:utf-8

import requests
import base64

request_url = "https://aip.baidubce.com/rest/2.0/image-classify/v2/logo"# 输入logo图片地址
f = open('[./lego.jpg]', 'rb')
img = base64.b64encode(f.read())

params = {"custom_lib":true,"image":img}
access_token = '[24.6c5e1ff107f0e8bcef8c46d3424a0e78.2592000.1485516651.282335-8574074]'
request_url = request_url + "?access_token=" + access_token
headers = {'content-type': 'application/x-www-form-urlencoded'}
response = requests.post(request_url, data=params, headers=headers)
if response:
    print (response.json())

```
- 输出结果
```
HTTP/1.1 200 OK
x-bce-request-id: 73c4e74c-3101-4a00-bf44-fe246959c05e
Cache-Control: no-cache
Server: BWS
Date: Tue, 18 Oct 2016 02:21:01 GMT
Content-Type: application/json;charset=UTF-8
{
  "log_id": 843411868,
  "result_num": 1,
  "result": [
    {
      "type": 0,
      "name": "乐高",# 返回品牌信息
      "probability": 0.99998807907104,
      "location": {
        "width": 296,
        "top": 20,
        "height": 128,
        "left": 23
      }
    }
  ]
}
```

### 数据流程图

![数据流程图](https://gitee.com/bakasui/api_final/raw/master/%E6%95%B0%E6%8D%AE%E6%B5%81%E7%A8%8B.jpg)

### 人工智能概率性考量

1. **[有道智云图片翻译API](https://ai.youdao.com/product-fanyi-picture.s)**

- 支持对多个国家的语种进行自动识别，适应不明确翻译语种等多复杂场景，提供离线图片翻译服务，确保无网络或网络不佳等情况下稳定的翻译服务，适应各种实际生活中的异常情况，如光照不均、倾斜、模糊等，具备非常高的复杂环境可用性。
- 可在客户端和web应用中内置图片翻译功能，实现特定场景下的实时翻译，海量并发支持实时翻译，准确性高，提供业界领先的文字翻译服务，利用有道深度学习技术及用户翻译习惯不断优化算法迭代模型，不断提高服务质量。

2. **[百度境外路线规划API](http://lbsyun.baidu.com/index.php?title=webapi/direction-api-abroad)**
- 境外路线规划服务，支持中国港、澳、台，以及海外国家/地区的出行路线规划服务能力。线路规划方式上支持：驾车、公交、步行。
- 用户可通过该服务，根据起点和终点检索符合条件的出行路线规划方案。可选择出行方案（公交、驾车、步行），可融入出行策略（时间优先、距离优先等）。
3. **[百度品牌LOGO识别API](https://ai.baidu.com/tech/imagerecognition/logo)**
- 识别超过2万类商品logo，支持用户创建属于自己的品牌logo图库，可准确识别图片中品牌logo的名称，适用于需要快速获取品牌信息的业务场景中。
- 根据拍摄照片，识别图片中商品logo名称，加快品牌信息获取速度，给消费者轻松高效的信息获取体验。

**结论**：本APP使用的API人工智能概率性都比较高

### 实践心得总结及感谢
#### 心得总结
- 有些API在调用时需要先填写所在平台的工单，审核通过之后才能开放调用。
- 部分API是作为SDK形式存在的，这类API无法在web端进行调用，适用于小程序/APP开发。
#### 感谢语
- 本文档内使用的API来自于[有道智云API开放平台](https://ai.youdao.com/gw.s)与[百度开放平台](https://ai.baidu.com/)，参考文档分别为[有道智云图片翻译API技术文档](http://ai.youdao.com/DOCSIRMA/html/%E8%87%AA%E7%84%B6%E8%AF%AD%E8%A8%80%E7%BF%BB%E8%AF%91/API%E6%96%87%E6%A1%A3/%E5%9B%BE%E7%89%87%E7%BF%BB%E8%AF%91%E6%9C%8D%E5%8A%A1/%E5%9B%BE%E7%89%87%E7%BF%BB%E8%AF%91%E6%9C%8D%E5%8A%A1-API%E6%96%87%E6%A1%A3.html)、[百度路线规划API技术文档](http://lbs.baidu.com/index.php?title=jspopular3.0/guide/routeplan)、[百度LOGO识别API技术文档](https://ai.baidu.com/ai-doc/IMAGERECOGNITION/Ok3bcxc59)，感谢这两个平台开发智能产品的努力。
- 对本课程的知识掌握来源于老师们的慷慨奉献，感谢许智超老师和廖汉腾老师的悉心教导。
