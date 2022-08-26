# 前端对接OpenApi

## 目的

提高与后端沟通的效率，使用openapi文档记录接口相关信息。
通过antdesign提供的工具解析openapi（json）文件，生成前端service.ts文件，mock数据等以往需要频繁改动且重复的工作。
配合antdesign可以在前端实时查看后端定义的接口信息。

## 使用

目前脚手架已经集成相关插件。
做好相关配置后，在前端脚手架目录下使用`npm run openapi`指令即可生成对应service文件及mock数据。

### config 文件配置示例

```js
openAPI: [
    {
      requestLibPath: "import { request } from 'umi'", //指定使用的request库
      // 或者使用在线的版本
      // schemaPath: "https://gw.alipayobjects.com/os/antfincdn/M%24jrzTTYJN/oneapi.json"
      schemaPath: join(__dirname, 'oneapi.json'),

      mock: false,
    },
    {
      requestLibPath: "import { request } from 'umi'",
      schemaPath: join(__dirname, 'homepage4.json'),
      projectName: 'Homepage4',
      apiPrefix: '"homeapi"', //注意此处的格式
      mock: true,
    },
  ]
```

#### 参数说明

1. *schemaPath*：可以使用在线的url也可以指定json文件所在目录
2. *mock*：设置为true时才生成mock数据
3. *requestLibPath*：指定要使用的request库
4. *projectName*：生成的service子文件夹名称
5. *apiPrefix*：生成的接口前缀部分的内容
**注意apiPrefix的格式，如果为'api'则会生成${api}样式的前缀，取全局变量，要制定为常量需要使用'"api"'这样的格式。（原因不明）**

### 生成文件位置

1. service文件在./src/service/${projectName}
2. mock文件在./mock/ 下，一个接口一个ts文件

## 存在的问题

1. 无法支持最新的swagger特性
2. 不支持header params
3. 不支持某些params定义的复用（需要后端同学进一步说明）
4. 进支持2.0版本的swagger

## 结论

确实提供了方便的功能，能在一定程度上提高前端开发阶段的效率，但有一定接入成本，且不太成熟。建议酌情使用。
