<template>
  <div class="environment-list">
    <el-row>
      <div class="button-group">
        <el-button size="small" type="primary" icon="el-icon-plus" @click="addEnv">{{ $t('Add Environment Variables') }}</el-button>
        <el-button size="small" type="success" @click="save">{{ $t('Save') }}</el-button>
      </div>
    </el-row>
    <el-row>
      <el-table :data="spiderForm.envs">
        <el-table-column :label="$t('Variable')">
          <template slot-scope="scope">
            <el-input v-model="scope.row.name" :placeholder="$t('Variable')" />
          </template>
        </el-table-column>
        <el-table-column :label="$t('Value')">
          <template slot-scope="scope">
            <el-input v-model="scope.row.value" :placeholder="$t('Value')" />
          </template>
        </el-table-column>
        <el-table-column :label="$t('Action')">
          <template slot-scope="scope">
            <el-button size="mini" icon="el-icon-delete" type="danger" @click="deleteEnv(scope.$index)" />
          </template>
        </el-table-column>
      </el-table>
    </el-row>
  </div>
</template>

<script>
  import {
    mapState
  } from 'vuex'

  export default {
    name: 'EnvironmentList',
    computed: {
      ...mapState('spider', [
        'spiderForm'
      ])
    },
    methods: {
      addEnv() {
        if (!this.spiderForm.envs) {
          this.$set(this.spiderForm, 'envs', [])
        }
        this.spiderForm.envs.push({
          name: '',
          value: ''
        })
        this.$st.sendEv('爬虫详情', '环境', '添加')
      },
      deleteEnv(index) {
        this.spiderForm.envs.splice(index, 1)
        this.$st.sendEv('爬虫详情', '环境', '删除')
      },
      save() {
        this.$store.dispatch('spider/editSpider')
          .then(() => {
            this.$message.success(this.$t('Spider info has been saved successfully'))
          })
          .catch(error => {
            this.$message.error(error)
          })
        this.$st.sendEv('爬虫详情', '环境', '保存')
      }
    }
  }
</script>

<style scoped>
  .button-group {
    width: 100%;
    text-align: right;
  }

  .el-table {
    min-height: 360px;
  }
</style>
