<template>
  <div class="header-bar">
    <div class="header-l">
      <sider-trigger @on-change="handleCollpasedChange"></sider-trigger>
      <!-- <span class="i-layout-header-trigger" @click="handleReload">
      <Icon type="ios-refresh" color='#fff' />
    </span> -->
      <custom-bread-crumb
        show-icon
        :list="breadCrumbList"
        :listLast="crumbPast"
      ></custom-bread-crumb>
    </div>

    <div class="custom-content-con">
      <slot></slot>
    </div>
  </div>
</template>
<style scoped lang="less">
.i-layout-header-trigger {
  position: absolute;
  margin: 0 10px;
  cursor: pointer;
  font-size: 19px;
}
.i-layout-header-trigger:hover {
  color: #2d8cf0;
}
</style>
<script>
import siderTrigger from './sider-trigger';
import customBreadCrumb from './custom-bread-crumb';
import { R } from '@/libs/util';
import { mapState } from 'vuex';
import './header-bar.less';
export default {
  name: 'HeaderBar',
  components: {
    siderTrigger,
    customBreadCrumb,
  },
  props: {
    // collapsed: Boolean,
  },
  computed: {
    ...mapState('layout', ['menuCollapse']),
    breadCrumbList() {
      let openMenus = this.$store.state.menus.openMenus;
      let menuList = this.$store.state.menus.menusName;
      let allMenuList = R(menuList, []);
      let selectMenu = [];
      if (allMenuList.length > 0) {
        openMenus.forEach((i) => {
          allMenuList.forEach((a) => {
            if (i === a.path) {
              selectMenu.push(a);
            }
          });
        });
      }
      return selectMenu;
    },
    crumbPast() {
      let that = this;
      let menuList = that.$store.state.menus.menusName;
      let allMenuList = R(menuList, []);
      let selectMenu = [];
      if (allMenuList.length > 0) {
        allMenuList.forEach((a) => {
          if (that.$route.path === a.path) {
            selectMenu.push(a);
          }
        });
      }
      return selectMenu;
    },
  },
  mounted() {},
  methods: {
    handleCollpasedChange(state) {
      this.$emit('on-coll-change', state);
    },
    handleReload() {
      this.$emit('on-reload');
    },
  },
};
</script>
