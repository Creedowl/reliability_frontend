<template>
  <div id="source">
    <Tabs value="file">
      <TabPane label="直接输入" name="input">
        <Input
          v-model="text"
          type="textarea"
          :rows="4"
          placeholder="请输入失效数据"
        />
        <Button v-if="text" type="primary" @click="uploadText()">
          点击提交
        </Button>
      </TabPane>
      <TabPane label="文件上传" name="file">
        <p>请上传txt, csv或xls文件</p>
        <Upload
          action="/"
          type="drag"
          :before-upload="handleUpload"
          :accept="'.txt,.csv,.xls,.xlsx'"
          :format="['txt', 'csv', 'xls', 'xlsx']"
        >
          <div style="padding: 20px 0">
            <Icon
              type="ios-cloud-upload"
              size="52"
              style="color: #3399ff"
            ></Icon>
            <p>点击或拖拽文件</p>
          </div>
        </Upload>
        <Button v-if="file" type="primary" @click="upload()">点击上传</Button>
      </TabPane>
      <TabPane label="数据库" name="database">
        <div id="db">
          <div id="form">
            <Form :model="db" :label-width="100">
              <FormItem label="数据库类型">
                <Select v-model="db.type">
                  <Option value="MYSQL">Mysql</Option>
                  <Option value="POSTGRES">PostgreSql</Option>
                </Select>
              </FormItem>
              <FormItem label="地址">
                <Input v-model="db.host"></Input>
              </FormItem>
              <FormItem label="端口">
                <Input v-model="db.port"></Input>
              </FormItem>
              <FormItem label="数据库名">
                <Input v-model="db.database"></Input>
              </FormItem>
              <FormItem label="表名">
                <Input v-model="db.table"></Input>
              </FormItem>
              <FormItem label="用户名">
                <Input v-model="db.username"></Input>
              </FormItem>
              <FormItem label="密码">
                <Input v-model="db.password" type="password"></Input>
              </FormItem>
              <FormItem>
                <Button type="primary" @click="connect()">连接</Button>
              </FormItem>
            </Form>
          </div>
        </div>
      </TabPane>
    </Tabs>
    <Table v-if="data.length > 0" :columns="col" :data="data"></Table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      file: null,
      uuid: '',
      text: '',
      db: {
        type: '',
        host: '',
        port: '',
        database: '',
        table: '',
        username: '',
        password: '',
      },
      col: [
        { title: '失效次数', key: 'times' },
        { title: '失效间隔', key: 'gaps' },
      ],
      data: [],
    }
  },
  methods: {
    handleUpload(file) {
      // this.file = file
      const form = new FormData()
      // const reader = new FileReader()
      form.append('file', file)
      this.file = form
      // reader.readAsDataURL(file)
      // reader.onload = () => {
      //   this.img = reader.result
      // }
      return false
    },
    async upload() {
      try {
        const res = await this.$api.upload(this.file)
        this.uuid = res.uuid
        this.data = res.data
        this.$store.commit('updateUUID', res.uuid)
        this.$Message.success('上传成功')
      } catch (error) {
        this.$Message.error('上传失败' + error.response.data.message)
      }
    },
    async uploadText() {
      try {
        const res = await this.$api.uploadText({ text: this.text })
        this.uuid = res.uuid
        this.data = res.data
        this.$store.commit('updateUUID', res.uuid)
        this.$Message.success('提交成功')
      } catch (error) {
        this.$Message.error('提交失败' + error.response.data.message)
      }
    },
    async connect() {
      try {
        const res = await this.$api.fromDB(this.db)
        this.uuid = res.uuid
        this.data = res.data
        this.$store.commit('updateUUID', res.uuid)
        this.$Message.success('导入成功')
      } catch (error) {
        this.$Message.error('导入失败' + error.response.data.message)
      }
    },
  },
}
</script>

<style scoped>
#source {
  /* display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center; */
}

#db {
  display: flex;
  justify-content: center;
  justify-items: center;
}

#form {
  width: 50%;
}
</style>
