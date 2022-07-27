<template>
  <div class="form-config-container">
    <el-form label-position="top">
      <el-form-item :label="$t('fm.config.form.labelPosition.title')">
        <el-radio-group v-model="data.labelPosition">
          <el-radio-button label="left">{{$t('fm.config.form.labelPosition.left')}}</el-radio-button>
          <el-radio-button label="right">{{$t('fm.config.form.labelPosition.right')}}</el-radio-button>
          <el-radio-button label="top">{{$t('fm.config.form.labelPosition.top')}}</el-radio-button>
        </el-radio-group>
      </el-form-item>

      <el-form-item :label="$t('fm.config.form.labelWidth')">
        <el-input-number v-model="data.labelWidth" :min="0" :max="200" :step="10"></el-input-number>
      </el-form-item>

      <el-form-item :label="$t('fm.config.form.size')">
        <el-radio-group v-model="data.size">
          <el-radio-button label="medium">medium</el-radio-button>
          <el-radio-button label="small">small</el-radio-button>
          <el-radio-button label="mini">mini</el-radio-button>
        </el-radio-group>
      </el-form-item>

          <!-- 新增表单css样式表选项 -->
      <el-form-item :label="'css样式自定义'" >
        <!-- 抽屉 -->
       <el-button type="primary" @click="dialogVisible = true" style="width:100%">设置样式</el-button>
        <el-dialog
          title="请在此处填写css样式"
          :visible.sync="dialogVisible"
          width="40%"
          :before-close="handleClose">
          <!-- 对话框内容 -->
          <el-input
            type="textarea"
            :autosize="{ minRows: 6, maxRows: 15}"
            placeholder="请输入内容"
            v-model="cssTextarea">
          </el-input>
          
          <span slot="footer" class="dialog-footer">
            <el-button @click="dialogVisible = false">取 消</el-button>
            <el-button type="primary" @click="setcss" >确 定</el-button>
          </span>
        </el-dialog>
      </el-form-item>

      <el-form-item :label="'自定义class'" >
          <el-select v-model="cssSelectValue" multiple placeholder="请选择" style="width:100%">
            <el-option
              v-for="item in classOptions"
              :key="item.cssClassName"
              :label="item.label"
              :value="item.cssClassName">
            </el-option>
          </el-select>
      </el-form-item>
    </el-form>


  </div>
</template>

<script>
export default {
  props: ['data'],
  data(){
    return{
      dialogVisible: false,
      cssTextarea:'.testcss1{color:red;backgroundcolor:pink} .testcss2{color:yellow}',
      // cssTextarea:'.a{34} .b{1012}',
      classOptions: [
        // {
        //   cssClassName: '选项1',
        //   // label: '黄金糕'
        //   cssContaint:'color:red'
        // }, {
        //   cssClassName: '选项2',
        //   // label: '双皮奶'
        //   cssContaint:'color:yellow'
        // }
        ],
        cssSelectValue: []
    }
  },
   methods: {
      handleClose(done) {
        this.$confirm('确认关闭？')
          .then(_ => {
            done();
          })
          .catch(_ => {});
      },
      setcss(){
        console.log('已设置的样式为',this.cssTextarea)
        // 解析用户填写的样式
        if(this.cssTextarea){
          let classNameArr=[]//类名数组
          let classContainArr=[]//类内容数组
          let classNameIndexArr=[]//类名索引数组
          let classContainIndexArr=[]//类内容索引数组
          let classIndexArr=[]//完整类索引数组
          let str=this.cssTextarea
          for(let i=0;i<str.length;i++){
            if(str[i]==='.'){
              classNameIndexArr.push(i)
              classIndexArr.push(i)
            }
            if(str[i]==='{'){
              classNameIndexArr.push(i)
              classContainIndexArr.push(i)
              classIndexArr.push(i)
            }
            if(str[i]==='}'){
              classContainIndexArr.push(i)
              classIndexArr.push(i)
            }
          }
          console.log('类名索引数组',classNameIndexArr)
          console.log('类内容索引数组',classContainIndexArr)
          for(let i=0;i< classNameIndexArr.length;i++){
            classNameArr.push(str.substring(classNameIndexArr[i]+1,classNameIndexArr[i+1]))
            i=i+1
          }
          for(let i=0;i< classContainIndexArr.length;i++){
            classContainArr.push(str.substring(classContainIndexArr[i]+1,classContainIndexArr[i+1]))
            i=i+1
          }
          console.log('类名数组',classNameArr,'类内容',classContainArr)
          // 把类名填到下拉框
          this.classOptions=[]
          for(let i=0,j=0;i< classNameArr.length,j<classContainArr.length;i++,j++){
            let tempClassObj={}
            tempClassObj['cssClassName']=classNameArr[i]
            tempClassObj['cssContaint']=classContainArr[j]
            this.classOptions.push(tempClassObj)
          }
          console.log('选项有',this.classOptions)
        }
        this.dialogVisible = false
        
      }
    }
}
</script>
