<!-- 导航栏 -->

<template>
  <div class="navbar">
    <Hamburger :is-active="sidebar.opened" class="hamburger" @toggle-click="state.toggleSideBar" />
    <BreadCrumb class="breadcrumb" />
    <div class="right-menu">
      <Screenfull class="right-menu-item" v-if="showScreenfull" />
      <ThemeSwitch class="right-menu-item" v-if="showThemeSwitch" />
      <el-dropdown class="avatar-container right-menu-item" trigger="click">
        <div class="avatar-wrapper">
          <img src="@/assets/layout/avatar.gif" class="user-avatar">
        </div>
        <template #dropdown>
          <el-dropdown-menu>
            <a target="_blank" href="https://github.com/un-pany/v3-admin">
              <el-dropdown-item>基于 v3-admin</el-dropdown-item>
            </a>
            <a target="_blank" href="https://github.com/un-pany/v3-electron">
              <el-dropdown-item>GitHub</el-dropdown-item>
            </a>
            <el-dropdown-item divided @click="state.logout">
              <span style="display: block">退出登录</span>
            </el-dropdown-item>
          </el-dropdown-menu>
        </template>
      </el-dropdown>
    </div>
  </div>
</template>

<script lang="ts" setup>
import BreadCrumb from '../bread-crumb/index.vue'
import Hamburger from '../hamburger/index.vue'
import ThemeSwitch from '@/components/theme-switch/index.vue'
import Screenfull from '@/components/screenfull/index.vue'
import { computed, reactive } from 'vue'
import { store } from '@/store'
import { useRouter } from 'vue-router'

const router = useRouter()
const sidebar = computed(() => {
  return store.state.app.sidebar
})
const showThemeSwitch = computed(() => {
  return store.state.settings.showThemeSwitch
})
const showScreenfull = computed(() => {
  return store.state.settings.showScreenfull
})
const state = reactive({
  toggleSideBar: () => {
    store.commit('app/TOGGLE_SIDEBAR', false)
  },
  logout: () => {
    store.dispatch('user/logout')
    router.push('/login').catch((err) => {
      console.warn(err)
    })
  }
})
</script>

<style lang="scss" scoped>
.navbar {
  height: 50px;
  overflow: hidden;
  background: #fff;
  .hamburger {
    line-height: 50px;
    height: 100%;
    float: left;
    padding: 0 15px;
    cursor: pointer;
    &:hover {
      background: rgba(0, 0, 0, 0.025);
    }
  }

  .breadcrumb {
    float: left;
  }

  .right-menu {
    float: right;
    margin-right: 8px;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    color: #5a5e66;
    .right-menu-item {
      padding: 0 8px;
      cursor: pointer;
    }
    .avatar-container {
      .avatar-wrapper {
        padding: 0 8px;
        cursor: pointer;
        .user-avatar {
          width: 40px;
          height: 40px;
          border-radius: 10px;
        }
      }
    }
  }
}
</style>
