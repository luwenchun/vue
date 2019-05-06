<template>
  <div class="home">
    <!-- 轮播图 -->
    <cube-slide :data="slider" :interval="5000">
      <cube-slide-item v-for="item in slider" :key="item.id">
        <router-link :to="`/detail/${item.id}`">
          <img :src="item.img" class="slider">
        </router-link>
      </cube-slide-item>
    </cube-slide>

    <!-- 触发分类选择按钮 -->
    <cube-button @click="showCatg($event)">选择分类</cube-button>

    <!-- 商品列表 -->
    <goods-list :goods="filterGoods" @addCart="onAddCart"></goods-list>

    <!-- 商品分类列表 -->
    <cube-drawer ref="drawer" title="请选择分类" :data="[drawerList]" @select="selectHandler"></cube-drawer>
    <!-- 加购动画载体 -->
    <!-- <div class="ball-wrap">
      <transition @before-enter="beforeEnter" @enter="enter" @after-enter="afterEnter">
        <div class="ball" v-show="ball.show">
          <div class="inner">
            <div class="cubeic-add"></div>
          </div>
        </div>
      </transition>
    </div>-->
  </div>
</template>

<script>
import GoodsList from "@/components/GoodsList.vue";
import create from "@/components/create";
import BallAnim from "@/components/BallAnim.vue";
import { debuglog } from "util";

const labels = {
  fe: "前端",
  python: "Python",
  java: "Java",
  bigdata: "大数据",
  ai: "人工智能"
};

export default {
  name: "home",
  data() {
    return {
      slider: [],
      goods: [], // 所有商品列表
      selectedKeys: [], // 分类过滤时使用
      keys: [], // 分类
      ball: {
        show: false, // 显示控制
        el: null // 目标dom引用
      }
    };
  },
  components: {
    GoodsList
  },
  async created() {
    // {data:{},statsu...}

    const {
      data: { data: goods, slider, keys }
    } = await this.$http.get("/api/goods");
    // const res=await this.$http.get("/api/goods");
    console.log(goods, slider);
    this.slider = slider;
    this.goods = goods;
    this.keys = keys;
    this.selectedKeys = [...this.keys]; // 默认选中全部分类
  },
  methods: {
    showCatg($event) {
      // 显示分类列表
      this.$refs.drawer.show();
    },
    selectHandler(val) {
      this.selectedKeys = [...val];
    },
    onAddCart(el) {
      // this.ball.el = el;
      // this.ball.show = true; // 触发动画钩子
      // 手动创建组件实例
      const anim = create(BallAnim, { el });
      anim.start();
      anim.$on("transitionend", () => {
        anim.remove();
      });
    }
    // beforeEnter(el) {
    //   // 设置小球初始位置
    //   // 小球移动到点击的位置
    //   // 1. 获取点击的dom位置
    //   const dom = this.ball.el;

    //   const rect = dom.getBoundingClientRect();
    //   console.log(rect.top, rect.left);
    //   // 2. 把小球从终点移动到点击的位置
    //   const x = rect.left - window.innerWidth / 2;
    //   const y = -(window.innerHeight - rect.top - 10 - 20);
    //   el.style.display = "block";
    //   // ball只移动y
    //   // el.style.transform = `translate3d(${x}px, ${y}px, 0)`;
    //   el.style.transform = `translate3d(0, ${y}px, 0)`;
    //   const inner = el.querySelector(".inner");
    //   // inner只移动x
    //   inner.style.transform = `translate3d(${x}px,0,0)`;
    // },
    // enter(el, done) {
    //   // 把小球移动到初始位置 加上动画
    //   // 获取offsetHeight就会重绘，前面的变量名随意 主要为了eslint校验
    //   document.body.offsetHeight;
    //   // 动画开始，移动到初始位置
    //   // 小球移动到购物车位置
    //   el.style.transform = `translate3d(0, 0, 0)`;
    //   const inner = el.querySelector(".inner");
    //   inner.style.transform = `translate3d(0,0,0)`;
    //   el.addEventListener("transitionend", done);
    // },
    // afterEnter(el) {
    //   // 结束 隐藏小球
    //   this.ball.show = true;
    //   el.style.display = "true";
    //   // 派发事件
    //   this.$emit("transitionend");
    // }
  },
  computed: {
    filterGoods() {
      let ret = [];
      this.selectedKeys.forEach(v => {
        ret = ret.concat(this.goods[v]);
      });
      return ret;
    },
    drawerList() {
      //   var arr=[]
      // this.keys.map(v => {
      //   arr.push( {
      //     value: v,
      //     text: labels[v] //转换为中文标签
      //   })
      // });

      // return arr
      var arr = this.keys.map(v => {
        return {
          value: v,
          text: labels[v] //转换为中文标签
        };
      });
      return arr;
    }
  }
};
</script>

<style scoped lang="stylus">
.cube-slide {
  height: auto;
}

.cube-slide-item > a > img {
  width: 100%;
}

.ball-wrap {
  .ball {
    position: fixed;
    left: 50%;
    bottom: 10px;
    z-index: 100000;
    color: red;
    transition: all 0.5s cubic-bezier(0.49, -0.29, 0.75, 0.41);

    .inner {
      width: 16px;
      height: 16px;
      transition: all 0.5s linear;

      .cubeic-add {
        font-size: 22px;
      }
    }
  }
}
</style>
