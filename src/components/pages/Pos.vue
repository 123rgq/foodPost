<template>
    <div>
        <el-row>
            <el-col :span="7" class="pos-order" id="order-list">
                 <!-- 选项卡 -->
                 <!-- 左侧订单内容 -->
                <el-tabs>
                    <el-tab-pane label="点餐">
                        <!-- 表格内容 -->
                        <el-table :data="tableData" border show-summary style="width: 100%" >
                            <el-table-column prop="goodsName" label="商品"  ></el-table-column>
                            <el-table-column prop="count" label="量" width="50"></el-table-column>
                            <el-table-column prop="price" label="金额" width="70"></el-table-column>
                            <el-table-column  label="操作" width="100" fixed="right">
                                <template slot-scope="scope">
                                    <el-button type="text" size="small"
                                        @click="delSingGoods(scope.row)">删除</el-button>
                                    <el-button type="text" size="small"
                                        @click="addOrderList(scope.row)">增加</el-button>
                                </template>
                            </el-table-column>
                        </el-table>
                        <div class="order-btn">
                            <el-button type="warning" >挂单</el-button>
                            <el-button type="danger" @click="delAllGoods()">删除</el-button>
                            <el-button type="success" @click="checkout()">结账</el-button>
                        </div>
                    </el-tab-pane>
                    <el-tab-pane label="挂单">
                    挂单
                    </el-tab-pane>
                    <el-tab-pane label="外卖">
                    外卖
                    </el-tab-pane>
                </el-tabs> 
            </el-col>
            <el-col :span="17">
                <div class="often-goods">
                    <div class="title">常用内容</div>
                    <div class="often-goods-list">
                        <ul>
                            <li v-for="(item,index) in oftenGoods" :key="index">
                                <span>{{item.goodsName}}</span>
                                <span class="o-price">&yen;{{item.price | moneyFilter}}元</span>
                            </li>
                        </ul>
                    </div>

                </div>
                <!-- 内容分类 -->
                <div class="goods-type">
                    <el-tabs>
                        <el-tab-pane label="汉堡">
                            <ul class="cookList">
                                <li 
                                    v-for="(goods,index) in textGoods" :key="index"
                                    @click="addOrderList(goods)"
                                    >
                                    <span class="foodImg">
                                        <img :src="goods.goodsImg" width="100%">
                                    </span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">&yen;{{goods.price |moneyFilter}}元</span>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane label="汉堡">
                        </el-tab-pane>
                        <el-tab-pane label="汉堡">
                        </el-tab-pane>
                    </el-tabs>
                </div>
            </el-col>
        </el-row>
    </div>
</template>

