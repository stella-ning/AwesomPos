<template>
    <div class="pos">
        <el-row>
            <!--左边-->
            <el-col :span=7 class="posOrder" id="orderList">
                <el-tabs>
                    <el-tab-pane  label="点餐">
                        <el-table :data="tableData" border show-summary style="width:100%">
                            <el-table-column prop="goodsName" label="商品名称"></el-table-column>
                            <el-table-column prop="count" label="数量"></el-table-column>
                            <el-table-column prop="price" label="金额"></el-table-column>
                            <el-table-column  label="操作" fixed="right">
                                <template scope="scope">
                                    <el-button type="text" size="small" @click="delOrderList(scope.row)">删除</el-button>
                                    <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                                </template>
                            </el-table-column>
                        </el-table>
                        <div class="divBtn">
                            <el-button type="warning" >挂单</el-button>
                            <el-button type="danger" @click="delAllGoods">删除</el-button>
                            <el-button type="success" @click="checkout">结账</el-button>
                        </div>
                    </el-tab-pane>
                    <el-tab-pane  label="挂单">

                    </el-tab-pane>
                    <el-tab-pane  label="外卖">

                    </el-tab-pane>
                </el-tabs>
            </el-col>
            <!--右边商品栏-->
            <el-col :span=17>
                <div class="oftenGoods">
                    <div class="title">
                        常用商品
                    </div>
                    <div class="oftenGoodsList">
                        <ul class="clearfix">
                            <li v-for="item in oftenGoods" @click="addOrderList(item)">
                                <span>{{item.goodsName}}</span>
                                <span class="proPrice">￥{{item.price}}</span>
                            </li>
                        </ul>
                    </div>
                    <div class="goodsType">
                        <el-tabs>
                            <el-tab-pane label="汉堡" color="#fff">
                                <ul class="cookList clearfix">
                                    <li v-for="goods in type0Goods" @click="addOrderList(goods)">
                                        <span class="foodImg">
                                            <img :src="goods.goodsImg"  width="100%" alt="">
                                        </span>
                                        <div class="foodRight">
                                            <span class="foodName">
                                                {{goods.goodsName}}
                                            </span>
                                                <span class="foodPrice">
                                                ￥{{goods.price}}
                                            </span>
                                        </div>

                                    </li>
                                </ul>
                            </el-tab-pane>
                            <el-tab-pane label="小食">
                                <ul class="cookList clearfix">
                                    <li v-for="goods in type1Goods" @click="addOrderList(goods)">
                                        <span class="foodImg">
                                            <img :src="goods.goodsImg"  width="100%" alt="">
                                        </span>
                                        <div class="foodRight">
                                            <span class="foodName">
                                                {{goods.goodsName}}
                                            </span>
                                            <span class="foodPrice">
                                                ￥{{goods.price}}
                                            </span>
                                        </div>

                                    </li>
                                </ul>
                            </el-tab-pane>
                            <el-tab-pane label="饮料">
                                <ul class="cookList clearfix">
                                    <li v-for="goods in type2Goods" @click="addOrderList(goods)">
                                        <span class="foodImg">
                                            <img :src="goods.goodsImg"  width="100%" alt="">
                                        </span>
                                        <div class="foodRight">
                                            <span class="foodName">
                                                {{goods.goodsName}}
                                            </span>
                                            <span class="foodPrice">
                                                ￥{{goods.price}}
                                            </span>
                                        </div>

                                    </li>
                                </ul>
                            </el-tab-pane>
                            <el-tab-pane label="套餐">
                                <ul class="cookList clearfix">
                                    <li v-for="goods in type3Goods" @click="addOrderList(goods)">
                                        <span class="foodImg">
                                            <img :src="goods.goodsImg"  width="100%" alt="">
                                        </span>
                                        <div class="foodRight">
                                            <span class="foodName">
                                                {{goods.goodsName}}
                                            </span>
                                            <span class="foodPrice">
                                                ￥{{goods.price}}
                                            </span>
                                        </div>

                                    </li>
                                </ul>
                            </el-tab-pane>
                        </el-tabs>
                    </div>
                </div>
            </el-col>
        </el-row>
    </div>
