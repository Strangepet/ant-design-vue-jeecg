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
          width="70%"
          :before-close="handleClose">
          <!-- 对话框内容 -->
          <!-- 代码编辑框 -->
          <template>
            <div>
              <j-code-editor
                language="css"
                v-model="cssTextarea"
                :fullScreen="true"
                style="min-height: 100px"/>
              <!-- {{ editorValue }} -->
            </div>
          </template>
          
          <span slot="footer" class="dialog-footer">
            <el-button @click="dialogVisible = false">取 消</el-button>
            <el-button type="primary" @click="setcss" >确 定</el-button>
          </span>
        </el-dialog>
      </el-form-item>

      <el-form-item :label="'自定义class'" >
          <el-select v-model="cssSelectValue" multiple placeholder="请选择" style="width:100%" @change="transCss">
            <el-option
              v-for="item in classOptions"
              :key="item.cssClassName"
              :label="item.cssClassName"
              :value="item.cssContaint">
            </el-option>
          </el-select>
      </el-form-item>
    </el-form>


  </div>
</template>

<script>
import bus from './eventBus'
import JCodeEditor from '@/components/jeecg/JCodeEditor'
export default {
  components: {JCodeEditor},
  props: ['data'],
  data(){
    return{
      dialogVisible: false,
      //用户输入的代码文本
      // cssTextarea:'',
      cssTextarea:'.testcss1{color:red;background:pink} .testcss2{color:yellow}',
      classOptions: [],
      cssSelectValue: []//被选中的下拉框数组
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
        console.log('已填入的样式为',this.cssTextarea)
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
          // 把解析后的类名类内容传给字段设置里面的下拉框组件
          bus.$emit('transClassOptions',this.classOptions)
        }
        //关闭对话框
        this.dialogVisible = false
      },

      //下拉框选项更改时把数据传输给widgetform组件
      transCss(){
      // 把下拉框的cssSelectValue变量通过transCssValue这个自定义事件共享给兄弟组件widgetform
        bus.$emit('transCssValue',this.cssSelectValue)
        bus.$emit('transCssValue2',this.cssSelectValue)
      }

    }
}
</script>
