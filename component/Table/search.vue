<!--
 * @Descripttion: 表格组件搜索部分
 * @version: 1.0.0
 * @Author: lw
 * @Date: 2021-03-16 19:06:43
 * @LastEditTime: 2021-03-30 18:22:53
-->
<template>
  <el-form ref="form" :model="form" inline size="medium">
    <el-form-item
      :label="item.label + '：'"
      v-for="item in fields"
      :key="item.prop"
    >
      <!-- 下拉框 -->
      <el-select
        v-model="form[item.key || item.prop]"
        v-if="item.type === 'select'"
        clearable
        :placeholder="item.placeholder || '请选择'"
      >
        <el-option
          v-for="op in selectOption[item.prop]"
          :key="op[item.sourceField ? item.sourceField.value : 'value']"
          :value="op[item.sourceField ? item.sourceField.value : 'value']"
          :label="op[item.sourceField ? item.sourceField.label : 'label']"
        ></el-option>
      </el-select>
      <!-- 日期选择器 -->
      <el-date-picker
        v-else-if="item.type === 'daterange'"
        v-model="temp[item.prop]"
        type="daterange"
        range-separator="~"
        start-placeholder="开始日期"
        end-placeholder="结束日期"
        value-format="timestamp"
        @change="handleDateChange(item)"
        :picker-options="item.pickerOptions || pickerOptions"
        v-bind="item.formItemProps"
      >
      </el-date-picker>
      <!-- 日期时间选择器 -->
      <el-date-picker
        v-else-if="item.type === 'datetimerange'"
        v-model="temp[item.prop]"
        type="datetimerange"
        range-separator="~"
        start-placeholder="开始日期"
        end-placeholder="结束日期"
        value-format="timestamp"
        @change="handleDateTimeChange(item)"
        :picker-options="pickerOptions"
        v-bind="item.formItemProps"
      >
      </el-date-picker>
      <!-- 员工选择器 -->
      <emp-select @getEmpId="ids => setEmpId(ids,item)" v-else-if="item.type === 'emp'"></emp-select>
      <!-- 输入框 -->
      <el-input
        v-model="form[item.key || item.prop]"
        v-else
        clearable
        :placeholder="item.placeholder || '请输入'"
      ></el-input>
    </el-form-item>
  </el-form>
</template>
<script>
import { debounce } from 'throttle-debounce'
import empSelect from '@/components/EmpSelectNew'
export default {
  props: ['fields'],
  components: {
    empSelect
  },
  data () {
    const disabledDate = (time) => {
      let _date = new Date()
      if (_date.toString().indexOf('GMT-08:00') > -1) {
        _date = new Date((_date.getTime() + (1 / 24 * 16 * 8.64e7)))
      }
      const _times = this.$day(this.$day(_date).format('YYYY-MM-DD') + ' 23:59:59')
      return time.getTime() > _times.valueOf()
    }
    return {
      form: {},
      temp: {},
      selectOption: {},
      pickerOptions: {
        disabledDate: (time) => {
          return disabledDate(time)
        }
      }
    }
  },
  watch: {
    form: {
      handler (n, o) {
        this.debounceChange(n, o)
      },
      deep: true
    }
  },
  mounted () {
    this.debounceChange = debounce(300, this.handlerChange)
    this.initFrom()
  },
  methods: {
    initFrom () {
      if (!this.fields.length) {
        this.$emit('change', {})
      }
      this.fields.map(e => {
        if (e.key && e.key instanceof Array) {
          this.$set(this.temp, e.prop, [])
          e.key.map(key => {
            this.$set(this.form, key, '')
          })
        } else {
          this.$set(this.form, e.key || e.prop, e.default === undefined ? '' : e.default)
        }

        if (e.type === 'select') {
          if (e.source instanceof Array) {
            this.selectOption[e.prop] = e.source
          } else {
            e.source().then((res) => {
              if (res.code === 0) {
                this.$$message.error(res.msg)
              } else {
                this.selectOption[e.prop] = res.data
              }
            })
          }
        }
      })
    },
    setEmpId (ids, item) {
      this.form[item.key || item.prop] = ids
    },
    handlerChange (newForm) {
      const params = {}
      for (const key in newForm) {
        if (newForm[key]) {
          params[key] = newForm[key]
        }
      }
      this.$emit('change', params)
    },
    handleDateChange (item) {
      item.key.map((e, i) => {
        if (i === 0) {
          this.form[e] = this.temp[item.prop] ? this.temp[item.prop][i] / 1000 : ''
        } else {
          this.form[e] = this.temp[item.prop] ? this.temp[item.prop][i] / 1000 + 24 * 60 * 60 - 1 : ''
        }
      })
    },
    handleDateTimeChange (item) {
      item.key.map((e, i) => {
        this.form[e] = this.temp[item.prop] ? this.temp[item.prop][i] / 1000 : ''
      })
    }
  }
}
</script>
<style lang="scss" scoped>
::v-deep {
  .el-form-item__label {
    padding: 0;
  }
  .el-form-item {
    margin-bottom: 24px;
    margin-right: 32px !important;
  }
  .el-input--medium .el-input__inner{
    width: 224px;
  }
  .el-date-editor--daterange.el-input__inner{
    width: 224px!important;
  }
  .el-date-editor--datetimerange.el-input__inner{
    width: 300px;
  }
}
</style>
