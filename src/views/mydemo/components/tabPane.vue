<template>
  <el-table :data="list" border fit highlight-current-row style="width: 100%">

    <el-table-column align="center" label="账号" width="165px"  v-loading="loading"
    element-loading-text="请给我点时间！">
      <template slot-scope="scope">
        <span>{{scope.row.id}}</span>
      </template>
    </el-table-column>

    <el-table-column width="180px" align="center" label="	姓名">
      <template slot-scope="scope">
        <span>{{scope.row.timestamp | parseTime('{y}-{m}-{d} {h}:{i}')}}</span>
      </template>
    </el-table-column>

    <el-table-column min-width="150px" label="分部">
      <template slot-scope="scope">
        <span>{{scope.row.title}}</span>
        <el-tag>{{scope.row.type}}</el-tag>
      </template>
    </el-table-column>

    <el-table-column width="160px" align="center" label="	部门">
      <template slot-scope="scope">
        <span>{{scope.row.author}}</span>
      </template>
    </el-table-column>

    <el-table-column width="120px" label="	锁定状态">
      <template slot-scope="scope">
        <svg-icon v-for="n in +scope.row.importance" icon-class="star" :key="n"></svg-icon>
      </template>
    </el-table-column>

    <el-table-column align="center" label="	启用状态" width="95">
      <template slot-scope="scope">
        <span>{{scope.row.pageviews}}</span>
      </template>
    </el-table-column>

    <el-table-column class-name="status-col" label="	安全级别" width="110">
      <template slot-scope="scope">
        <el-tag :type="scope.row.status | statusFilter">{{scope.row.status}}</el-tag>
      </template>
    </el-table-column>

  </el-table>
</template>

<script>
import { fetchList } from '@/api/article'

export default {
  props: {
    type: {
      type: String,
      default: 'CN'
    }
  },
  data() {
    return {
      list: null,
      listQuery: {
        page: 1,
        limit: 5,
        type: this.type,
        sort: '+id'
      },
      loading: false
    }
  },
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: 'success',
        draft: 'info',
        deleted: 'danger'
      }
      return statusMap[status]
    }
  },
  created() {
    this.getList()
  },
  methods: {
    getList() {
      this.loading = true
      this.$emit('create') // for test
      fetchList(this.listQuery).then(response => {
        this.list = response.data.items
        this.loading = false
      })
    }
  }
}
</script>

