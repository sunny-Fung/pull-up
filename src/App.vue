<template>
  <div id="app">
    <PullUp ref="LoadMore" @loadMore="onScrollBottom" class="home scrollable">
      <div class="content">
        <div class="item" v-for="(item, index) in arr" :key="index">
          <div class="item-cover" />
          <img :src="item.img" alt="图片" class="item-img" />
          <div class="item-text">{{ item.title }}</div>
          <div class="item-date">
            <span>{{ index }}||{{ item.date }}</span>
            <span>{{ item.review }}阅读</span>
          </div>
          <div class="item-arrow cbg"></div>
        </div>
      </div>
    </PullUp>
  </div>
</template>

<script>
import config from "./data";
import PullUp from "./PullUp";
export default {
  components: {
    PullUp,
  },
  data() {
    return {
      views: [],
      config,
    };
  },
  computed: {
    arr() {
      return this.views.slice(0).reverse();
    },
  },
  methods: {
    onScrollBottom() {
      console.log("pull up");
      this.views = this.views.concat(this.config);
      // 如果没有更多数据，则加载完成
      // 这里假设数据量达40停止
      if (this.views.length > 39) {
        this.$refs.LoadMore.finish = true;
        return;
      }
      this.$refs.LoadMore.loading = false;
    },
  },
};
</script>

<style lang="scss" scoped>
@function vw($x, $vpw: 750) {
  @return ($x / $vpw * 100) * 1vw;
}

.cbg {
  background-repeat: no-repeat;
  background-size: contain;
  background-position: center;
}

#app {
  height: 100%;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
  -webkit-overflow-scrolling: touch;
  user-select: none;
}
.home {
  position: relative;
  width: 100vw;
  height: 100vh;
  background: linear-gradient(#ff833f, #ffc066);
  overflow: scroll;
}

.content {
  position: relative;
  width: 100vw;
  left: 0;
  right: 0;
  margin: 0 auto;
  top: 0;
}

.item {
  position: relative;
  width: vw(710);
  height: vw(240);
  border-radius: vw(20);
  background-color: rgb(255, 160, 37);
  left: 0;
  right: 0;
  margin: 0 auto;
  margin-bottom: vw(25);
}

.item-cover {
  border-radius: vw(16);
  background-color: rgb(255, 255, 255);
  position: absolute;
  left: vw(5);
  top: vw(5);
  width: vw(700);
  height: vw(230);
}

.item-img {
  background-color: rgb(255, 244, 191);
  position: absolute;
  left: vw(24);
  top: vw(25);
  width: vw(220);
  height: vw(190);
}

.item-text {
  position: absolute;
  left: vw(266);
  top: vw(60);
  font-size: vw(28);
  color: rgb(51, 51, 51);
  font-weight: bold;
  line-height: vw(36);
  width: vw(310);
  height: vw(70);
  max-height: vw(70);
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
  overflow: hidden;
}

.item-date {
  font-size: vw(21);
  color: rgb(153, 153, 153);
  line-height: vw(21);
  position: absolute;
  left: vw(264);
  top: vw(177);
  width: vw(200);
  height: vw(20);
  display: flex;
  justify-content: space-between;
}

.item-arrow {
  position: absolute;
  width: vw(51);
  height: vw(51);
  right: vw(26);
  top: vw(95);
  background-image: url("./arrow.png");
}
</style>
