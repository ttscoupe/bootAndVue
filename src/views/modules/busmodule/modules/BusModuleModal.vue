<template>
  <a-modal
    :title="title"
    :width="width"
    :visible="visible"
    :confirmLoading="confirmLoading"
    @ok="handleOk"
    @cancel="handleCancel"
    cancelText="关闭">
    <a-spin :spinning="confirmLoading">
      <a-form :form="form">

        <!--<a-form-item label="创建日期" :labelCol="labelCol" :wrapperCol="wrapperCol">-->
          <!--<j-date placeholder="请选择创建日期" v-decorator="[ 'createTime', validatorRules.createTime]" :trigger-change="true" :show-time="true" date-format="YYYY-MM-DD HH:mm:ss" style="width: 100%"/>-->
        <!--</a-form-item>-->
          
        <a-form-item label="模型名称" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'modName', validatorRules.modName]" placeholder="请输入模型名称"></a-input>
        </a-form-item>
          
        <a-form-item label="模型分类" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <j-dict-select-tag type="list" v-decorator="['modType']" :trigger-change="true" dictCode="mod_type" placeholder="请选择模型分类"/>
        </a-form-item>
          
        <a-form-item label="预览图" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <j-upload v-decorator="['modPic']" :trigger-change="true"></j-upload>
        </a-form-item>
          
        <!--<a-form-item label="分享地址" :labelCol="labelCol" :wrapperCol="wrapperCol">-->
          <!--<a-input v-decorator="[ 'modFxurl', validatorRules.modFxurl]" placeholder="请输入分享地址"></a-input>-->
        <!--</a-form-item>-->
          <!---->
        <!--<a-form-item label="分享二维码" :labelCol="labelCol" :wrapperCol="wrapperCol">-->
          <!--<a-input v-decorator="[ 'modFxscan', validatorRules.modFxscan]" placeholder="请输入分享二维码"></a-input>-->
        <!--</a-form-item>-->
          
        <a-form-item label="模型" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <j-upload v-decorator="['modObj']" :trigger-change="true"></j-upload>
        </a-form-item>
          
        
      </a-form>
    </a-spin>
  </a-modal>
</template>

<script>

  import { httpAction } from '@/api/manage'
  import pick from 'lodash.pick'
  import JDate from '@/components/jeecg/JDate'  
  import JUpload from '@/components/jeecg/JUpload'
  import JDictSelectTag from "@/components/dict/JDictSelectTag"
  
  export default {
    name: "BusModuleModal",
    components: { 
      JDate,
      JUpload,
      JDictSelectTag,
    },
    data () {
      return {
        form: this.$form.createForm(this),
        title:"操作",
        width:800,
        visible: false,
        model: {},
        labelCol: {
          xs: { span: 24 },
          sm: { span: 5 },
        },
        wrapperCol: {
          xs: { span: 24 },
          sm: { span: 16 },
        },

        confirmLoading: false,
        validatorRules:{
        createTime:{},
        modName:{},
        modType:{},
        modPic:{},
        modFxurl:{},
        modFxscan:{},
        modObj:{},
        },
        url: {
          add: "/busmodule/busModule/add",
          edit: "/busmodule/busModule/edit",
        }
     
      }
    },
    created () {
    },
    methods: {
      add () {
        this.edit({});
      },
      edit (record) {
        this.form.resetFields();
        this.model = Object.assign({}, record);
        this.visible = true;
        this.$nextTick(() => {
          this.form.setFieldsValue(pick(this.model,'createTime','modName','modType','modPic','modFxurl','modFxscan','modObj'))
        })
      },
      close () {
        this.$emit('close');
        this.visible = false;
      },
      handleOk () {
        const that = this;
        // 触发表单验证
        this.form.validateFields((err, values) => {
          if (!err) {
            that.confirmLoading = true;
            let httpurl = '';
            let method = '';
            if(!this.model.id){
              httpurl+=this.url.add;
              method = 'post';
            }else{
              httpurl+=this.url.edit;
               method = 'put';
            }
            let formData = Object.assign(this.model, values);
            console.log("表单提交数据",formData)
            httpAction(httpurl,formData,method).then((res)=>{
              if(res.success){
                that.$message.success(res.message);
                that.$emit('ok');
              }else{
                that.$message.warning(res.message);
              }
            }).finally(() => {
              that.confirmLoading = false;
              that.close();
            })
          }
         
        })
      },
      handleCancel () {
        this.close()
      },
      popupCallback(row){
        this.form.setFieldsValue(pick(row,'createTime','modName','modType','modPic','modFxurl','modFxscan','modObj'))
      }
      
    }
  }
</script>