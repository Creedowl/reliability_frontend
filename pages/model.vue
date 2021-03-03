<template>
  <div class="model">
    <Card class="card">
      <p slot="title">
        <Icon type="ios-bulb-outline" />
        <span>模型处理</span>
      </p>
      <Steps :current="current">
        <Step title="选择数据" content="上传数据文件或连接数据库"></Step>
        <Step title="选择模型" content="选择对应的可靠性模型"></Step>
        <!-- <Step title="设置参数" content="设置所选模型的参数"></Step>
        <Step title="查看结果" content="查看模型输出"></Step> -->
      </Steps>
      <Divider />
      <div id="action">
        <DataSource v-if="current === 0"></DataSource>
        <ModelList v-if="current === 1"></ModelList>
        <!-- <Params v-if="current === 2"></Params>
        <Result v-if="current === 3"></Result> -->
      </div>
      <Divider />
      <div id="controller">
        <Button type="primary" :disabled="previous" @click="current--">
          上一步
        </Button>
        <Button type="primary" :disabled="next" @click="current++">
          下一步
        </Button>
      </div>
    </Card>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import DataSource from '~/components/DataSource'
import ModelList from '~/components/ModelList'
// import Params from '~/components/Params'
// import Result from '~/components/Result'

export default {
  components: {
    DataSource,
    ModelList,
    // Params,
    // Result,
  },
  data() {
    return {
      current: 0,
    }
  },
  computed: {
    ...mapGetters({
      uuid: 'dataUUID',
    }),
    previous() {
      return this.current <= 0
    },
    next() {
      return this.current > 0 || !this.nextPage
    },
    nextPage() {
      return this.uuid
    },
  },
}
</script>

<style scoped>
.model {
  width: 80vw;
}

#controller {
  text-align: right;
}
</style>
