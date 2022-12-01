<template>
  <div>
    <Header v-if="!(routerPath==='/building' || routerName==='vr')" />
    <router-view></router-view>
    <Footer v-if="!(routerPath==='/building' || routerName==='vr')"/>
    <Sidebar />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
@Component({
  components: {
    Header: () => import(/* webpackChunkName: "main" */ '@/components/business/pc/header/InsHeader.vue'),
    Footer: () => import(/* webpackChunkName: "main" */ '@/components/business/pc/footer/InsFooter.vue'),
    Sidebar: () => import(/* webpackChunkName: "main" */ '@/components/business/pc/header/InsSidebar.vue')
  }
})
export default class pcIndex extends Vue {
  get routerPath() {
    return this.$route.path;
  }

  get routerName() {
    return this.$route.name;
  }
}
</script>

<style lang="less" scoped>
</style>
