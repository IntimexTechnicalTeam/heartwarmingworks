<template>
    <div class="slide-menu">
        <img src="/HW/nav.png" class="nav-btn" @click="closeSlideMenu" />

        <div class="nav-box">
            <div class="handle-group">
                <ins-login />
                <shopcart class="shoppingcart" />

                <ins-fav  />
            </div>

            <div class="switch-group">
                <CodeSelect/>
                <InsLangSwitch />
            </div>

            <Menu textColor="#808080" backColor="#fff" id="menu" />
            <img class="mini-log" src="/HW/mini-log.png" />
        </div>
    </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component({
  components: {
    InsLogin: () => import('@/components/business/mobile/header/InsLogin.vue'),
    InsFav: () => import('@/components/business/mobile/header/InsFav.vue'),
    shopcart: () => import('@/components/business/mobile/header/InsShoppingCart.vue'),

    InsLogo: () => import('@/components/base/mobile/InsLogo.vue'),
    Menu: () => import('@/components/business/mobile/header/InsElMenu.vue'),
    InsLangSwitch: () => import('@/components/business/mobile/header/InsLangSwitch.vue'),
    CodeSelect: () => import('@/components/business/mobile/header/InsCodeSelect.vue')
  }
})
export default class InsMenuLayout extends Vue {
  showMemNav: boolean = false;
  searchKey: string = '';

  handleOpen (key, keyPath) {
    console.log(key, keyPath);
  }
  handleClose (key, keyPath) {
    console.log(key, keyPath);
  }

  closeSlideMenu () {
    this.$store.dispatch('isShowMenu', false);
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
      this.$store.dispatch('isShowMenu', !this.$store.state.isShowMenu);
    } else {
      this.$router.push({
        path: '/product/search/-'
      });
      this.$store.dispatch('isShowMenu', !this.$store.state.isShowMenu);
    }
  }

  get user () {
    return this.$store.state.user;
  }

  get isLogin () {
    return this.$store.state.isLogin;
  }
}
</script>

<style lang="less">
.slide-menu {
    padding: 1rem;

    .nav-btn {
        width: 2.2rem;
    }

    .nav-box {
        .handle-group {
            display: flex;
            align-items: center;
            margin: 2rem 0;

            .memberlogin {
                .menberCentral {
                    padding-left: 0;
                    border: 0;
                    img {
                        width: auto;
                        height: 1.8rem;
                    }
                }
            }

            .handle-one.shoppingcart{
                margin: 0 1.2rem;

                .cart-icon {
                    height: 1.8rem;
                    background: url(/HW/cart.png) no-repeat center center;
                    background-size: contain;
                }
            }

            .headerFav {
                border: 0;
                img {
                    width: auto;
                    height: 1.8rem;
                }
            }
        }

        .switch-group {
            margin: 25px 0;
            display: flex;
            align-items: center;

            .main-code {
                margin-right: 1rem;
            }

            #code-select {
                font-size: 1.2rem;
                margin: 0;
                padding-left: 0;
                padding-right: 15px;
                appearance: none;
                -moz-appearance: none;
                -webkit-appearance: none;
                color: #808080;
                background: url(/HW/arrow-down.png) no-repeat right 3px center;
                background-size: 8px;

            // option{
            //   color:#666666;
            //   outline: none;
            // }
            }

            .langSwitch {
                p {
                    font-size: 1.2rem;
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

        #menu {
            border: 0;
            > li {
                padding: 0 !important;

                > a > b,
                > .el-submenu__title > .name {
                    font-size: 2rem;
                    color: #808080;
                    font-weight: normal;
                }

                > .el-submenu__title {
                    padding: 0 !important;

                    > .el-submenu__icon-arrow {
                        font-size: 1.5rem;
                    }
                }

                .el-submenu__title {
                    background-color: #fff !important;
                }

                li {
                    > a > b,
                    > .el-submenu__title > .name {
                        font-size: 1.5rem;
                        color: #808080;
                        font-weight: normal;
                    }

                    > .el-submenu__title {
                        > .el-submenu__icon-arrow {
                            font-size: 1.2rem;
                        }
                    }
                }
            }
        }

        .mini-log {
            height: 11rem;
        }
    }
}
</style>
