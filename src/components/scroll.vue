<template>
  <div id="app">
    <div class="bc-body bc-ppt-content">
      <div class="bc-ppt-area" @scroll="handleScroll">
        <div class="bc-ppt-image-wrapper" v-for="item in pptData" :index="item.index" :key="item.index">
          {{item.index}}
          <img class="ppt-content" width="100%"
               :src="item.img"
               :alt="item.alt">
        </div>
      </div>
      <div class="bc-link-area">
        <div class="link-line"></div>
        <div class="link-wrapper">
          <div v-for="item in pptData" v-if="item.link" class="link" :data-index="item.index"
               @click="activeLink(item.index)">
            <i class="circle-icon"></i>
            <span :href="item.link" class="link-item">
                            {{item.link}}
                        </span>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>
<script>
  const pptData = [
    {
      index: 0,
      img: 'http://ohwxyjv7u.bkt.clouddn.com/1aa543ff2132f6b1de93a4dadfc1f69e.png',
      alt: '宝宝打伞',
      link: '宝儿姐'
    }, {
      index: 1,
      img: 'http://ohwxyjv7u.bkt.clouddn.com/1510926798106.jpg',
      alt: '冈本零点零一',
      link: '冈本零点零一'
    }, {
      index: 2,
      img: 'http://ohwxyjv7u.bkt.clouddn.com/0.jpeg',
      alt: '埋喽',
      link: '我就想埋了你，咋的'
    }, {
      index: 3,
      img: 'http://ohwxyjv7u.bkt.clouddn.com/1383393385-935292309.jpg',
      alt: '菊理',
      link: '想死一次吗？'
    },
    {
      index: 4,
      img: 'http://ohwxyjv7u.bkt.clouddn.com/1319095900-1337303941.jpg',
      alt: '流泪',
      link: '写点啥玩意'
    }, {
      index: 5,
      img: 'http://ohwxyjv7u.bkt.clouddn.com/1319095900-1337303941.jpg',
      alt: '流泪',
    }, {
      index: 6,
      img: 'http://ohwxyjv7u.bkt.clouddn.com/1319095900-1337303941.jpg',
      alt: '流泪',
    }, {
      index: 7,
      img: 'http://ohwxyjv7u.bkt.clouddn.com/1319095900-1337303941.jpg',
      alt: '流泪',
    }, {
      index: 8,
      img: 'http://ohwxyjv7u.bkt.clouddn.com/1319095900-1337303941.jpg',
      alt: '流泪',
      link: '最后'
    },
  ];
  export default {
    name: 'scroll',
    data: function() {
      return {
        pptData: pptData,
        index: 0
      }
    },
    watch: {
      index(val, old) {
        console.log(val, old);
        this.activeLink(val)
      }
    },
    mounted() {
      this.initLinkGap(),
        this.activeLink(0)

    },
    methods: {
      /*
       * 根据图片编号判断上移量,停留在两图片中间
       * @param page
       * @returns {number}
       * @private
       */
      _page2scroll: function(page) {
        let height = 330;
        let gap = 40;
        return (height + gap) * page - gap / 2
      },
      /**
       * 由于滚动本身就是抖动的，所以需要设置一个阈值
       **/
      _calcPos: function(val, div, gap) {
        let ceil = Math.ceil(val / div);
        let floor = Math.floor(val / div);
        return ceil - val / div < gap ? ceil : floor
      },
      /**
       * 设置 Scroll 值，根据 page 进行 scroll 值计算
       * @param page 需要滚动到的页数
       */
      initScroll: function(page) {
        let pptEl = this.$el.querySelector('.bc-ppt-content');
        let oPPT = pptEl.querySelector('.bc-ppt-area');
        oPPT.scrollTop = this._page2scroll(page);
      },
      initLinkGap: function() {
        let pptEl = this.$el.querySelector('.bc-ppt-content');
        let oLink = pptEl.querySelectorAll('.link');
        let oLine = pptEl.querySelector('.link-line');
        let wrapperHeight = pptEl.querySelector('.link-wrapper').clientHeight;
        oLine.style.height = (pptEl.querySelector('.link-wrapper').clientHeight - 10) + 'px';
        let gap = wrapperHeight / (oLink.length - 1) > 56 ? (wrapperHeight - 20) / (oLink.length - 1) - 20 : 40
        oLink && oLink.length && oLink.forEach(function(value, index) {
          if (index) {
            value.style.marginTop = gap + 'px';
          }
        })
      },
      handleScroll: function(e) {
        let pptEl = this.$el.querySelector('.bc-ppt-content');
        let oPPT = pptEl.querySelector('.bc-ppt-area');
        let imgWrapper = pptEl.querySelector('.bc-ppt-image-wrapper');
        let img = imgWrapper.querySelector('img');
        let index = this._calcPos(oPPT.scrollTop, img.clientHeight + 40, 0.05);
        this.index = index;
      },
      activeLink: function(id) {
        let pptEl = this.$el.querySelector('.bc-ppt-content');
        let oLink = pptEl.querySelectorAll('.link');
        if (oLink[id]) {
          if (oLink[id].getAttribute('data-index') === '' + id) {
            pptEl.querySelectorAll('.link').forEach(function(value) {
              value.classList.remove('active-link')
            });
            pptEl.querySelectorAll('.link')[id].classList.add('active-link')

          }
        }
        this.initScroll(id)
      }
    }
  }
</script>

<style lang="less">
  .bc-ppt-content {
    display: flex;
    .bc-ppt-area {
      flex: 0 0 566px;
      width: 566px;
      height: 555px;
      box-sizing: border-box;
      margin-top: 5px;
      overflow-y: scroll;
      &::-webkit-scrollbar {
        display: none;
      }
      .bc-ppt-image-wrapper + .bc-ppt-image-wrapper {
        margin-top: 40px;
      }
      .bc-ppt-image-wrapper {
        position: relative;
        padding-bottom: 51%;
        .ppt-content {
          position: absolute;
          left: 0;
          right: 0;
          top: 0;
          bottom: 0;
          img {
            width: 100%;
          }
        }
      }
    }
    .bc-link-area {
      flex: 1;
      display: flex;
      height: 560px;
      padding-left: 69px;
      .link-line {
        flex: 0 0 0;
        width: 0;
        margin-top: 10px;
        border-left: 1px solid #e0e0e0;
      }
      .link-wrapper {
        flex: 1;
        display: flex;
        height: 100%;
        flex-direction: column;
        .link {
          display: inline-block;
          padding: 0 0 0 16px;
          position: relative;
          cursor: pointer;
          .circle-icon {
            position: absolute;
            top: 5px;
            margin-left: -21px;
            width: 10px;
            height: 10px;
            border-radius: 10px;
            background: #e0e0e0;
          }
          .link-item {
            color: #4A4A4A;
            padding-left: 5px;
            font-size: 14px;
            cursor: pointer;
          }
        }
        .active-link {
          .circle-icon {
            border-radius: 0;
            width: 20px;
            height: 12px;
            top: 4px;
            left: 12px;
            background-size: 20px 12px;
            background-color: #ffffff;
            background-image: url("/static/icon_select.png");
          }
          .link-item {
            font-weight: 400;
          }
        }
      }
    }
  }
</style>
