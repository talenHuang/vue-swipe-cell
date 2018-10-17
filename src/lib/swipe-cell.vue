<template>

  <div class="h-cell-swipe">

    <a @touchstart="touchStart" @touchmove="touchMove" @touchend="touchEnd">

      <div class="cell-content">
        <span>{{title}}</span>

        <div class="right-btn" ref="rightBtn" @touchstart.stop>

          <div class="action-btn">
            <slot>

              <button @click="clickHander" class="del-span">删除</button>

            </slot>

          </div>

        </div>

      </div>

    </a>

  </div>
</template>

<script>
export default {
  name: "swipe-cell",
  data() {
    return {
      start_position: [],
      end_position: [],
      right_item: {},
      standardValue: 60,
      swiping: false,
      opening: false
    };
  },
  props: {
    title: { type: String }
  },
  mounted() {
    this.right_item = this.$refs.rightBtn;
    this.initRightItem();
  },

  methods: {
    clickHander(e) {
      this.$emit("del", e);
    },

    // 动态改变translate值
    addTraslate(value) {
      return `translate3d(${value}px,0,0)`;
    },

    // 初始化右边按钮操作部分
    initRightItem() {
      this.right_item.style.webkitTransform = this.addTraslate(
        Math.ceil(this.right_item.getBoundingClientRect().width)
      );
    },

    // 寻找父节点
    findParentsNode(element) {
      if (
        element.nodeName.toLowerCase() === "a" ||
        element.nodeName.toLowerCase() === "body"
      ) {
        return element;
      } else {
        return this.findParentsNode(element.parentNode);
      }
    },

    // 向左滑执行函数
    swipeToLeft(ele, scale) {
      let current_btn_width = 0;
      current_btn_width =
        scale ||
        ele.getElementsByClassName("right-btn")[0].getBoundingClientRect()
          .width;

      current_btn_width = Math.ceil(current_btn_width);

      ele.style.webkitTransform = this.addTraslate(-current_btn_width);
    },
    // 向右滑执行函数
    swipeToRight(ele, scale) {
      let current_btn_width = 0;
      current_btn_width = scale || 0;

      ele.style.webkitTransform = this.addTraslate(current_btn_width);
    },

    touchStart(e) {
      this.start_position = [
        e.changedTouches[0].clientX,
        e.changedTouches[0].clientY
      ];
      let ele = this.checkSwipeDom(this.findParentsNode(e.target));

      // 滑块已经向左滑完如果触碰直接还原
      if (this.opening && !this.swiping) {
        this.opening = false;
        this.initAllCell();
        this.handerByDirection("right")(ele);
      }
    },
    touchMove(e) {
      let pre_position = this.start_position,
        current_position = [
          e.targetTouches[0].clientX,
          e.targetTouches[0].clientY
        ],
        scale = current_position[0] - pre_position[0];

      // 滑动距离
      let swipe_scale =
        Math.abs(scale) > this.standardValue
          ? scale > 0 ? this.standardValue : -this.standardValue
          : scale;

      let ele = this.checkSwipeDom(this.findParentsNode(e.target));
      let swipe_area_width = Math.ceil(
        ele.getElementsByClassName("right-btn")[0].getBoundingClientRect().width
      );

      if (Math.abs(swipe_scale) >= swipe_area_width) {
        swipe_scale = swipe_scale >= 0 ? swipe_area_width : -swipe_area_width;
      }

      // 正在滑动的时候
      if (!this.opening && swipe_scale < 0) {
        this.swiping = true;
        this.handerByDirection("left")(ele, -swipe_scale);
      }
    },
    touchEnd(e) {
      this.end_position = [
        e.changedTouches[0].clientX,
        e.changedTouches[0].clientY
      ];

      this.checkSwipeDirection(e.target);

      this.swiping = false;
    },

    // 确认滑动的dom
    checkSwipeDom(ele) {
      return ele.getElementsByClassName("cell-content")[0];
    },

    // 所有cell还原
    initAllCell() {
      let arr = Array.from(document.getElementsByClassName("cell-content"));
      arr.forEach(ele => {
        ele.style.webkitTransform = this.addTraslate(0);
      });
    },

    // 判断滑动方向
    checkSwipeDirection(ele) {
      let x_start = this.start_position[0],
        x_end = this.end_position[0],
        y_start = this.start_position[1],
        y_end = this.end_position[1],
        abs_scale = Math.abs(x_start - x_end);

      ele = this.checkSwipeDom(this.findParentsNode(ele));

      if (x_start - x_end > 0) {
        if (abs_scale > this.standardValue) {
          // 初始化已滑动cell
          this.initAllCell();
          this.opening = true;
          return this.handerByDirection("left")(ele);
        } else if (this.swiping) {
          if (abs_scale > this.standardValue / 2) {
            this.opening = true;
            this.initAllCell();
            return this.handerByDirection("left")(ele);
          } else {
            this.opening = false;
            return this.handerByDirection("right")(ele);
          }
        }
      } else {
        if (abs_scale > this.standardValue) {
          this.opening = false;
          return this.handerByDirection("right")(ele);
        } else if (this.swiping) {
          if (abs_scale < this.standardValue) {
            this.opening = false;
            return this.handerByDirection("right")(ele);
          }
        }
      }
    },

    // 通过滑动方向控制方法
    handerByDirection(direction) {
      let obj = {
        left: this.swipeToLeft,
        right: this.swipeToRight
      };

      return obj[direction];
    }
  }
};
</script>

<style lang="scss" scoped>
.h-cell-swipe {
  width: 100%;
  overflow: hidden;
  font-size: 14px;

  a {
    display: block;
    position: relative;
    line-height: 15px;
    border-bottom: 1px solid #eeeeee;

    .cell-content {
      transform: translate3d(0, 0, 0);
      transition: transform 0.2s linear;
      padding: 15px;
    }

    .right-btn {
      display: flex;
      align-items: center;
      justify-content: center;
      height: 100%;
      position: absolute;
      // background: #f00;
      right: 0;
      top: 0;

      .action-btn {
        height: 100%;
        display: inherit;

        button {
          height: 100%;
          color: #eeeeee;
          padding: 0px 30px;
          border: 0;
          background: red;
        }
      }
    }
  }
}
</style>


