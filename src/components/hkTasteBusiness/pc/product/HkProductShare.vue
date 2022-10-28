<template>
  <div class="productshare_icon_warpper">
    <a
      onclick="window.open('http://www.facebook.com/share.php?u=' + window.location.href+'&t=' + document.title);"
      ><img src="/HW/pro-fb.png"
    /></a>
    <a @click="whatsApp" class="xq-whatsapp" data-action="share/whatsapp/share"
      ><img src="/HW/pro-whatsapp.png"
    /></a>
    <a
      onclick="window.open('https://lineit.line.me/share/ui?url='+window.location.href);"
      ><img src="/HW/pro-line.png"
    /></a>
  </div>
</template>
<script lang="ts">
import { Vue, Prop, Component } from 'vue-property-decorator';
@Component
export default class HkProductShare extends Vue {
  whatsApp() {
    var u = location.href;
    var t = document.title;
    window.open(
      'https://api.whatsapp.com/send?text=' +
        encodeURIComponent(t) +
        encodeURIComponent('\n\n' + u),
      '_blank'
    );
  }
}
</script>
<style lang="less" scoped>
.productshare_icon_warpper {
  text-align: right;
}
.productshare_icon_warpper a {
  display: inline-block;

  &:not(:last-child) {
    margin-right: 15px;
  }
}
.productshare_icon_warpper img {
  // width: 30px;
  cursor: pointer;
}
</style>
