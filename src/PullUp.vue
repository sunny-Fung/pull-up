<template>
  <div>
    <slot></slot>
    <div class="pull-state" v-if="showState">
      <div class="finish" v-if="finish">
        <span class="pull-text">上滑加载更多</span>
      </div>
      <div class="more" v-else>
        <span class="pull-text">加载中</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      showState: true,
      finish: false,
      loading: false,
      domHeight: 0,
      container: null,
    };
  },
  mounted() {
    this.container = this.$el;
    this.domHeight = this.$el.clientHeight;
    this.switchBottom();
    this.bindScroll();
  },
  updated() {
    this.switchBottom();
  },
  methods: {
    scrollPage() {
      if (!this.$el) return;
      let domScrollTop = this.container.scrollTop;
      // console.log('domScrollTop', domScrollTop);
      // console.log(
      //   'this.$el.scrollHeight - domScrollTop - this.domHeight',
      //   this.$el.scrollHeight - domScrollTop - this.domHeight
      // );
      // console.log('this.loading', this.loading);
      // console.log('this.finish', this.finish);
      if (
        this.$el.scrollHeight - domScrollTop - this.domHeight < 30 &&
        !this.loading &&
        !this.finish
      ) {
        this.loading = true;
        const timer = setTimeout(() => {
          this.$emit("loadMore");
          clearTimeout(timer);
        }, 500);
      }
    },
    switchBottom() {
      this.$nextTick(() => {
        // console.log("this.$el.scrollHeight ", this.$el.scrollHeight);
        // console.log("this.domHeight", this.domHeight);
        if (this.$el.scrollHeight > this.domHeight) {
          console.log("step 2");
          console.log("this.finish", this.finish);
          this.showState = !this.finish ? true : false;
        } else {
          console.log("step 1");
          console.log("this.finish", this.finish);
          // 如果内容高度还不够填满一页，且还有数据没加载完，继续执行加载更多
          if (!this.finish) {
            console.log(
              "如果内容高度还不够填满一页，且还有数据没加载完，继续执行加载更多"
            );
            this.showState = true;
            const timer = setTimeout(() => {
              this.$emit("loadMore");
              clearTimeout(timer);
            }, 1000);
          } else {
            // 没有数据不显示底部加载状态
            this.showState = false;
          }
        }
      });
    },
    bindScroll() {
      this.unScroll();
      this.container &&
        this.container.addEventListener("scroll", this.scrollPage, false);
    },
    unScroll() {
      this.container &&
        this.container.removeEventListener("scroll", this.scrollPage, false);
    },
    beforeDestory() {
      this.unScroll();
    },
  },
};
</script>

<style lang="scss" scoped>
.pull-text {
  position: relative;
  top: vw(477);
  color: rgba(#000, 0.8);
  display: inline-block;
  width: 100vw;
  text-align: center;
}
</style>
