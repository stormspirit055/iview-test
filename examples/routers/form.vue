<template>
  <div>
    <Form class='form-wrap'>
        <RadioGroup  v-model="rswitch">
            <Radio  label="open">
                <span>展示</span>
            </Radio>
            <Radio  label="close">
                <span>不展示</span>
            </Radio>
        </RadioGroup>
      <FormItem style='width:350px;' required  label='a'  prop='a' v-if='rswitch === "open"'>
      </FormItem>
      <FormItem style='width:350px;'  label='b' >
      </FormItem>
      <FormItem class='ivu-form-item-long' label='c' prop='b'  v-if='rswitch === "open"'>
      </FormItem>
    </Form>
  </div>
</template>

<script>
export default {
    data() {
        return {
            rswitch: 'open',
        };
    },
    methods: {
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
}
</style>