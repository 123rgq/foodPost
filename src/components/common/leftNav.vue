<template>
    <div class="left-nav">
        <!-- 左侧导航 -->
         <ul>
                <li>
                    <i class="icon iconfont icon-wodezichan"></i>
                    <div>首页</div>
                </li>
                <li>
                    <i class="icon iconfont icon-dianpu"></i>
                    <div>店铺</div>
                </li>
                <li>
                    <i class="icon iconfont icon-hanbao"></i>
                    <div>内容列表</div>
                </li>
                <li>
                    <i class="icon iconfont icon-huiyuanqia"></i>
                    <div>个人中心</div>
                </li>
                <li>
                    <i class="icon iconfont icon-tongji"></i>
                    <div>统计</div>
                </li>
        </ul>
    </div>
</template>

<script>
    export default {
        name:'left-nav'
    }
</script>

<style scoped>
    .left-nav{
       color:#fff;
       font-size:10px;
       height:100%;
       background-color: #1D8ce0;
       float:left;
       width:5%;
    }
    .iconfont{
       font-size:24px;
    }
    .left-nav ul{
        padding:0px;
        margin: 0px;
    }
    .left-nav li{
        list-style: none;
        text-align: center;
        border-bottom:1px solid #20a0ff;
        padding:10px;
    }
</style>