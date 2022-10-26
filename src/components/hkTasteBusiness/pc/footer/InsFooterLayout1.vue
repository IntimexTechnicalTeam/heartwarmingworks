<template>
<div id="footer">
  <div class="mid-box">
    <p class="footercopy">
      <span>COPYRIGHT @ 暖心工房 Limited Powered by Eventizer</span>
    </p>
  </div>
</div>

</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component
export default class InsFooterLayout1 extends Vue {
  currentYear: number = 0;
  footerMenus: any[] = [];
  goToTop () {
    let sTop = document.documentElement.scrollTop;
    let times = setInterval(() => {
      sTop -= 50;
      if (sTop <= 0) {
        document.documentElement.scrollTop = 0;
        clearInterval(times);
      } else {
        document.documentElement.scrollTop = sTop;
      }
    }, 1);
  }
  To (n) {
    return n.Type === 1 ? '/cms/catDetail/' + n.Value.Id : n.Type === 2 ? '/CMS/content/' + n.Value.Id : n.Type === 3 ? '/RegNPay/Form/' + n.Value.Id : n.Type === 4 && !this.$store.state.catMenuType ? '/product/cat/' + n.Value.Id : n.Type === 4 && this.$store.state.catMenuType ? '/product/search/-?catalogs=' + JSON.stringify([parseInt(n.Value.Id)]) + '&type=0' : n.Type === 5 ? '/product/search/-?attrs=' + JSON.stringify([{ Id: parseInt(n.Value.Id), Vals: [] }]) + '&type=0' : '/product/search/-?attrs=' + JSON.stringify([{ Id: parseInt(n.ParentId), Vals: [parseInt(n.Value.Id)] }]) + '&type=0';
  }
  getMenu () {
    this.$Api.promotion.getMenu().then((result) => {
      this.footerMenus = result.ReturnValue.FooterMenus;
    });
  }
  created () {
    var date = new Date();
    this.currentYear = date.getFullYear();
    this.getMenu();
  }
}
</script>

<style scoped lang="less">
/* 底部文件 */
.footbg{
    background: #9f2f34 url('/images/pc/pcindex_05.jpg') no-repeat center bottom;
    background-size: cover;
    width: 100%;
    display: inline-block;
    padding-bottom: 10px;
    min-height: 278px;
}
.footerMain{
    width: 1200px;
    margin: 0 auto;
}
.footerTop{
    text-align: center;
    padding-top: 25px;
    padding-bottom: 25px;
    width: 100%;
}
.footerTop p{
    text-align: center;
    display: inline-block;
    margin-right: 50px;
}
.footerTop p span{
    font-size: 14px;
    color: #FFF;
    line-height: 35px;
    margin-right: 15px;
}
.footerTop p b{
    font-weight: 100;
    font-size: 35px;
    color: #FFF;
    line-height: 35px;
}
.footerBotttom{
    width: 100%;
}
.footerLeft{
    float: left;
    width: 40%;
}
.footerLeft > ul{
    float: left;
    margin-right: 10%;
}
.footerLeft > ul >li{
    width: 100%;
    line-height: 30px;
}
.footerLeft > ul >li >a{
    font-size:20px;
    color:#FFF;
}
.footerLeft > ul >li >ul{
  width: 100%;
}
.footerLeft > ul >li >ul a{
    font-size: 16px;
    color:#FFF;
    display: inline-block;
    text-transform: uppercase;
}
.footerLeft > ul >li >ul a:hover{
   transform: translateY(-3px);
}
.footerLeft p{
    width: 100%;
    display: block;
    font-size: 14px;
    color: #fff;
    padding-top: 20px;
}
.footerLeft p img{
    display: inline-block;
    vertical-align: middle;
    padding-left: 10px;
}
.footerRight{
    float: right;
    width: 40%;
    text-align: center;
}
.footerRight img{
  width: 60%;
  display: block;
}
.footercopy{
  width: 100%;
  display: inline-block;
  padding: 30px 0;
  text-align: right;
}
.footercopy span:nth-child(1){
  color:#808080;
  font-size: 16px;
}
.footercopy span:nth-child(1) img{
  display: inline-block;
  vertical-align:middle;
  padding-left: 10px;
}
.footercopy span:nth-child(2){
  float: right;
  width: 40%;
  text-align: center;
  color:#808080;
  font-size: 14px;
}
.footercopy span:nth-child(2) img{
  display: inline-block;
  vertical-align:middle;
  padding-left: 10px;
}
</style>
