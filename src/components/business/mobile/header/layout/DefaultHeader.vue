<template>
    <div class="header-box mid-box">
      <img src="/HW/nav.png" class="menu-nav" @click="showSlideMenu" />
    </div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';

@Component({
  components: {
    InsLogo: () => import('@/components/base/mobile/InsLogo.vue'),
    InsLangSwitch: () => import('@/components/business/mobile/header/InsLangSwitch.vue'),
    InsLogin: () => import('@/components/business/mobile/header/InsLogin.vue'),
    InsFav: () => import('@/components/business/mobile/header/InsFav.vue'),
    shopcart: () => import('@/components/business/mobile/header/InsShoppingCart.vue'),
    InsMenu: () => import('@/components/business/mobile/header/InsMenu.vue'),
    CodeSelect: () =>
      import('@/components/business/mobile/header/InsCodeSelect.vue')
  }
})
export default class DefaultHeader extends Vue {
  @Prop() private showInFixed!: boolean;

  showSlideMenu () {
    let isShow = !JSON.parse(JSON.stringify(this.menuShow));
    this.$store.dispatch('isShowMenu', isShow);
  }

  get menuShow () {
    return this.$store.state.isShowMenu;
  }

  get isMobile () {
    return this.$store.state.isMobile;
  }
  get ShopCart () {
    return this.$store.state.shopCart;
  }
  created() {
    this.$store.dispatch('setShopCart', this.$Api.shoppingCart.getShoppingCart());
  }
}
</script>

<style scoped lang="less">
.header-box {
  position: relative;

  .menu-nav {
    position: absolute;
    top: 1rem;
    left: 0;
    z-index: 2;
    width: 2.2rem;
  }
}
</style>
