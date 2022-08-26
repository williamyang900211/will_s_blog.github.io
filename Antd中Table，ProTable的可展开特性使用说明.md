# Antd中Table，ProTable的可展开特性使用说明

## 适用场景

官方指出，当表格内容较多不能一次性完全展示时，可以使用该方式

### 可替代的方式

当要展示的内容过多，且交互比较多时，可以使用List组件或卡片来展示数据。

## API

在table或protable中配置expandable参数。  

### 字段说明

#### rowExpandable

该行是否可以展开的判断逻辑

#### expandedRowRender

展开行需要展示的内容渲染函数

#### onExpand

如果需要在点击+按钮后异步获取数据，请配置此项

```js
expandable={{
        // 展开嵌套子表格 异步调取数据
        onExpand: (expanded, record) => {
          onExpandRow(expanded, record);
        },
        // 子表格渲染
        expandedRowRender: (record) => {
          return getExamines(record);
        },
        // 是否可展开
        rowExpandable: (record) => record?.total_count > 1,
      }}
```

## 常见错误

点击某行展开所有行，且每个展开行显示的都是同样的数据    
原因： 没有配置rowKey, 需在Table或ProTable中配置rowKey  
如 rowKey={record.id}

## 示例

可参考CAP项目中【评审】模块下component中的ReviewTableList组件相关代码

```js
const onExpandRow = (expanded?: any, record?: any) => {
    if (expanded) {
      dispatch({
        type: 'review/fetchHistory',
        payload: {
          group_id: record?.group_id,
          id: record?.id,
        },
        callback: (items: any) => {
          setChildList({
            ...childList,
            [record.group_id]: items,
          });
        },
      });
    }
  };
const getExamines = (record: any) => {
    console.log('aa', childList);
    return (
      <Space direction="vertical">
        {childList[record?.group_id]?.map((examine: any, dataIndex: number) => {
          return (
            <Row>
              <Col push={2}>
                <Space>
                  <span> 第{dataIndex + 1}次评审</span>
                  <Divider type="vertical" />
                  <EnvironmentOutlined />
                  <span>{examine?.review_room}</span>
                  <Divider type="vertical" />
                  <ClockCircleOutlined />
                  <span>{examine?.review_date}</span>
                  <Divider type="vertical" />
                  <Popover content={showReqCaseLinkPop(examine)}>
                    <Button type="link">Req | Case | Scheme Doc</Button>
                  </Popover>
                </Space>
              </Col>
            </Row>
          );
        })}
      </Space>
    );
  };
  <ProTable<ReviewDataType>
      size="default"
      search={false}
      pagination={{ defaultPageSize: 10 }}
      toolBarRender={false}
      columns={columns}
      dataSource={list}
      rowKey={(record) => record.group_id}
      expandable={{
        // 展开嵌套子表格 异步调取数据
        onExpand: (expanded, record) => {
          onExpandRow(expanded, record);
        },
        // 子表格渲染
        expandedRowRender: (record) => {
          return getExamines(record);
        },
        // 是否可展开
        rowExpandable: (record) => record?.total_count > 1,
      }}
    />
```

更多功能请移步antd官方文档table组件页面[expandable相关内容](https://ant.design/components/table-cn/#expandable)
