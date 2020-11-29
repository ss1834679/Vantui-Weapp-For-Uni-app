<template>
  <view v-title="'心海无忧'">
    <!-- <van-search :value="word" placeholder="请输入搜索关键词" @change="handleSearch" /> -->
    <van-search v-model="word" placeholder="输入医院名称搜索" clearable autofocus @change="handleSearch" @clear="onClear" />
    {{word}}
    <!-- <van-cellgroup v-if="list.length" title="搜索结果">
      <van-cell v-for="item in list" :key="item.id" :title="item.name" value="进入" is-link clickable @click="clickSearchResult(item)"></van-cell>
    </van-cellgroup>-->
  </view>
</template>

<script>
export default {
  data() {
    return {
      word: "",
      list: [{ id: 2, name: "南充市身心医院" }]
    };
  },
  // mounted() {
  //   this.init();
  // },
  methods: {
    // 初始化：如果已经选择过直接跳过
    init() {
      //   const unitid = this.$store("unit-id");
      const unitid = uni.getStorage({ key: "unit-id" });
      unitid && uni.redirectTo({ url: `/pages/login?unit=${unitid}` });
    },
    onClear() {
      wx.showToast({
        title: "取消",
        icon: "none"
      });
    },
    handleSearch(event) {
      this.word = event.detail;
      console.log(this.word);
      console.log(123);
    },
    // 查询
    // handleSearch: debounce(
    //   function() {
    //     console.log(123);
    //     const word = this.word;
    //     if (word.length >= 2) {
    //       this.$http.get("/table/5", { f: "id,name", w: { name: ["like", "%" + word + "%"] } }).then(json => {
    //         if (json.code === 0) {
    //           this.list = json.data;
    //         }
    //       });
    //     } else {
    //       // this.list = [];
    //     }
    //   },
    //   500,
    //   false
    // ),
    // 点击结果
    clickSearchResult(item) {
      this.$dialog
        .confirm({ message: "系统将记住你的选择，下次会跳过搜索，直接进入【" + item.name + "】登录页，确认吗？" })
        .then(() => {
          //   this.$store("unit-id", item.id);
          uni.setStorage({ key: "unit-id", data: item.id });
          this.$router.replace("/" + item.id + "/login");
        });
    }
  }
};

// 防抖
function debounce(func, wait = 20, immediate = true) {
  let timeout = null;
  return function() {
    const context = this;
    const args = arguments;
    const later = function() {
      timeout = null;
      !immediate && func.apply(context, args);
    };
    const callNow = immediate && !timeout;
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
    callNow && func.apply(context, args);
  };
}
</script>

<style lang="scss" scoped>
</style>