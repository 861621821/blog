# 省市区三级联动
基于[el-popover](https://element.eleme.cn/#/zh-CN/component/popover)
> 省市区三级联动组件，支持搜索，默认显示全国省市区信息，如需显示国外或国内部分省市区信息，可通过data字段传入
#### 示例

<img src="https://861621821.github.io/blog/images/component/省市区.gif">

#### npm安装
``` js
npm i xl-cascader --save
```
#### 开始使用
``` js
import xlCascader from 'xl-cascader'

Vue.use(xlCascader)
```

### 基础用法 

```html
<template>
  <el-form ref="form" inline :model="form" label-width="80px">
    <el-form-item label="活动名称">
      <el-input clearable v-model="form.name"></el-input>
    </el-form-item>
    <el-form-item label="省市区">
      <xl-cascader v-model="form.ssq" />
    </el-form-item>
  </el-form>
</template>
<script>
  export default {
    data () {
      return {
        form: {
          name: '',
          ssq: []
        }
      }
    }
  }
</script>
```

### Attributes
| 参数 | 类型 | 说明 | 可选值 | 默认值 |
| - | - | - | - | - |
| size | string | 用于控制该表单内组件的尺寸 | `medium`  `small`  `mini` | - |
| clearable | boolean | 是否可清空 | - | true |
| height | number | 弹出框的高度 | - | 300 |
| data | array | 默认情况下展示全国所有省市区，如果只想展示部分，手动传入 | - | - |
#### 自定义数据源
``` html
<template>
  <xl-cascader v-model="ssq" :data="ssqData"></xl-cascader>
</template>
<script>
  export default {
    data(){
      return{
        ssq: [],
        ssqData: [
          {
            name: '湖北省',
            districtList: [
              {
                name: '武汉市',
                districtList: [
                  { name: '江夏区' },
                  { name: '洪山区' },
                  { name: '武昌区' }
                ]
              },
              {
                name: '荆门市',
                districtList: [
                  { name: '东宝区' },
                  { name: '掇刀区' },
                  { name: '沙洋县' }
                ]
              }
            ]
          },
          {
            name: '广东省',
            districtList: [
              {
                name: '深圳市',
                districtList: [
                  { name: '南山区' },
                  { name: '宝安区' },
                  { name: '福田区' }
                ]
              },
              {
                name: '广州市',
                districtList: [
                  { name: '天河区' },
                  { name: '番禺区' },
                  { name: '越秀区' }
                ]
              }
            ]
          },
          {
            name: '河南省',
            districtList: [
              {
                name: '信阳市',
                districtList: [
                  { name: '淮滨县' },
                  { name: '罗山县' }
                ]
              },
              {
                name: '南阳市',
                districtList: [
                  { name: '西峡县' },
                  { name: '卧龙区' }
                ]
              }
            ]
          }
        ]
      }
    }
</script>
```
