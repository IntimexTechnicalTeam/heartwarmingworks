<template>
<div class="header-layout mid-box" v-cloak>
  <div class="menu-nav" v-click-outside="close">
    <img src="/HW/nav.png" @click="show = !show" />

    <div class="nav-box" v-show="show">
      <div class="handle-group">
        <InsLogin class="memberLogin"></InsLogin>
        <Shopcart class="memberLogin"></Shopcart>

        <div class="handle-one fav">
          <router-link to="/account/MyFavorite" class="handle-icon fav-icon"></router-link>
        </div>
      </div>

      <div class="switch-group">
        <CodeSelect/>
        <InsLangSwitch />
      </div>

      <Menu />
      <img class="mini-log" src="/HW/mini-log.png" />
    </div>
  </div>
</div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';
@Component({
  components: {
    Menu: () =>
      import('@/components/business/pc/header/InsMenu.vue'),
    Shopcart: () =>
      import('@/components/business/pc/header/InsShoppingCart.vue'),
    InsLogin: () =>
      import('@/components/business/pc/header/InsLogin.vue'),
    InsLangSwitch: () =>
      import('@/components/business/pc/header/InsLangSwitch.vue'),
    CodeSelect: () =>
      import('@/components/business/pc/header/InsCodeSelect.vue')
  }
})
export default class DefaultHeader extends Vue {
  @Prop() private showInFixed!: boolean;

  private key: string = '';
  private show: boolean = false;

  close () {
    this.show = false;
  }

  getMenu () {
    this.$Api.promotion
      .getMenu()
      .then(result => {
        this.$store.dispatch('setHeaderMenus', result.ReturnValue.HeaderMenus);
        this.$store.dispatch('setFooterMenus', result.ReturnValue.FooterMenus);
      })
      .catch(error => {
        console.log(error);
      });
  }

  searchFun (key) {
    this.$store.dispatch('setSearchKey', key);
    if (key !== '') {
      this.$router.push({
        path: '/product/search',
        name: 'productSearch',
        params: {
          key: key
        }
      });
    } else {
      this.$router.push({
        path: '/product/search/-'
      });
    }
  }
  get currentlang () {
    return this.$i18n.locale;
  }
  private changLange (lang) {
    this.$Api.member
      .setUILanguage(lang)
      .then(result => {
        this.$i18n.locale = lang;
        localStorage.setItem('locale', lang);
        this.Reload();
      })
      .catch(error => {
        console.log(error);
      });
  }

  created () {
    this.$store.dispatch('setShopCart', this.$Api.shoppingCart.getShoppingCart());
  }

  mounted () {
    this.getMenu();
  }

  @Watch('$route', { deep: true })
  onIdChange () {
    this.show = false;
  }
}
</script>

<style scoped lang="less">
.showMenuYes{
  height:151px;
  transition:all 1s;
}
#header{
  z-index: 9999;
  top:0px;
  width: 100%;
  display: flex;
}
.headerBg{
   width: 100%;
   background:#fff;
   background-size: cover;
   display: inline-block;
   box-shadow: 0 0 10px 0 #d4d5d1;
}
.headerTop{
    width: 1200px;
    margin: 0 auto;
    padding-top: 10px;
    height: 34px;
}
.headerTop .inner{
    float: right;
}
.search-box {
    border: 1px solid #808080;
    width: 340px;
    display: flex;
    float: left;
    align-items: center;
    margin-right: 20px;
}
.searchBtn{
    width: 25px;
    height: 25px;
    display: inline-block;
    background: url('/images/pc/pcindex_03.png') no-repeat center center;
    background-size: 100%;
    cursor: pointer;
}
.search-box .inputBox {
    width: 305px;
    float: left;
    border:none;
    background: transparent;
    line-height: 30px;
    text-indent: 10px;
}
.search-box  .btn-send {
    position: absolute;
    right: 10px;
    top: 6px;
    line-height: 1;
    background: none;
    border: none;
    color: #555;
}
.search-box form {
    position: relative;
    font-size: 12px;
}
.search-box input {
    font-family: inherit;
    font-size: inherit;
    font-style: inherit;
    font-weight: inherit;
    outline: 0;
}
.memberLogin{
    display: flex;
    float: left;
    align-items: center;
    position: relative;
    margin-right: 10px;
}
.cartTop{
    display: flex;
    float: left;
    align-items: center;
    position: relative;
    margin-right: 10px;
}
.langBox{
    display: flex;
    float: left;
    align-items: center;
    position: relative;
    justify-content: center;
    margin-left: 20px;
}
.langBox a{
   color:#323232;
   font-size: 14px;
    line-height: 32px;
    margin-right: 10px;
    width: 40px;
    display: inline-block;
    text-align: center;
}
.langBox a:nth-child(2){
    margin-right: 0px!important;
}
.langActive{
    background: #808080;
    color:#FFF!important;
}
.logoBox{
    width: 100%;
    text-align: center;
    display: flex;
    align-items: center;
    justify-content: center;
    padding-top: 40px;
}
.logoBox a{
    display: flex;
}
.logoBox a img{
   width: 100%;
}

.fav-icon {
    background: url('/HW/heart.png') no-repeat center center;
    display: inline-block;
    width: 25px;
    height: 25px;
    background-size: contain;
}

// new css
.header-layout {
  position: relative;

  .menu-nav {
    position: absolute;
    top: calc(100% + 50px);
    right: calc(100% + 50px);
    z-index: 2;
    cursor: pointer;

    /deep/ .nav-box {
      min-height: 750px;
      width: 150px;
      position: absolute;
      top: 100%;
      right: 0;
      background: #fff;
      padding: 30px 10px;
      box-sizing: border-box;

      .handle-group {
        display: flex;
        justify-content: space-between;
        align-items: center;

        .handle-one.fav {
          font-size: 0;
        }

        .handle-icon {
          width: 22px;
          height: 22px;
        }

        .cart-icon {
          background: url(/HW/cart.png) no-repeat center center;
          background-size: contain;
        }

        .member-icon {
          background: url(/HW/user.png) no-repeat center center;
          background-size: contain;
        }

        .top-member-detail {
          left: 0;

          .window-top {
            right: unset;
            left: 0;
          }
        }
      }

      .switch-group {
        margin: 25px 0;
        display: flex;
        justify-content: space-between;
        align-items: center;

        #code-select {
          margin: 0;
          padding-right: 15px;
          appearance: none;
          -moz-appearance: none;
          -webkit-appearance: none;
          color: #808080;
          background: url('/HW/arrow-down.png') no-repeat right 5px center;
          background-size: 10px;

          // option{
          //   color:#666666;
          //   outline: none;
          // }
        }

        .langSwitch {
          p {
            font-size: 15px;
            padding: 0 3px;
            color: #808080;
            text-transform: uppercase;

            &.active {
              background: #fff;
              color: #808080 !important;
            }
          }
        }
      }

      .header_menu {
        ul > li {
          width: 100%;

          > a {
            font-size: 18px;
            color: #808080;
          }

          > ul {
            top: 0;
            left: 100%;

            > li:hover {
              background-color: #fff;

              > a {
                color: #808080;
              }
            }
          }
        }
      }

      .mini-log {
        width: 80%;
        margin: 0 auto;
        display: block;
      }
    }
  }
}

@media screen and (max-width: 1600px){
  .header-layout .menu-nav {
    .nav-box {
      right: unset;
      left: 0;
    }
  }
}

@media screen and (max-width: 1420px){
  .header-layout .menu-nav {
    right: unset;
    left: 0;
    background: #ffff;
  }
}
</style>
