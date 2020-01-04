<template>
  <a-card :bordered="false">
    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">
          <a-col :md="5" :sm="6">
            <a-form-item label="模型名称">
              <a-input placeholder="请输入模型名称" v-model="queryParam.modName"></a-input>
            </a-form-item>
          </a-col>
          <a-col :md="4" :sm="4">
            <a-form-item label="模型分类">
              <j-dict-select-tag placeholder="请选择" v-model="queryParam.modType" dictCode="mod_type"/>
            </a-form-item>
          </a-col>
          <a-col :md="8" :sm="8">
            <a-form-item label="创建日期">
              <j-date placeholder="开始日期" class="query-group-cust" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss"  v-model="queryParam.createTime_begin"></j-date>
              <span class="query-group-split-cust"></span>
              <j-date placeholder="结束日期" class="query-group-cust" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss"  v-model="queryParam.createTime_end"></j-date>
            </a-form-item>
          </a-col>
          <a-col :md="6" :sm="8">
            <span style="float: left;overflow: hidden;" class="table-page-search-submitButtons">
              <a-button type="primary" @click="searchQuery" icon="search">查询</a-button>
              <a-button type="primary" @click="searchReset" icon="reload" style="margin-left: 8px">重置</a-button>
            </span>
          </a-col>
        </a-row>
      </a-form>
    </div>
    <!-- 查询区域-END -->

    <div class="ant-alert ant-alert-info" style="margin-bottom: 16px;">
      <i class="anticon anticon-info-circle ant-alert-icon"></i>
    </div>
    <div class="card-list" ref="content">
      <a-list :grid="{gutter: 24, lg: 4, md: 2, sm: 1, xs: 1}" :dataSource="dataSource">
        <a-list-item slot="renderItem" slot-scope="item, index">
          <a-card :hoverable="true">
            <a-card-meta>
              <div style="margin-bottom: 3px;margin-left:20px;" slot="title">{{ item.modName }}</div>
              <div style="margin-bottom: 10px;margin-left:20px;" slot="description">分类 {{ item.modType }}</div>
              <a-avatar class="card-avatar" slot="avatar" :src="'http://127.0.0.1:8080/ttscoupe-boot/sys/common/view/{{item.modPic}}'" size="large"/>
              <div style="margin-left:20px;" slot="description">{{ item.createTime}}</div>
            </a-card-meta>
            <template class="ant-card-actions" slot="actions">
              <a>分享二维码</a>
              <a>分享链接</a>
            </template>
          </a-card>
        </a-list-item>
      </a-list>
    </div>
    <busModule-modal ref="modalForm" @ok="modalFormOk"></busModule-modal>
  </a-card>
</template>

<script>

  import { JeecgListMixin } from '@/mixins/JeecgListMixin'
  import {initDictOptions, filterMultiDictText} from '@/components/dict/JDictSelectUtil'
  import JDate from '@/components/jeecg/JDate.vue'

  export default {
    name: "CardModuleListccc",
    mixins:[JeecgListMixin],
    components: {
      JDate
    },
    data () {
      return {
        description: '模型表管理页面',
        // 表头
        columns: [],

        url: {
          list: "/busmodule/busModule/list",
        },
        dictOptions:{
          modType:[],
        },
      }

    },

    methods: {
      initDictConfig(){
        initDictOptions('mod_type').then((res) => {
          if (res.success) {
            this.$set(this.dictOptions, 'modType', res.result)
          }
        })
      }
    }
  }
</script>
<style lang="scss" scoped>
  .card-avatar {
    width: 120px;
    height: 120px;
    border-radius: 38px;
  }

  .ant-card-actions {
    background: #f7f9fa;
    li {
      float: left;
      text-align: center;
      margin: 12px 0;
      color: rgba(0, 0, 0, 0.45);
      width: 50%;

      &:not(:last-child) {
        border-right: 1px solid #e8e8e8;
      }

      a {
        color: rgba(0, 0, 0, .45);
        line-height: 22px;
        display: inline-block;
        width: 100%;
        &:hover {
          color: #1890ff;
        }
      }
    }
  }

  .new-btn {
    background-color: #fff;
    border-radius: 2px;
    width: 100%;
    height: 188px;
  }

  .meta-content {
    position: relative;
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    height: 64px;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
  }
</style>