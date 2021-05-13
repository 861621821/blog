# Table表格
基于[el-table](https://element.eleme.cn/#/zh-CN/component/table) [el-form](https://element.eleme.cn/#/zh-CN/component/form) [el-pagination](https://element.eleme.cn/#/zh-CN/component/pagination) 

#### 快速上手
1.引入ly-table的mixin文件，并混入

``` js
import lyTable from '@/components/mixins/ly-table.js'
export default {
  mixins: [lyTable]
  ...
}
```
2.创建http.js，并配置相关接口地址
``` js
import axios from '@/common/ajax.js'
const { stringify } = require('qs')
export const recordsApi = {
  reqQuery: (params) => axios.get(`/wecom/robot/login/history?${stringify(params)}`), // 查询表格数据的接口
  getOption: (params) => axios.get(`/wecom/api/test?${stringify(params)}`) // 查询搜索框下拉选项的接口
}
```
!> 注意：reqQuery、reqDelete不可更改  

3.引入http.js，并在data中定义状态：COMM_HTTP
``` js
import { recordsApi } from './http.js'
...
export default {
  data () {
    return {
      COMM_HTTP: recordsApi, // 表格内所有接口配置,必要值
    }
  }
}
```

#### Attributes
| 属性  | 类型    | 说明 |  默认值 |
| ---- |  ----  | ----  | ----  |
| data |  Array | 表格数据 | -  |
| option | Object  | 表格配置[option](/doc/table?id=option)  | -  |
| table-loading |  Boolean  | loading效果  | -  |
| page |  Number  | 当前页码  | -  |
| pageSize |  Number  | 分页尺寸  | -  |
| total |  Number  | 数据总条数  | -  |

#### Events 
| 事件名  | 说明    | 参数 |
| ---- |  ----  | ----  |
| events | 监听内置操作列方法的事件 | event |
| searchChange  | 过滤条件改变时触发的事件  | params  |
| sizeChange  | 分页尺寸改变时触发的事件  | size  |
| currentChange  | 当前页面改变时触发的事件  | page  |
| selectionChange  | 当选择项发生变化时会触发该事件  | selection  |

#### option  
| 配置项  | 类型   | 说明 |  默认值 |
| ---- |  ----  | ----  | ----  |
| tableKey | String | 表格的key | - |
| setting | Boolean | 是否可对表格设置 | false |
| showSelection | Boolean | 是否显示选择列 | false |
| showIndex | Boolean | 是否显示序号 | false |
| operation | String/Array | 是否显示操作栏，为hide时隐藏，内置：`detail`, `edit`, `delete` | - |
| operationWidth | String | 操作列的宽度 | 自适应 |
| levitate | Boolean | 是否开启悬浮分页，一个页面多个表格组件开启悬浮，会出现异常 | true |
| scrollEl | String | 监听滚动元素的选择器，详情参考[scrollEl](/doc/table?id=scrollEl)说明 | 默认为表格组件的父元素 |
| column | Array | 列数据[column](/doc/table?id=column) | - |
| layout | String | 分页组件功能，同[el-pagination](https://element.eleme.cn/#/zh-CN/component/pagination) | 完整分页 |

#### column  
| 配置项 | 类型   | 说明 | 默认值 |
| ---- |  ----  |----  |----  |
| label |  String  | 表格头以及搜索表单的label | - |
| prop |  String  | 字段 | - |
| hide |  Boolean  | 是否在表格中隐藏此字段 | false |
| hideable |  Boolean  | 是否可隐藏此字段 | false |
| search |  Boolean  | 是否是搜索栏位 | false |
| searchSlot |  String  | 开启搜索栏位插槽，值为插槽name | - |
| sort |  Number  | 对搜索栏位排序 | - |
| key |  String/Array  | 自定义搜索条件的key | - |
| type |  String  | 表单类型：`input`、`select`、`emp`、`daterange`、`datetimerange` | input |
| pickerOptions | Object | 同el-date-picker的Picker Options | 默认不能选择当前时间之后的时间，如不需要限制，传入一个空对象即可 |
| default |  String  | 表单默认值 | - |
| placeholder |  String  | 表单原生属性placeholder | 请输入/请选择 |
| slot |  String  | 开启插槽，值为插槽name | - |
| columnProps |  Object  | 表格列的属性，可以是任何el-table支持的属性 | - |
| formItemProps |  Object  | 表单的属性，可以是任何表单支持的属性 | - |
| source |  Array/Function  | 下拉选项的数据源：数组形式表示手动传入选项；方法形式表示接口形式动态获取选项 | - |
| sourceField | Object | 修正选项的key： `{label: 'name',value: 'id'}` | - |
| width |  String  | 列的宽度 | - |
| fixed |  Boolean  | 是否是固定列，同el-table | false |
| sortable |  Boolean  | 是否可以排序，同el-table | false |
| formatter |  Function  | 格式化内容，同el-table | - |
| showOverflowTooltip |  Boolean  | 溢出是否显示tooltip | false |

#### 其他 
+ 状态：DATA_LOCATION
> `DATA_LOCATION: 'xxx'`  修正接口中表格数据所在的位置，数据在list或者data下时不需要设置。比如下面这种情况就需要配置`DATA_LOCATION: 'test'`
``` js
{
    "code":200,
    "data":{
      "total":106,
      "test":[] // 这里的key为test
    }
}
```
+ 状态：IMMEDIATE
> 是否立即请求数据，默认为true。例如在dialog中使用ly-table组件时，在打开dialog前就会请求数据，如果想要在dialog打开的时候请求数据那么就配置状态`IMMEDIATE: false`
+ 状态：FIELD
> 修正内置删除、更新、修改操作的id字段，默认以`id`为key执行内置删除、更新、修改操作。比如在某个表格内使用内置删除功能，删除接口接收userId参数，那么就配置`FIELD: userId`，这时候传给后台的参数就是`userId: xxx`
+ 方法：formatterParams 
> 定义formatterParams 方法，方法接收params参数，组件将自动调用并混入自定义/格式化参数，方法的返回值为对象，用于设置自定义或者格式化参数。
``` js
formatterParams (params) {
    return {
      ...params,
      begin_at: params.begin_at / 1000,
      end_at: params.end_at / 1000
    }
}
```

#### 插槽
+ handler 功能区左侧插槽
+ handlerright 功能区右侧插槽 
+ operation 操作列插槽
``` html
<template #handler>
    <span>共 1 个客户</span>
</template>
```

#### scrollEl  
> 默认情况下，表格的悬浮分页功能是通过监听表格组件的父元素：`document.querySelector('.xl-ly-table').parentNode`实现的。在实际开发过程中，有可能出现需要重新布局导致实际滚动的元素不是
> `document.querySelector('.xl-ly-table').parentNode`，所以就需要指定`scrollEl`。例如：`scrollEl: '.page-warp-bg'`
#### 基础用法
``` html
<template>
  <div class="records-warp">
    <ly-table
      ref="lyTable"
      :data="tableData"
      :option="tableOpt"
      :table-loading="loading"
      :page="page"
      :pageSize="per_page"
      :total="total"
      @searchChange="searchChange"
      @sizeChange="handleSizeChange"
      @currentChange="handleCurrentChange"
      @selectionChange="handleSelectionChange"
    >
      <!-- 搜索插槽 -->
      <!-- 功能区插槽 -->
      <!-- 列插槽 -->
      <template #user_name_slot="{row}">
        <template v-if="row.WecomUserID != 0">
          <img class="avatar" :src="row.user_avatar"/>
          <span style="margin-left: 10px">{{ row.user_name }}</span>
        </template>
        <template v-else>
          <span class="other-icon">外</span>
          <span class="other-name">非本企业账号</span>
        </template>
      </template>
    </ly-table>
  </div>
</template>
<script>
import lyTable from '@/components/mixins/ly-table.js' // 引入组件公共方法
import { recordsApi } from './http.js' // 定义http.js 用来管理组件需要用到的接口
export default {
  mixins: [lyTable],
  data () {
    return {
      COMM_HTTP: recordsApi, // 表格内所有接口配置
      DATA_LOCATION: 'list' // 接口中表格数据所在的位置 默认为list
    }
  },
  computed: {
    tableOpt () {
      return {
        showSelection: false, // 是否显示选择框 默认隐藏
        showIndex: false, // 是否显示序号 默认隐藏
        operation: [ // 是否显示操作栏 为空或者不传时隐藏  内置：'detail', 'edit', 'delete'
          'detail',
          {
            label: '下线',
            handler (row) { console.log(row) } // 自定义按钮的回调
          }
        ],
        operationWidth: 80, // 操作栏宽度
        // layout: 'prev, pager, next', // 自定义分页组件的功能
        column: [
          {
            label: '登录账号',
            prop: 'user_name',
            search: true, // 将该字段设为搜索项
            placeholder: '请输入账号名称',
            slot: 'user_name_slot' // 使用插槽，自定义显示格式
          },
          {
            label: '登录时间',
            prop: 'login_at',
            search: true,
            type: 'datetimerange', // 设置搜索类型为日期时间选择器
            key: ['begin_at', 'end_at'], // 自定义搜索条件的key
            formItemProps: { // 其他表单属性
              style: 'width: 360px',
              'default-time': ['00:00:00', '23:59:59']
            },
            formatter: (row, column, cellValue, index) => { // 格式化单元格
              return row.login_at === 0 ? '-' : this.util.timeTofmt(cellValue, 'yyyy-MM-dd hh:mm')
            }
          },
          {
            label: '下线时间',
            prop: 'login_out_at',
            formatter: (row, column, cellValue, index) => {
              return row.login_out_at === 0 ? '-' : this.util.timeTofmt(cellValue, 'yyyy-MM-dd hh:mm')
            }
          },
          {
            label: '扣除云豆',
            prop: 'bean',
            columnProps: { // 其他列属性
              'class-name': 'cost'
            }
          }
        ]
      }
    }
  }
}
</script>
```
