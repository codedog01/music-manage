<template>
  <div class="sidebar">
    <el-menu
      class="sidebar-el-menu"
      :default-active="onRoutes"
      :collapse="collapse"
      text-color="#FFF"
      active-text-color="#F40"
      unique-opened
      router
      style="  background-color: rgba(255,255,255,.1);"
    >
      <template v-for="item in items">
          <template>
            <el-menu-item :index="item.index" :key="item.index">
              <i :class="item.icon"></i>
              <span slot="title">{{ item.title }}</span>
            </el-menu-item>
        </template>
      </template>
    </el-menu>
  </div>
</template>

<script>
import _ctrEvent from '../assets/js/ctr-event'

export default {
  data () {
    return {
      collapse: false,
      items: [
        {
          icon: 'el-icon-s-data',
          index: 'info',
          title: '系统首页'
        },
        {
          icon: 'el-icon-user-solid',
          index: 'consumer',
          title: '用户管理'
        },
        {
          icon: 'el-icon-service',
          index: 'singer',
          title: '歌手管理'
        },
        {
          icon: 'el-icon-document',
          index: 'songList',
          title: '歌单管理'
        }
      ]
    }
  },
  computed: {
    onRoutes () {
      return this.$route.path.replace('/', '')
    }
  },
  created () {
    _ctrEvent.$on('collapse', msg => {
      this.collapse = msg
    })
  }
}
</script>

<style scoped>
.sidebar {
  display: block;
  position: absolute;
  background-color: rgba(255,255,255,.1);
  box-shadow: 0px 0px 8px 2px rgba(0, 0, 0, 0.3);
  left: 0;
  top: 70px;
  bottom: 0;
  overflow-y: scroll;
}
.sidebar::-webkit-scrollbar {
  width: 0;
}
.sidebar-el-menu:not(.el-menu--collapse) {
  width: 150px;
}
.sidebar > ul {
  height: 100%;
}
</style>
