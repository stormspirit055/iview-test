<template>
  <div>
    <!-- <button @click='isRequired = !isRequired'>sssss</button> -->
    <Form class='form-wrap' :model='formItem' >
      <FormItem style='width:350px;' required label='调试数据来源'  prop='debugDataSource'>
        <RadioGroup   @on-change='_handleSwitchDataSource'  v-model="formItem.debugDataSource">
            <Radio  label="extraUpload">
                <span>文件上传</span>
            </Radio>
            <Radio  label="sourceSampling">
                <span>原始数据采样</span>
            </Radio>
        </RadioGroup>
      </FormItem>
      <FormItem style='width:350px;' required  label='调试数据表结构'  prop='sss'  v-if='formItem.debugDataSource === "extraUpload"'>
      </FormItem>
      <FormItem style='width:350px;'  label='字段分隔符' >
        <Select style='width:350px;' placeholder="请选择字段分隔符或输入自定义分隔符" v-model="formItem.separator">
          <Option v-for='(item, index) in  separators' :key='index' :value="item.key">{{item.value}}</Option>
        </Select>
      </FormItem>
      <FormItem class='ivu-form-item-long' label='上传数据' required  v-if='formItem.debugDataSource === "extraUpload"'>
      </FormItem>
    </Form>
  </div>
</template>

<script>
export default {
    data() {
        return {
            isRequired: false,
            formItem: {
                debugDataSource: 'extraUpload',
                separator: ','
            },
            separators: [
                {key: ',', value: '逗号'},
            ]
        };
    },
    methods: {
        _handleSwitchDataSource() {
        }
    }
};
</script>

<style lang='less' scoped>
.form-wrap{
  width: 800px;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  margin-top: 32px;
  .ivu-form-item{
    width: 350px;
  }
  .ivu-form-item-long{
    width: 800px !important;
  }
  .add-btn{
    width: 1000px;
    border: 1px dashed #d5d5d8;
    outline: none;
    &:hover{
      border-color: var(--primary);
    }
  }
}
</style>