<template>
    <div id="cart">
        <div class="cart-title">
            <span>购物车</span>
        </div>
        <div class="cart-list">
            <ul class="list-title">
                <li><el-checkbox size="mini" v-model="checkedAll" @change="changeAllCheckedStatus">全选</el-checkbox></li>
                <li>商品信息</li>
                <li>单价</li>
                <li>数量</li>
                <li>金额</li>
                <li>操作</li>
            </ul>
            <div class=list-body>
                <ul class="list-body-item" v-for="(cart, index) in cartList" v-bind:key="index">
                    <li><el-checkbox size="mini" v-model="cart.checked" @change="changeCheckedStatus"></el-checkbox></li>
                    <li class="media">
                        <div class="media-img"><img :src="cart.picture"></div>
                        <div class="media-info"><span>{{ cart.title }}</span></div>
                    </li>
                    <li>￥{{ cart.price }}</li>
                    <li><el-input-number :min="cart.min" size="mini" v-model="cart.count"></el-input-number></li>
                    <li>￥{{ cart.count * cart.price }}</li>
                    <li><i class="el-icon-delete remove" @click="removeGoods(index)"></i></li>
                </ul>
            </div>
        </div>
        <div class="cart-accounts">
            <span>总金额：￥{{ allPrice }}</span>
            <el-button type="danger">结算</el-button>
        </div>
    </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Cart",
  data() {
    return {
      checkedAll: false,
      cartList: null,
      totalPrice: 0
    };
  },
  computed: {
    allPrice: function() {
      let _this = this;
      let totalPrice = 0;
      if (!this.cartList) return totalPrice;
      _this.cartList.forEach(element => {
        if (element.checked) totalPrice += element.price * element.count;
      });
      return totalPrice;
    }
  },
  methods: {
    getCartList: function() {
      let _this = this;
      axios.get("./static/data/cart.json", { id: 123 }).then(function(res) {
        _this.cartList = res.data.cartList;
      });
    },
    changeAllCheckedStatus: function() {
      let _this = this;
      _this.cartList.forEach(function(element) {
        element.checked = _this.checkedAll;
      });
    },
    changeCheckedStatus: function() {
      let _this = this;
      let length = 0;
      _this.cartList.forEach(function(element) {
        if (element.checked) length += 1;
      });
      if (length === _this.cartList.length) {
        _this.checkedAll = true;
      } else {
        _this.checkedAll = false;
      }
    },
    removeGoods: function(index) {
      this.cartList.splice(index, 1);
    }
  },
  mounted: function() {
    let _this = this;
    this.$nextTick(() => _this.getCartList());
  }
};
</script>

<style>
.remove {
  color: #f56c6c;
  cursor: pointer;
}
.cart {
  padding: 60px 0;
}
.cart-title {
  position: relative;
  height: 30px;
  font-size: 24px;
  color: #303133;
  font-weight: 600;
}
.cart-title:before {
  display: block;
  content: "";
  position: absolute;
  left: 0;
  right: 0;
  border-top: 1px solid #eee;
  top: 50%;
}
.cart-title span {
  background: #fff;
  position: relative;
  padding: 0 20px;
}
.list-body {
  border: 1px solid #eee;
}
.list-title,
.list-body-item {
  list-style: none;
  display: flex;
  padding-left: 0;
}
.list-body-item {
  border-bottom: 1px solid #eee;
  padding: 20px 0;
  margin: 0;
}
.list-body-item:last-child {
  border-bottom: 0;
}
.list-title {
  color: #606266;
  line-height: 50px;
}
.list-title li,
.list-body-item li {
  width: 15%;
}
.list-title li {
  background: #f5f7fa;
}
.list-title li:nth-child(2),
.list-body-item li:nth-child(2) {
  flex-grow: 1;
}
.media {
  display: flex;
}
.media-img {
  padding-right: 15px;
}
.media-img img {
  width: 80px;
  height: auto;
  max-height: 80px;
  border: 1px solid #eee;
}
.cart-accounts {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  padding: 20px;
}
.cart-accounts span {
  padding-right: 20px;
}
</style>