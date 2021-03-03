<template>
  <Header>
    <Menu mode="horizontal" theme="dark" class="header_menu">
      <div class="logo">
        <span>Reliability</span>
      </div>
      <div class="header-nav">
        <MenuItem name="home" to="/">
          <Icon type="ios-home-outline" size="20" custom="icon" />
          <span class="nav_text">首页</span>
        </MenuItem>
        <template v-if="!isAuthenticated">
          <Button
            shape="circle"
            icon="ios-contact"
            @click="$router.push('/login')"
          >
            <span>登录</span>
          </Button>
          <Button
            shape="circle"
            icon="md-person-add"
            @click="$router.push('/register')"
          >
            <span>注册</span>
          </Button>
        </template>
        <template v-else>
          <MenuItem name="model" to="/model">
            <Icon type="ios-flask-outline" size="20" custom="icon" />
            <span class="nav_text">模型</span>
          </MenuItem>
          <Submenu name="user">
            <template slot="title">
              <Icon type="ios-person-outline" size="20" custom="icon" />
              <span class="nav_text">{{ user.username }}</span>
            </template>
            <MenuGroup title="用户">
              <MenuItem name="logout" @click.native="logout()">退出</MenuItem>
            </MenuGroup>
          </Submenu>
        </template>
      </div>
    </Menu>
  </Header>
</template>

<script>
export default {
  computed: {
    isAuthenticated() {
      return this.$auth.loggedIn
    },
    user() {
      return this.$auth.user
    },
  },
  methods: {
    logout() {
      this.$auth.logout()
    },
  },
}
</script>

<style scoped>
.icon {
  font-weight: bold;
}

.header_menu {
  height: 64px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin: 0 10%;
  z-index: auto;
}

.logo {
  font-size: 2rem;
  color: white;
}
</style>
