<template>
  <div class="viewpage" :style="{height:viewpageheight}">
    <div class="item-content" ref="itemcontent" :style="{width:itemcontentwidth,left:left}" @touchstart="touchstart">
      <slot></slot>
    </div>
  </div>
</template>
<script>
  export default {
    name: 'ViewPage',
    data() {
      return {
        itemcontentwidth: '0rem',
        itemwidth: 0,
        viewpageheight: '0rem',
        left: '0px',
        nodeNum: 0,
        nodes: {}
      }
    },
    methods: {
      convertListToArray(nodes) {
        var array = null;
        array = new Array();
        for (var i = 0, len = nodes.length; i < len; i++) {
          if (nodes[i].nodeType === 1) {
            array.push(nodes[i]);
          }
        }
        return array;
      },
      touchstart(ev) {
        this.itemwidth = this.node.offsetWidth;
        var _this = this;
        var startX = ev.touches[0].clientX;
        var tagView = ev.currentTarget;
        var h=100;
        var yLeft = tagView.offsetLeft;
        var maxLeft = (1 - this.nodeNum) * this.itemwidth;
        console.log(this.itemwidth);
        document.ontouchmove = function (ev) {
          var oEvent = ev || event;
          var left = yLeft + oEvent.touches[0].clientX - startX;
          if (left > 0) {
           // h=h*0.8;
            left = 0;//yLeft + h;
          }
          if (left < maxLeft) {
            left = maxLeft;
          }
          _this.move(left);
          ev.preventDefault();
        }
        document.ontouchend = function (ev) {
          var oEvent = ev || event;
          var fLeft = yLeft + oEvent.changedTouches[0].clientX - startX;
          var m = -parseInt(fLeft / _this.itemwidth);
          var n = fLeft % _this.itemwidth;
          var targtLeft;
          if (n > -_this.itemwidth / 2) {
            targtLeft = -m * _this.itemwidth;
          } else {
            targtLeft = -(m + 1) * _this.itemwidth;
          }
          if (targtLeft < maxLeft) {
            targtLeft = maxLeft;
          }
          _this.animateMove(fLeft, targtLeft, maxLeft);
//          console.log(m);
//          console.log(n);
//          console.log('------------');
          document.ontouchmove = null;
          document.ontouchend = null;
        }
      },
      move(left) {
        this.left = left + 'px';
      },
      animateMove(currentLeft, targetLeft, maxLeft) {
        var _this = this;
        var speed=30;
        var time = window.setInterval(function () {
          speed=speed-3;
          speed=speed<1?1:speed;
          if (currentLeft < targetLeft) {
            currentLeft = currentLeft + speed;
            if (currentLeft > targetLeft) {
              currentLeft = targetLeft;
              clearInterval(time);
            }
          } else {
            currentLeft = currentLeft - speed;
            if (currentLeft < targetLeft) {
              currentLeft = targetLeft;
              clearInterval(time);
            }
          }
          if (currentLeft < maxLeft) {
            currentLeft = maxLeft;
          }
          if (currentLeft>0){
            currentLeft=0;
          }
          _this.left = currentLeft + 'px';
        }, 10);
      }
    },
    mounted: function () {
      var nodes = this.convertListToArray(this.$refs.itemcontent.childNodes);
      this.itemcontentwidth = 7.5 * nodes.length + 'rem';
      this.viewpageheight = nodes[0].offsetHeight * 0.02 + 'rem';
      this.itemwidth = nodes[0].offsetWidth;
      this.nodeNum = nodes.length;
      this.node = nodes[0];
    }
  }
</script>
<style scoped>
  .viewpage {
    /*border: solid #00ccff 1px;*/
    width: 7.5rem;
    overflow: hidden;
    position: relative;
  }

  .viewpage .item-content {
    display: flex;
    position: absolute;
  }
</style>
