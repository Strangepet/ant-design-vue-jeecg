<template>
  <fm-making-form 
    ref="makingform" 
    style="height: 600px;" 
    preview 
    generate-code 
    generate-json
  >
    <template slot="action">
    </template>
  </fm-making-form>
</template>

<script>
    export default {
        data(){
            return{
              formName:'',
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