<script>
    // 过滤钱
    import {toMoney} from '@/filter/moneyFilter.js'
    // 请求后台数据
    import axios from 'axios'
    export default {
        data (){
            return{
                // 表格
                tableData:[],
                // 常用商品
                oftenGoods:[],
                // 常用内容
                textGoods:[],
                totalCount:0,//订单商品的总数量
                totalMoney:0//订单商品的总价格
            }
        },
        components:{

        },
        filters:{
            moneyFilter(money){
                return toMoney(money)
            }
        },
        methods: {
            // 添加左侧列表的行数据
            addOrderList(goods){
                this.totalCount = 0;//汇总数量清0
                this.totalMoney = 0;
                let isHave = false;
                // 判断这个内容是否存在左侧列表
                for(let i = 0; i < this.tableData.length; i++){
                    console.log(this.tableData[i].goodsId);
                    if(this.tableData[i].goodsId == goods.goodsId){
                        isHave = true; //存在
                    }
                }
                // 根据isHave的值判断左侧列表是否有此内容
                if(isHave){
                    // 存在就进行数量添加
                    let arr = this.tableData.filter(o => o.goodsId == goods.goodsId);
                    arr[0].count++;
                    arr[0].price += arr[0].price
                }else{
                    // 不存在数量添加
                    let newGoods = {
                        goodsId: goods.goodsId,
                        goodsName: goods.goodsName,
                        price:goods.price,
                        count: 1
                    };
                    this.tableData.push(newGoods);
                }
                // 进行数量和金额汇总
                // this.tableData.forEach((element) => {
                //     this.totalCount += element.count;
                //     this.totalMoney =this.totalMoney + (element.price*element.count)
                // });
                this.getAllMoney();
            },
            // 删除左侧列表的对应列表数据
            delSingGoods(goods) {
                console.log(goods);
                this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId);
                this.getAllMoney();
            },
            // 删除所有数据
            delAllGoods() {
                this.tableData = [];
                this.totalMoney = 0;
                this.totalMoney = 0;
            },
            // 汇总数量和金额
            getAllMoney() {
                this.totalCount = 0;
                this.totalMoney = 0;
                this.tableData.forEach((element) => {
                    this.totalCount += element.count;
                    // this.totalMoney = this.totalMoney + (element.price * element.count);
                    this.totalMoney = this.totalMoney + (element.price * element.count);
                })
            },
            // 合计方法模拟
            checkout() {
                console.log(this.totalCount + '---' + 'totalCount');
                if(this.totalCount != 0){
                    this.tableData = [];
                    this.totalCount = 0;
                    this.totalMoney = 0;
                    // this.$message({
                    //     message:'合算成功，感谢参与',
                    //     type:'success'
                    // })
                    this.$message.success('合算成功,感谢参与')
                }else{
                    // this.$message({
                    //     message:'不能空合算,请选择右侧内容',
                    //     type:'error'
                    // });
                    this.$message.error('不能空合算,请选择右侧内容')
                }
            }
        },
        mounted:function(){
            var orderHeight = document.body.clientHeight;
            document.getElementById('order-list').style.heigt = orderHeight + 'px'
        },
        created(){
            axios({
                url:'https://www.easy-mock.com/mock/5c3d540d4352732b5c26f28d/elefoodpos',
                method:'get'
            }).then(response => {
                console.log(response);
                if(response.status == 200){
                    // 常见内容
                    console.log(response.data.data.textGoods);
                    this.textGoods = response.data.data.textGoods
                    this.tableData = response.data.data.tableData
                    this.oftenGoods = response.data.data.oftenGoods
                }
            }).catch(error => {
                console.log(error);
                alert('网络错误，不能访问');
            })
        }
    }
</script>

<style scoped>
    .pos {
    font-size: 12px;
}

.pos-order {

    background-color: #F9FAFC;
    border-right: 1px solid #C0CCDA;
}

.order-btn {
    margin-top: 10px;
    text-align: center;
}

.title {
    height: 20px;
    border-bottom: 1px solid #D3DCE6;
    background-color: #F9FAFC;
    padding: 10px;
}

.often-goods-list ul li {
    list-style: none;
    float: left;
    border: 1px solid #E5E9F2;
    padding: 10px;
    margin: 5px;
    background-color: #fff;
    cursor: pointer;
}

.goods-type {
    clear: both;
}

.o-price {
    color: #58B7FF;
}


.often-goods-list {
    border-bottom: 1px solid #C0CCDA;
    height: auto;
    overflow: hidden;
    padding-bottom: 10px;
    background-color: #F9FAFC;
}

.cookList li {
    list-style: none;
    width: 23%;
    border: 1px solid #E5E9F2;
    height: auot;
    overflow: hidden;
    background-color: #fff;
    padding: 2px;
    float: left;
    margin: 2px;
    cursor: pointer;
}

.cookList li span {

    display: block;
    float: left;
}

.foodImg {
    width: 40%;
}

.foodName {
    font-size: 18px;
    padding-left: 10px;
    color: brown;
}

.foodPrice {
    font-size: 16px;
    padding-left: 10px;
    padding-top: 10px;
}

.totalDiv {
    height: auot;
    overflow: hidden;
    text-align: right;
    font-size: 16px;
    background-color: #fff;
    border-bottom: 1px solid #E5E9F2;
    padding: 10px;
}
</style>