</template>

<script>
    import axios from 'axios';

    export default {
        name: 'pos',
        data(){
            return {
                tableData:[],
                oftenGoods:[],
                type0Goods:[],
                type1Goods:[],
                type2Goods:[],
                type3Goods:[]
            }
        },
        created:function(){
            //常用商品
            axios.get('http://jspang.com/DemoApi/oftenGoods.php')
                .then(response => {
                    console.log(response);
                    this.oftenGoods = response.data;
                })
                .catch(error => {
                    console.log(error);
                    alert('网络错误，不能访问')
                })

            //商品分类
            axios.get('http://jspang.com/DemoApi/typeGoods.php')
                .then(res => {
                    console.log(res);
                    this.type0Goods = res.data[0];
                    this.type1Goods = res.data[1];
                    this.type2Goods = res.data[2];
                    this.type3Goods = res.data[3];

                })
                .catch(error => {
                    console.log(error)
                    alert('网络错误，不能访问')
                })


        },
        mounted:function(){
            var orderHeight = document.body.clientHeight;
            document.getElementById('orderList').style.height = orderHeight +'px';
        },
        methods:{
            addOrderList(goods){
                //判断是否这个商品已经存在于订单列表
                let isHave = false;
                for(let i=0;i<this.tableData.length;i++){
                    if(this.tableData[i].goodsId == goods.goodsId){
                        //存在
                        isHave = true;
                    }
                }
                //根据isHave的值判断订单列表中是否已经有此商品
                if(isHave){
                    //存在就进行数量添加
                    let arr = this.tableData.filter(o => o.goodsId == goods.goodsId);
                    arr[0].count++
                }else{
                    //不存在
                    let newGoods = {
                        goodsId:goods.goodsId,
                        goodsName:goods.goodsName,
                        price:goods.price,
                        count:1
                    }
                    this.tableData.push(newGoods)
                }
            },
            //删除单个商品
            delOrderList(goods){
                this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId);
            },
            //全部删除
            delAllGoods(){
                this.tableData = []
            },
            //结账
            checkout(){
                if(this.tableData.length != 0){
                    this.tableData = [];
                    this.$message({
                        message:'结账成功，感谢你又为店里出了一份力!',
                        type:'success'
                    })
                }else{
                    this.$message.error('不能空结。老板了解你急切的心情！')
                }
            }
        }
    }

</script>

<style scoped lang="less">
    .posOrder{
        background:#f9fafc;
        border-right:1px solid #c0ccda;
    }
    .divBtn{
        margin-top:10px;
    }
    .oftenGoods{
        .title{
            height:20px;
            border:1px solid #d3dce6;
            background:#f9fafc;
            padding:10px;
            text-align:left;
        }
        .oftenGoodsList {
            background:#eff2f7;
            padding:0 20px;
        }
        .oftenGoodsList ul li{
            list-style: none;
            float:left;
            border:1px solid #e5e9f2;
            padding:10px;
            margin:20px;
            background-color:#fff;

            .proPrice{
                color:#58b7ff;
            }
        }
    }
    .goodsType{
        .cookList{
            background: #eff2f7;
        }
        .cookList li{
            list-style: none;
            width:23%;
            border:1px solid #E5E9F2;
            height: auot;
            overflow: hidden;
            background-color:#fff;
            padding: 2px;
            float:left;
            margin: 2px;
            span{
                display: block;
                &.foodImg{
                    width:40%;
                    float:left;
                }
                &.foodName{
                    font-size:18px;
                    padding-left:10px;
                    color:brown;
                }
                &.foodPrice{
                    font-size:16px;
                    padding-lef:10px;
                    padding-top:10px;
                }
            }
        }
    }
</style>
