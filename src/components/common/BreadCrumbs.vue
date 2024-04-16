<template>
  <el-breadcrumb class="app-breadcrumb" separator="→">
    <transition-group name="breadcrumb" class="box">
      <el-breadcrumb-item v-for="(item, index) in levelList" :key="item.path">
        <span v-if="item.redirect === 'noRedirect' || index == levelList.length - 1" class="no-redirect">{{ item.name
          }}</span>
        <a v-else @click.prevent="handleLink(item)">{{ item.name }}</a>
      </el-breadcrumb-item>
    </transition-group>
  </el-breadcrumb>
</template>

<script>
import pathToRegexp from 'path-to-regexp'
import { generateTitle } from '@/utils/i18n'
export default {
  data() {
    return {
      levelList: null
    }
  },
  watch: {
    $route() {
      this.getBreadcrumb()
    }
  },
  created() {
    this.getBreadcrumb()
    this.breadcrumbStyleChange()
  },
  methods: {
    generateTitle,
    getBreadcrumb() {
      // only show routes with meta.title
      let route = this.$route
      let matched = route.matched.filter(item => item.meta)
      const first = matched[0]
      matched = [{ path: '/index' }].concat(matched)

      this.levelList = matched.filter(item => item.meta)
    },
    isDashboard(route) {
      const name = route && route.name
      if (!name) {
        return false
      }
      return name.trim().toLocaleLowerCase() === 'Index'.toLocaleLowerCase()
    },
    pathCompile(path) {
      // To solve this problem https://github.com/PanJiaChen/vue-element-admin/issues/561
      const { params } = this.$route
      var toPath = pathToRegexp.compile(path)
      return toPath(params)
    },
    handleLink(item) {
      const { redirect, path } = item
      if (redirect) {
        this.$router.push(redirect)
        return
      }
      this.$router.push(path)
    },
    breadcrumbStyleChange(val) {
      this.$nextTick(() => {
        document.querySelectorAll('.app-breadcrumb .el-breadcrumb__separator').forEach(el => {
          el.innerText = "→"
          el.style.color = "rgba(77, 82, 89, 1)"
        })
        document.querySelectorAll('.app-breadcrumb .el-breadcrumb__inner a').forEach(el => {
          el.style.color = "rgba(251, 252, 253, 1)"
        })
        document.querySelectorAll('.app-breadcrumb .el-breadcrumb__inner .no-redirect').forEach(el => {
          el.style.color = "rgba(255, 255, 255, 1)"
        })

        let str = "vertical"
        if ("vertical" === str) {
          let headHeight = "60px"
          headHeight = parseInt(headHeight) + 10 + 'px'
          document.querySelectorAll('.app-breadcrumb').forEach(el => {
            el.style.marginTop = headHeight
          })
        }

      })
    },
  }
}
</script>

<style lang="scss" scoped>
.app-breadcrumb {
  display: block;
  font-size: 14px;
  line-height: 50px;
  background-color: #5CACEE; // 指定的中等亮度蓝色
  border-radius: 1px;
  padding: 0 20px;
  box-shadow: 0 0 8px #aaaaaa;
  margin-bottom: 10px;

  .box {
    display: flex;
    width: 100%;
    height: 100%;
    justify-content: flex-start;
    align-items: center;
  }

  .el-breadcrumb__separator {
    color: #fff; // 面包屑分隔符颜色为白色
  }

  .el-breadcrumb__inner a {
    color: #ebf5ff; // 面包屑可点击链接的颜色为淡蓝色
    &:hover {
      color: #fff; // 悬停时颜色变为白色
      text-decoration: underline; // 添加下划线
    }
  }

  .no-redirect {
    color: #fff; // 不可点击的面包屑颜色为白色
    cursor: default;
  }
}
</style>
