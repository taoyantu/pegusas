<template>
  <el-row class="panel">
    <el-col :span="24">
      <div class="List-title" style="margin: 10px">
        <div class="List-titleText">参数配置</div>
      </div>
    </el-col>
    <el-col :span="24">
      <el-col :span="12">
        <el-input placeholder="请输入" v-model="filter.name" @keyup.enter.native="onSearchChange({search: filter.name})"
                  @clear="onSearchChange({search: filter.name})" :clearable="true">
          <el-button slot="append" @click="onSearchChange({search: filter.name})">
            <i class="fa fa-search"></i>
          </el-button>
        </el-input>
      </el-col>
      <div style="display: flex;justify-content: flex-end">
        <router-link :to="{ name: 'createSystem'}">
          <el-button type="primary">新建</el-button>
        </router-link>
      </div>
    </el-col>
    <el-col :span="24" class="list">
      <el-table :data="systems.results">
        <el-table-column prop="name" label="参数名称" min-width="100" show-overflow-tooltip></el-table-column>
        <el-table-column prop="description" label="参数描述" min-width="100"></el-table-column>
        <el-table-column prop="updateBy" label="修改人" min-width="100"></el-table-column>
        <el-table-column prop="updateTime" label="修改时间" min-width="100"></el-table-column>
        <el-table-column label="操作" min-width="100" align="right">
          <template slot-scope="scope">
            <el-tooltip content="编辑" placement="top">
              <button class="List-actionButton" @click="viewSysParams(scope.row, 'update')">
                <i class="fa fa-pencil"></i>
              </button>
            </el-tooltip>
            <el-tooltip content="查看" placement="top">
              <button class="List-actionButton" @click="viewSysParams(scope.row, 'view')">
                <i class="fa fa-search-plus"></i>
              </button>
            </el-tooltip>
          </template>
        </el-table-column>
      </el-table>
    </el-col>
    <!--工具条-->
    <el-col :span="24" style="display: flex; justify-content: flex-end">
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="pagination.page"
        :page-sizes="[10, 20, 50, 100]"
        :page-size="pagination.page_size"
        layout="sizes, prev, pager, next"
        :total="systems.count">
      </el-pagination>
    </el-col>
  </el-row>
</template>
<script>
import { mapState, mapActions } from 'vuex'
import moment from 'moment'

export default {
  name: 'systems',
  components: {},
  computed: {
    ...mapState({
      systems: state => state.system.systems
    })
  },
  data () {
    return {
      pagination: {
        page: 1,
        page_size: 10
      },
      filter: {
        name: ''
      },
      baseUrl: '/api/v1/system/'
    }
  },
  mounted: function () {
    this.querySystems()
  },
  methods: {
    moment: moment,
    ...mapActions([
      'fetchSystems'
    ]),
    querySystems () {
      console.log('###QUERY_SYSTEMS###', this.baseUrl, this.pagination)
      this.fetchSystems({ url: this.baseUrl, params: this.pagination })
    },
    handleSizeChange (val) {
      this.pagination = { ...this.pagination, page: 1, page_size: val }
      this.querySystems()
    },
    handleCurrentChange (val) {
      this.pagination = { ...this.pagination, page: val }
      this.querySystems()
    },
    changeConfig (type) {
      if (type !== this.type) {
        this.$router.push({ name: type })
      }
    }
  }
}
</script>
