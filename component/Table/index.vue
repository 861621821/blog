<!--
 * @Descripttion: saas平台表格组件，基于el-table
 * @version: 1.0.0
 * @Author: lw
 * @Date: 2021-03-16 18:49:51
 * @LastEditTime: 2021-04-02 17:35:57
-->
<template>
  <div
    class="xl-ly-table"
    v-loading="$attrs['table-loading']"
    element-loading-spinner="lyloading"
    element-loading-background="rgba(255, 255, 255, 0.5)"
  >
    <!-- 搜索区域 -->
    <search :fields="getSearchFields()" @change="handleSearchChange"></search>
    <!-- 设置及自定义区域区域 -->
    <setting>
      <slot name="handler"></slot>
      <template #handlerright>
        <slot name="handlerright"></slot>
      </template>
    </setting>
    <!-- 表格主体 -->
    <el-table
      ref="elTable"
      stripe
      :data="$attrs.data"
      style="width: 100%"
      :header-cell-style="{ background: 'rgba(0, 0, 0, .02', color: '#262626' }"
      @selection-change="(val) => $emit('selectionChange', val)"
    >
      <el-table-column v-if="option.showSelection" type="selection">
      </el-table-column>
      <el-table-column
        v-if="option.showIndex"
        type="index"
        :index="getIndex"
        label="序号"
      >
      </el-table-column>
      <template v-for="column in columns">
        <el-table-column
          v-if="column.slot"
          :prop="column.prop"
          :key="column.prop"
          :label="column.label"
          :width="column.width"
          :fixed="column.fixed"
          :show-overflow-tooltip="column.showOverflowTooltip"
          :sortable="column.sortable"
          :formatter="column.formatter"
          v-bind="column.columnProps"
        >
          <template slot-scope="scope">
            <slot :row="scope.row" :name="column.slot"></slot>
          </template>
        </el-table-column>
        <el-table-column
          v-else
          :prop="column.prop"
          :key="column.prop"
          :label="column.label"
          :width="column.width"
          :fixed="column.fixed"
          :show-overflow-tooltip="column.showOverflowTooltip"
          :sortable="column.sortable"
          :formatter="column.formatter || formatter"
          v-bind="column.columnProps"
        >
        </el-table-column>
      </template>
      <!-- 操作列 -->
      <el-table-column
        v-if="option.operation !== 'hide'"
        label="操作"
        class-name="operation-cell"
        :width="option.operationWidth"
        >
        <template slot-scope="scope">
          <el-button :type="item.type || 'text'" size="small" @click="item.handler(scope.row)" v-for="item in operationMenu" :key="item.label">{{item.label}}</el-button>
          <slot :row="scope.row" name="operation"></slot>
        </template>
      </el-table-column>
    </el-table>
    <!-- 分页 -->
    <div class="table-pagination-area">
      <el-pagination
        background
        :class="{ fixedPagination }"
        :current-page="page"
        :page-sizes="[10, 20, 50, 100]"
        :page-size="pageSize"
        :layout="option.layout || 'total, sizes, prev, pager, next, jumper'"
        :total="total"
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
      />
    </div>
  </div>
</template>
<script>
import search from './search'
import setting from './setting'
import { debounce } from 'throttle-debounce'
export default {
  components: {
    search, setting
  },
  data () {
    return {
      fixedPagination: false
    }
  },
  watch: {
    '$attrs.data': {
      handler () {
        this.option.levitate !== false && this.initScroll()
      },
      deep: true
    }
  },
  computed: {
    option () {
      return this.$attrs.option
    },
    columns () {
      return this.option.column.filter(e => {
        return !e.hide
      })
    },
    page () {
      return this.$attrs.page
    },
    pageSize () {
      return this.$attrs.pageSize
    },
    total () {
      return this.$attrs.total
    },
    el () {
      return document.querySelector('.xl-ly-table').parentNode
    },
    scrollThrottle () {
      return debounce(50, this.handleScroll)
    },
    operationMenu () {
      const operation = this.$attrs.option.operation || []
      return operation.map(e => {
        if (typeof e === 'string') {
          if (e === 'detail') {
            return {
              label: '详情',
              handler: this.detail
            }
          } else if (e === 'delete') {
            return {
              label: '删除',
              handler: this.delete
            }
          } else if (e === 'edit') {
            return {
              label: '编辑',
              handler: this.edit
            }
          }
        } else return e
      })
    }
  },
  mounted () {
    if (this.option.levitate !== false) {
      this.el.onscroll = this.scrollThrottle
    }
  },
  methods: {
    // 获取搜索栏位
    getSearchFields () {
      return this.$attrs.option.column.filter(e => {
        return e.search
      })
    },
    // 搜索栏位变动的回调
    handleSearchChange (obj) {
      this.$emit('searchChange', obj)
    },
    handleSizeChange (size) {
      this.$emit('sizeChange', size)
    },
    handleCurrentChange (current) {
      this.$emit('currentChange', current)
    },
    formatter (row, column, cellValue, index) {
      if (cellValue === '' || cellValue === null) {
        return '-'
      }
      return cellValue
    },
    getIndex (i) {
      return (this.page - 1) * this.pageSize + i + 1
    },
    handleScroll () {
      const clientHeight = document.documentElement.clientHeight
      const pageingBottom = document.querySelector('.table-pagination-area').getBoundingClientRect().bottom
      this.fixedPagination = clientHeight - pageingBottom < 30
    },
    initScroll () {
      // 手动触发一次scroll
      const event = document.createEvent('Event')
      event.initEvent('scroll', true, true)
      this.el.dispatchEvent(event)
    },
    // 内置按钮回调
    detail (row) {
      this.$emit('events', { event: 'detail', data: row })
    },
    delete (row) {
      this.$confirm('此操作无法撤销, 是否确定删除?', '确认要删除吗？', {
        customClass: 'delconfirm',
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.$emit('events', { event: 'delete', data: row })
      }).catch(() => {})
    },
    edit (row) {
      this.$emit('events', { event: 'edit', data: row })
    }
  }
}
</script>
<style lang="scss" scoped>
.xl-ly-table {
  display: flex;
  flex-direction: column;
  ::v-deep {
    .el-table {
      flex: 1;
    }
    .el-table td {
      padding: 15px 0;
    }
    .el-table thead th {
      line-height: 48px;
    }
    .el-table .cell {
      padding: 0 24px;
    }
    .el-pagination {
      text-align: right;
      margin-top: 10px;
    }
    .el-button--text{
      padding: 0;
      height: unset;
    }
  }
  .table-pagination-area {
    width: 100%;
    height: 60px;
    display: flex;
    justify-content: flex-end;
    transition: all 0.3s;
    .el-pagination {
      width: 100%;
      height: 60px;
      text-align: right;
      display: flex;
      left: 0;
      justify-content: flex-end;
      align-items: center;
      box-sizing: border-box;
    }
  }
  .fixedPagination {
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.12);
    position: fixed;
    bottom: 0;
    background: #fff;
    z-index: 99;
    // transition: all 0.2s;
    padding-right: 50px;
    li,
    button,
    input {
      background: #f5f7fa;
    }
  }
}
</style>
<style>
  .delconfirm{
    position: relative;
  }
  .delconfirm .el-message-box__header{
    padding-left: 60px;
  }
  .delconfirm .el-message-box__status{
    position: absolute;
    top: -28px;
  }
</style>
