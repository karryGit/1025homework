# 1025homework
Vue实现简易购物车

```
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>shopCar</title>
    <link rel="stylesheet" href="css/shopCar.css">
    <script src="node_modules/vue/dist/vue.js"></script>
</head>
<body>
<div id="app">
    <!--组件部分 通过v-for循环遍历 把index值传给属性 通过自定义事件将组件信息传给 实例属性计算中-->
    <shopping id="shop"
            v-for="(good, index) in goods"
            :price="good.price"
            :index="index"
            :key="index"
            @pass="total">
    </shopping>
    <p class="total">总价:{{totalPrice}}</p>
</div>
<script>


    var goods = [
        {price: 10, count: 0},
        {price: 20, count: 0},
        {price: 30, count: 0},
        {price: 40, count: 0}
    ]
    //定义全局组件
    Vue.component('Shopping', {
        //定义模板
        template: `<div>
                          <p>商品单价:{{price}}</p>
                          <button @click="sub">-</button>
                          <p>{{count}}件</p>
                          <button @click="add">+</button>
                  </div>`,
        //定义模板属性
        props: ['price', 'index'],
        //定义组件数据
        data: function () {
            return {
                count: 0
            }
        },
        //定义组件方法
        methods: {
            //增加方法
            add: function () {
                this.count++;
                this.$emit('pass', this.count, this.index)
            },
            //减少方法
            sub: function () {
                if (this.count <= 0) {
                    this.count = 0;
                    return;
                }
                this.count--;
                this.$emit('pass', this.count, this.index)
            }
        }
    });
    //Vue实例化
    new Vue({
        //获取dom
        el: '#app',
        //传递数据
        data: {
            //定义数组
            goods: goods
        },
        //计算总价属性
        computed: {
            totalPrice:function () {
                //定义总价变量初始为0
                var total = 0;
                //通过循环遍历数组来获取数组中的价格和点击数量
                for (var i = 0; i < this.goods.length; i++) {
                    //用总价加上(商品数量和单价)
                    total += this.goods[i].count * this.goods[i].price;
                }
                //将结果返回给totalPrice
                return total;
            }
        },
        //定义方法
        methods: {
            //获取组件自定义事件传递的点击数量和下标
            total: function (count,index) {
                //将组件传来的数量和下标赋给数组中的count
                this.goods[index].count = count;

            }
        }
    })
</script>
</body>
</html>
```

***

