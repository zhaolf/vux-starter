<template>
  <div style="height:100%">
    <div v-transfer-dom>
      <loading v-model="isLoading"></loading>
    </div>
    <view-box ref="viewBox"
              body-padding-top="46px"
              body-padding-bottom="55px">

      <my-header :menus="menus"
                 :leftOptions="leftOptions"
                 :title="title"
                 :rightOptions="rightOptions"
                 :changeLocale="changeLocale"></my-header>

      <transition :name="'vux-pop-' + (direction === 'forward' ? 'in' : 'out')">
        <router-view class="router-view"></router-view>
      </transition>

      <my-tabbar :showTabbar="isShowTarbar"
                 :componentName="componentName"></my-tabbar>
    </view-box>

  </div>
</template>

<script>
import { ViewBox, Loading, TransferDom } from 'vux'
import { mapState } from 'vuex'
import MyTabbar from '@/components/layout/Tabbar'
import MyHeader from '@/components/layout/MyHeader'

export default {
  name: 'app',
  data: function () {
    return {
      isShowTarbar: true,
      menus: {
        'language.noop': '<span class="menu-title">Language</span>',
        'zh-CN': '中文',
        'en': 'English'
      }
    }
  },
  directives: {
    TransferDom
  },
  components: { ViewBox, Loading, MyTabbar, MyHeader },
  methods: {
    changeLocale(locale) {
      console.info(locale)
    },
    onItemClick: function (event) {
      console.info('hello world')
    }
  },
  computed: {
    ...mapState({
      route: state => state.route,
      path: state => state.route.path,
      deviceready: state => state.app.deviceready,
      demoTop: state => state.vux.demoScrollTop,
      isLoading: state => state.vux.isLoading,
      direction: state => state.vux.direction
    }),
    leftOptions() {
      return {
        showBack: this.route.path !== '/'
      }
    },
    rightOptions() {
      return {
        showMore: true
      }
    },
    componentName() {
      if (this.route.path) {
        const parts = this.route.path.split('/')
        if (/component/.test(this.route.path) && parts[2]) return parts[2]
      }
    },
    title() {
      if (this.route.path === '/') return 'Home'
      if (this.route.path === '/project/donate') return 'Donate'
      if (this.route.path === '/demo') return 'Demo list'
      return this.componentName ? `Demo/${this.componentName}` : 'Demo/~~'
    }
  }
}
</script>

<style lang="less">
@import '~vux/src/styles/reset.less';
@import '~vux/src/styles/1px.less';
@import '~vux/src/styles/tap.less';

body {
  background-color: #fbf9fe;
}

html,
body {
  height: 100%;
  width: 100%;
  overflow-x: hidden;
}

.router-view {
  width: 100%;
  top: 46px;
}

.vux-pop-out-enter-active,
.vux-pop-out-leave-active,
.vux-pop-in-enter-active,
.vux-pop-in-leave-active {
  will-change: transform;
  transition: all 500ms;
  height: 100%;
  top: 46px;
  position: absolute;
  backface-visibility: hidden;
  perspective: 1000;
}

.vux-pop-out-enter {
  opacity: 0;
  transform: translate3d(-100%, 0, 0);
}

.vux-pop-out-leave-active {
  opacity: 0;
  transform: translate3d(100%, 0, 0);
}

.vux-pop-in-enter {
  opacity: 0;
  transform: translate3d(100%, 0, 0);
}

.vux-pop-in-leave-active {
  opacity: 0;
  transform: translate3d(-100%, 0, 0);
}

.menu-title {
  color: #888;
}
</style>
