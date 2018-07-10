<template>
  <div class="search-container">
    <input class="search-input" v-model.trim="keywords"
           placeholder="请输入关键字"/>
    <div class="search-content">
      <div class="lside sub-container">
        <div class="search-list">
          <div class="search-item">
            <div class="search-item-content" :key="index" v-if="searchLess.length>=1"
                 v-for="(item,index) in searchLess">
              <input class="search-item-input" type="checkbox" :id="item.name" :value="item.name" v-model="selected">
              <label class="search-item-label" :for="item.name">{{item.name}}({{item.id}})</label>
            </div>
            <div class="load-more" v-if="searchItem && searchLess.length < searchItem.length" @click="handleLoadMore">
              加载更多
            </div>
            <div v-if="!searchItem || searchItem.length === 0">暂无符合条件内容</div>
          </div>
        </div>
      </div>
      <div class="rside sub-container">
        <div class="select-res">
          <span v-for="item in selected">{{item}}</span>
        </div>

      </div>
    </div>
    <div class="button-group">
      <button @click="handleClick">确定</button>
      <button @click="handleCancel">取消</button>
    </div>

  </div>
</template>

<script>
  export default {
    name: 'search',
    props: {
      search: {
        type: Array,
        required: true
      },
      selectedItem: {
        required: false
      },
      sliceNum: {
        required: false,
        type: Number,
        default: 10
      },
    },
    data() {
      return {
        selected: [],
        keywords: '',
        delay: 500,
        searchItem: [],
        timer: null,
        count: this.sliceNum
      };
    },
    computed: {
      // 分页
      searchLess() {
        return this.searchItem.slice(0, this.count);
      }
    },
    watch: {
      keywords(val) {
        if (val) {
          // eg. '(' => '\(','+' => '\+'
          val = `${val}`.replace(/([.?*+^$[\]\\(){}|-])/g, '\\$1');
          let origin = this.search;
          let reg = new RegExp(`${val}`);
          if (this.keywords && origin && Array.isArray(origin)) {
            this.searchItem = origin.filter(item => reg.test(item.name));
          } else {
            this.searchItem = this.search;
          }
        } else {
          this.searchItem = this.search;
        }

        // this.$emit('update:keyword', val)
        clearTimeout(this.timer);
        this.timer = setTimeout(() => {
          this.$emit('change-keyword', val);
        }, this.delay);
      },
      immediate: true
    },
    created() {
      if (this.selectedItem && this.selectedItem.length) {
        this.selected = this.selectedItem;
      }
      this.searchItem = this.search;
    },
    mounted() {
    },
    methods: {
      handleClick() {
        this.$emit('content-change', { values: this.selected });
      },
      handleCancel() {
      },
      handleLoadMore() {
        this.count += this.sliceNum;
      }
    }
  };
</script>

<style scoped lang="less">
  input {
    border: 1px solid #ccc;
    padding: 7px 0 7px 5px;
    border-radius: 3px;
    box-shadow: inset 0 1px 1px rgba(0, 0, 0, .075);
    transition: border-color ease-in-out .15s, box-shadow ease-in-out .15s
  }

  input:focus {
    border-color: #66afe9;
    outline: 0;
    box-shadow: inset 0 1px 1px rgba(0, 0, 0, .075), 0 0 8px rgba(102, 175, 233, .6)
  }

  .search-container {
    padding: 10px;
    .search-input {
      width: 100%;
      border: 1px solid #f0f0f0;
    }
    .search-content {
      padding: 10px 0;
      .lside, .rside {
        display: inline-block;
        width: 49.5%;
      }
      .lside {
        vertical-align: top;
        .search-list {
          background: #f0f0f0;
          overflow: auto;
          .search-item {
            margin-top: 1%;
            width: 99.5%;
            border: 1px solid #f0f0f0;
            overflow: auto;
            height: 59%;
            &-content {
              display: inline-block;
              width: 100%;
              white-space: nowrap;
            }
          }
        }
        .load-more {
          cursor: pointer;
          color: #f85959;
          text-align: center;
        }
      }
      .rside {
        .select-res {
          resize: none;
          width: 100%;
          height: 72%;
          span {
            display: inline-block;
            padding-right: 10px;
          }
        }

      }
    }
    .button-group {
      margin-top: 20px;
      text-align: center;
      button {
        width: auto;
        padding: 8px 30px;
        line-height: 18px;
        font-size: 16px;
        background: #519cf8;
        color: #fff;
        cursor: pointer;
        border: 0;
        border-radius: 3px;
      }
      button + button {
        margin-left: 50px;
      }

    }
  }
</style>
