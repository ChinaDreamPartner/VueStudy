<template>
  <div class="app-container">
    <es-header title="购物车案例"></es-header>
    <es-goods
      v-for="item in goodslist"
      :key="item.id"
      :id="item.id"
      :thumb="item.goods_img"
      :title="item.goods_name"
      :price="item.goods_price"
      :count="item.goods_count"
      :checked="item.goods_state"
      @stateChange="onGoodsStateChange"
      @countChange="onGoodsCountChange"
    ></es-goods>

    <es-footer
      :total="total"
      :amount="amount"
      @fullChange="onFullStateChange"
    ></es-footer>
  </div>
</template>

<script>
import EsHeader from "./components/EsHeader.vue";
import EsFooter from "./components/EsFooter.vue";
import EsGoods from "./components/EsGoods.vue";
export default {
  name: "MyApp",
  data() {
    return {
      // 商品列表的数据
      goodslist: [],
    };
  }, // 组件实例创建完毕之后的生命周期函数
  created() {
    // 调用 methods 中的 getGoodsList 方法，请求商品列表的数据
    this.getGoodsList();
  },
  methods: {
    // 请求商品列表的数据
    async getGoodsList() {
      // 1. 通过组件实例 this 访问到全局挂载的 $http 属性，并发起Ajax 数据请求
      const { data: res } = await this.$http.get("/api/cart");
      // 2. 判断请求是否成功
      if (res.status !== 200) return alert("请求商品列表数据失败！");
      // 3. 将请求到的数据存储到 data 中，供页面渲染期间使用
      this.goodslist = res.list;
    },
    onFullStateChange(isFull) {
      this.goodslist.forEach((x) => (x.goods_state = isFull));
    },
    // 监听商品选中状态变化的事件
    onGoodsStateChange(e) {
      const findResult = this.goodslist.find((x) => x.id === e.id);
      if (findResult) {
        findResult.goods_state = e.value;
      }
    },
    onGoodsCountChange(e) {
      // 根据 id 进行查找
      const findResult = this.goodslist.find((x) => x.id === e.id);
      // 找到了对应的商品，则更新其数量
      if (findResult) {
        findResult.goods_count = e.value;
      }
    },
  },

  computed: {
    amount() {
      // 1. 定义商品总价格
      let a = 0;
      // 2. 循环累加商品总价格
      this.goodslist
        .filter((x) => x.goods_state)
        .forEach((x) => {
          a += x.goods_price * x.goods_count;
        });
      // 3. 返回累加的结果
      return a;
    },
    total() {
      let t = 0;
      this.goodslist
        .filter((x) => x.goods_state)
        .forEach((x) => {
          t += x.goods_count;
        });
      return t;
    },
  },

  components: {
    EsHeader,
    EsFooter,
    EsGoods,
  },
};
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
