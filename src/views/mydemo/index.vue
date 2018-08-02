<template>
  <div class="tab-container">
    <el-tabs style='margin-top:15px;' v-model="activeName" type="border-card">
      <el-tab-pane label="用户管理" key='1' name="CN">
       <div>
         <div class="left fl">
           <p>鸿合科技</p>
           <ul class="nameList" v-for="item in lists">
             <li @click="liFun(item)">{{item}}</li>
           </ul>
         </div>
         <div class="right fl">
           <h3 class="title">{{cname}}</h3>
           <button class="addUsr" @click="handleCreate" >+新建用户</button>
           <br>
           <el-input style="width: 300px;" class="filter-item" placeholder="请输入账号或用户名进行查询">
           </el-input>
           <button class="addUsr">查询</button>
           <button class="addUsr">批量导入</button>
           <keep-alive>
             <tab-pane>
             </tab-pane>
           </keep-alive>
         </div>
       </div>
      </el-tab-pane>
      <el-tab-pane label="用户字段" key='2' name="US">
        <keep-alive>
          <tab-pane>
          </tab-pane>
        </keep-alive>
      </el-tab-pane>
    </el-tabs>
    <el-dialog :title="textMap[dialogStatus]" :visible.sync="dialogFormVisible">
      <el-form :rules="rules" ref="dataForm" :model="temp" label-position="left" label-width="70px" style='width: 400px; margin-left:50px;'>
        <el-form-item :label="$t('table.type')" prop="type">
          <el-select class="filter-item" v-model="temp.type" placeholder="Please select">
            <el-option v-for="item in  calendarTypeOptions" :key="item.key" :label="item.display_name" :value="item.key">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item :label="$t('table.date')" prop="timestamp">
          <el-date-picker v-model="temp.timestamp" type="datetime" placeholder="Please pick a date">
          </el-date-picker>
        </el-form-item>
        <el-form-item :label="$t('table.title')" prop="title">
          <el-input v-model="temp.title"></el-input>
        </el-form-item>
        <el-form-item :label="$t('table.status')">
          <el-select class="filter-item" v-model="temp.status" placeholder="Please select">
            <el-option v-for="item in  statusOptions" :key="item" :label="item" :value="item">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item :label="$t('table.importance')">
          <el-rate style="margin-top:8px;" v-model="temp.importance" :colors="['#99A9BF', '#F7BA2A', '#FF9900']" :max='3'></el-rate>
        </el-form-item>
        <el-form-item :label="$t('table.remark')">
          <el-input type="textarea" :autosize="{ minRows: 2, maxRows: 4}" placeholder="Please input" v-model="temp.remark">
          </el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">{{$t('table.cancel')}}</el-button>
        <el-button v-if="dialogStatus=='create'" type="primary" @click="createData">{{$t('table.confirm')}}</el-button>
        <el-button v-else type="primary" @click="updateData">{{$t('table.confirm')}}</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import tabPane from './components/tabPane'

export default {
  name: 'tab',
  components: { tabPane },
  data() {
    return {
      temp: {
        id: undefined,
        importance: 1,
        remark: '',
        timestamp: new Date(),
        title: '',
        type: '',
        status: 'published'
      },
      rules: {
        type: [{ required: true, message: 'type is required', trigger: 'change' }],
        timestamp: [{ type: 'date', required: true, message: 'timestamp is required', trigger: 'change' }],
        title: [{ required: true, message: 'title is required', trigger: 'blur' }]
      },
      textMap: {
        update: 'Edit',
        create: '添加新用户'
      },
      tabMapOptions: [
        { label: '用户管理', key: 'CN' },
        { label: '用户字段', key: 'US' }
      ],
      activeName: 'CN',
      createdTimes: 0,
      dialogStatus: '',
      dialogFormVisible: false,
      lists: ['鸿合科技-信息部', '鸿合科技-财务部', '鸿合科技-风险部', '鸿合科技-信息部', '鸿合科技-财务部', '鸿合科技-风险部'],
      cname: '鸿合科技'
    }
  },
  methods: {
    /* 点击li名称 */
    liFun(item) {
      this.cname = item
    },
    resetTemp() {
      this.temp = {
        id: undefined,
        importance: 1,
        remark: '',
        timestamp: new Date(),
        title: '',
        status: 'published',
        type: ''
      }
    },
    handleCreate() {
      this.resetTemp()
      this.dialogStatus = 'create'
      this.dialogFormVisible = true
      this.$nextTick(() => {
        this.$refs['dataForm'].clearValidate()
      })
    },
    showCreatedTimes() {
      this.createdTimes = this.createdTimes + 1
    }
  }
}
</script>

<style scoped>
  *{
    padding: 0;
    color: #777;
  }
  .tab-container{
    margin: 30px;
    font-family: '微软雅黑';
  }
  .fl{
    float: left;
  }
  .left{
    width: 20%;
    height: 100vh;
    padding: 0 20px;
    font-size: 14px;
  }
  .right{
    width: 80%;
    height: 100vh;
    border-left: 1px solid #eee;
    padding: 0 20px;
  }
  .title{
    font-size: 16px;
  }
  .nameList li{
    list-style: none;
    height: 30px;line-height: 30px;
    border-radius: 4px;
    padding:0 8px;cursor: pointer;
  }
  .nameList li:hover{
    background: #eee;
  }
  .addUsr{
    padding: 8px 10px;border: 1px solid #ccc;background: #fff;border-radius: 4px; outline: medium;margin-bottom: 10px;
  }
  .addUsr:hover{
    box-shadow: 2px 3px 6px #eee;cursor: pointer;
  }
</style>
