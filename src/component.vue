<template>
  <section v-if="total > 0" class="ui-pagination-layout clearfix block">
    <section class="inline-block left" v-if='isShowRecord'>
      <span class="fs-middle">共有{{ total }}条记录</span>
      <span class="fs-middle" v-if="maxIndex > 1">，显示第{{ minIndex }}-{{ maxIndex }}条</span>
    </section>
    <ul class="clearfix inline-block right ul">
       <li @click="skipStart" :class="[startUnable ? 'unclick': '']" class="other li">首页</li>
       <li @click="skipBefore" :class="[startUnable ? 'unclick': '']" class="other li">上页</li>
       <li @click="skipTo(item)" v-for="(item, index) in previewPage" :class="[item===current ? 'on' : '']" class="li">{{ item }}</li>
       <li @click="skipNext" :class="[endUnable ? 'unclick': '']" class="other li">下页</li>
       <li @click="skipEnd" :class="[endUnable ? 'unclick': '']" class="other li">末页</li>
    </ul>
  </section>
</template>
<script>
  export default {
    name: 'Pagination',
    props: ['options'],
    data () {
      return {
        total: 0,
        totalPage: 0,
        current: 1,
        previewCount: 5,
        onPageChanged: null,
        endCount: 0,
        startCount: 0,
        size: 10,
        isShowRecord: false
      }
    },
    created () { // 视图被创建
      this.setOptions();
    },
    methods: {
      setOptions () {
        let options = this.options || {};
        this.total = options.total || 0;
        this.totalPage = options.totalPage || 0;
        this.current = options.current || 1;
        this.size = options.size || 10;
        let tmp = this.total / this.size;
        this.totalPage = Math.ceil(tmp) > this.totalPage ? Math.ceil(tmp) : this.totalPage;
        this.onPageChanged = options.onPageChanged || null;
        this.isShowRecord = options.isShowRecord || false;
      },
      skipTo (item) {
        this.current = item;
        typeof this.onPageChanged === 'function' && this.onPageChanged(item);
      },
      skipStart () {
        if (!this.startUnable) this.skipTo(1);
      },
      skipBefore () {
        if (!this.startUnable) this.skipTo(this.current - 1);
      },
      skipNext () {
        if (!this.endUnable) this.skipTo(this.current + 1);
      },
      skipEnd () {
        if (!this.endUnable) this.skipTo(this.totalPage);
      }
    },
    watch: {
      options () {
        this.setOptions();
      }
    },
    computed: {
      previewPage () {
        let self = this;
        let temp = [];
        let startPreCount = self.current - 2;
        let endPreCount = startPreCount + self.previewCount;
        if (self.totalPage <= self.previewCount || self.current < self.previewCount) {
          let arr = self.totalPage;
          if (self.totalPage > self.previewCount) arr = self.previewCount;
          self.startCount = 1;
          self.endCount = arr;
          for (let i = 1; i <= arr; i++) {
            temp.push(i);
          }
        } else {
          if (endPreCount > self.totalPage + 1) endPreCount = self.totalPage + 1;
          self.endCount = endPreCount;
          self.startCount = (endPreCount - startPreCount) < 5 ? (endPreCount - self.previewCount) : startPreCount;
          for (let i = self.startCount; i < endPreCount; i++) {
            temp.push(i);
          }
        }
        return temp;
      },
      minIndex () {
        return this.current <= 1 ? 1 : (this.current - 1) * this.size + 1;
      },
      maxIndex () {
        return this.endCount <= 1 ? 1 : (this.current === this.totalPage ?  this.total : this.current * this.size);
      },
      startUnable () {
        return (this.startCount === this.current && this.startCount === 1);
      },
      endUnable () {
        return (this.totalPage === this.current);
      }
    }
  }
</script>
<style lang="scss" scoped="scoped">
$white: #fff;
$blue: #5cadff;
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
}
.fs-middle{font-size: 14px;}
.inline-block{display: block;}
.block{display: block;}
.clearfix:before,
.clearfix:after{
  display: block;
  clear: both;
  content: '';
  height:0;
}
.ui-pagination-layout{
    .left{
      float: left;
    }
    .right{
      float: right;
    }
    .ul{
        list-style: none;
        .li{
            border-right: 1px solid #ccc;
            cursor: pointer;
            display: block;
            padding: 4px;
            border: 1px solid #ccc;
            border-radius: 3px;
            text-align: center;
            float: left;
            color: #767676;
            font-size: 12px;
            line-height: 22px;
            height: 30px;
            min-width: 30px;
            margin-right: 3px;
            &:hover{
                background-color: $blue;
                color: $white;
            }
        }
        .li.other{
            padding-right: 10px;
            padding-left: 10px;
        }
        .li.unclick{
            cursor: not-allowed;
            background-color: #f5f5f5;
            &:hover{
                background-color: #f5f5f5;
                color: #767676;
            }
        }
        .li.on{
            background-color: $blue;
            color: $white;
        }
    }
}
</style>
