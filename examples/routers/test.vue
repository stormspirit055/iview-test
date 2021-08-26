<template>
<div>
  <FormItem required label='调试数据来源'  prop='debugDataSource'>
        <RadioGroup   @on-change='_handleSwitchDataSource'  v-model="formItem.debugDataSource">
            <Radio  label="extraUpload">
                <span>文件上传</span>
            </Radio>
            <Radio  label="sourceSampling">
                <span>原始数据采样</span>
            </Radio>
        </RadioGroup>
      </FormItem>
      <FormItem  required label='调试数据表结构'  prop='hasHeader' v-if='formItem.debugDataSource === "extraUpload"'>
      </FormItem>
      <FormItem label='字段分隔符' prop='separator'>
        <Select   placeholder="请选择字段分隔符或输入自定义分隔符" v-model="formItem.separator">
          <Option v-for='(item, index) in  separators' :key='index' :value="item.key">{{item.value}}</Option>
        </Select>
      </FormItem>
      <FormItem class='ivu-form-item-long' v-if='formItem.debugDataSource === "extraUpload"'>
      </FormItem>
      <FormItem  v-if='formItem.debugDataSource === "extraUpload"' label='调试数据表'  class='ivu-form-item-long'>
      </FormItem>
</div>
  
</template>

<script>
export default {

}
</script>

<style>

</style>