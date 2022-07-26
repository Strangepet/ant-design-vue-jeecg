<template>
  <fm-making-form 
    ref="makingform" 
    style="height: 600px;" 
    preview 
    generate-code 
    generate-json
    :global-config="globalConfig"
  >
    <template slot="action">
        <!-- 头部内容自定义，添加提交按钮 -->
        
    </template>
  </fm-making-form>
</template>

<script>
    export default {
        data(){
            return{
              formName:'',
              globalConfig: {
                // 可以将表单预设的样式放入到设计器中
                styleSheets: '.a .el-form-item__label{color: red;}'
              },
            }
        },
        components:{  },
        methods:{
            handleSubmit () {
                const json = this.$refs.makingform.getJSON();
                console.log(json) //这个就是生成的表单数据，可以直接给后台或者直接拿过来生成表单
            }
        },
        created() {
            
        }
    }
</script>
