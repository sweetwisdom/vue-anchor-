<template>
  <div>
    <div class="card-anchor" :style="position">
      <div class="titles cc">
        <ul>
          <li
            v-for="(item, i) in anchorList"
            @click="toToc(item)"
            :key="i"
            :class="{ isSelect: active == i }"
          >
            {{ item.name }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import { on, off } from "./dom";
export default {
  data() {
    return {
      active: 0,
      scrollContainer: null,
    };
  },
  props: {
    container: {
      default: "",
    },
    /**
     * 目录列表
     */
    anchorList: {
      type: Array,
      default: () => {
        return [];
      },
    },
    /**
     * 检测滚动元素的className
     */
    anchorClass: {
      default: "articles",
    },
    /**
     * 定义垂直方向的对齐， "start", "center", "end", 或 "nearest"之一。默认start
     */
    scorllType: {
      default: "block",
    },
    /**
     * 位置
     */
    position: {
      default: () => {
        return { top: "60px", right: "0" };
      },
    },
  },
  watch: {},
  computed: {},
  mounted() {
    this.init();
    // window.addEventListener("scroll", this.onScroll);
  },
  created() {},
  beforeDestroy() {
    this.removeListener();
  },

  methods: {
    getContainer() {
      this.scrollContainer = this.container
        ? typeof this.container === "string"
          ? document.querySelector(this.container)
          : this.container
        : window;
      this.scrollElement = this.container
        ? this.scrollContainer
        : document.documentElement || document.body;
    },
    //   scroll函数
    onScroll(e) {
      const navContents = document.querySelectorAll("." + this.anchorClass);

      const offsetTopArr = [];

      navContents.forEach((item) => {
        offsetTopArr.push(item.offsetTop);
      });

      //   let aa = document.getElementById("article");
      //   const scrollTop = aa.pageYOffset;

      const scrollTop =
        e.target.scrollTop ||
        window.pageYOffset ||
        document.documentElement.scrollTop ||
        document.body.scrollTop; // 滚动条偏移量
      if (!scrollTop) {
        return;
      }

      let navIndex = 0;
      for (let n = 0; n < offsetTopArr.length; n++) {
        if (scrollTop >= offsetTopArr[n]) {
          navIndex = n;
        }
      }

      this.active = navIndex;
    },
    // 跳转到指定锚点
    toToc(a) {
      let as = "#" + a.name;

      document.querySelector(as).scrollIntoView({
        behavior: "smooth", // 平滑过渡
        block: "center",
      });
    },
    init() {
      this.$nextTick(() => {
        this.removeListener();
        this.getContainer();
        on(this.scrollContainer, "scroll", this.onScroll);
      });
    },
    removeListener() {
      off(this.scrollContainer, "scroll", this.onScroll);
    },
  },
};
</script>

<style  scoped>
.card-anchor {
  position: fixed;
  width: 120px;

  top: 30px;
  background: rgba(245, 245, 245, 0.548);
  border-radius: 10px;
  text-align: center;
}

.card-anchor .isSelect {
  background-color: #ff0000;
  color: white;
}

.card-anchor li {
  line-height: 2.5rem;
  border-radius: 10px;
}

.card-anchor li:hover {
  background-color: #ff0000;
  color: white;
}
</style>