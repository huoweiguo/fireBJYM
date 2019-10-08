<template>
    <div id="app">
        <div class="body-mask"></div>
        <navigation>
            <a href="/app-huopan/goOrder" slot="navigation_goback" class="navigation_goback"></a>
            <span slot="navigation_title" class="navigation_title">还款</span>
        </navigation>
        <div class="orderNav">
            <span>未还总额(元)</span>
            <h2>
                {{amt}}
            </h2>
            <span>共计<em>{{unclearCount}}笔</em>未结清</span>
        </div>
        <div class="orderTitle">
            账单明细
        </div>
        <div class="playList" v-for="(item,index) in planList" :key="index">
            <h2 class="orderEnd" v-if="item.status == 'P'" >
                {{item.repayDate}}
                <span>锁定中<img src="../../../static/images/rightarrow_1f.png"/></span>
            </h2>
            <h2 class="orderEnd" v-if="item.status == 'S'" @click="goOrderDetailOver(item.sysSeqId)">
                {{item.repayDate}}
                <span>已结清<img src="../../../static/images/rightarrow_1f.png"/></span>
            </h2>
            <h2 class="orderOver" v-if="item.status == 'W' && item.overdueStatus == 'O'" @click="goRepayDetail(item.sysSeqId)">
                {{item.repayDate}}
                <span>{{item.shouldRepayAmt}}元<img src="../../../static/images/rightarrow_1f.png"/></span>
            </h2>
            <h2 @click="goRepayDetail(item.sysSeqId)" v-if="item.status == 'W' && item.overdueStatus == 'I'">
                {{item.repayDate}}
                <span>{{item.shouldRepayAmt}}元<img src="../../../static/images/rightarrow_1f.png"/></span>
            </h2>
        </div>
    </div>
</template>

<script>
import navigation from '../../components/navigation.vue';
import xmy from '../../../static/js/xmy.js';
export default {
    components: {
        navigation: navigation
    },
     data () {
        return {
            token: xmy.getQueryString('token'),
            userId: xmy.getQueryString('userId'),
            planList: [],
            amt: xmy.getQueryString('amt'),
            unclearCount: '',
            orderId: xmy.getQueryString('orderId'),
            merchantId: xmy.getQueryString('merchantId'),
            productId: xmy.getQueryString('productId')
        }
    },
    methods: {
        // 得到还款计划
        getPlan(){
            var _this = this;
            xmy.getData({
                url: '/gateway/postloan/loan/queryBillRepayPlanList',
                type: 'POST',
                contentType: "application/json",
                data: JSON.stringify({
                    token: _this.token,
                    billOrderId: _this.orderId
                }),
                success:function(res){
                    if(res.respCode == '000000'){
                        _this.unclearCount = res.data.unclearCount;
                        _this.planList = res.data.repayPlanRoughList;
                    }else{
                        xmy.toast(res.respMsg);
                    }
                }
            })
        },
        // 进去还款计划详情
        goRepayDetail (repayPlanId) {
            let _this = this;
            window.location.href = "/bjym/module/repayDetail.html?token="+_this.token+"&userId="+_this.userId+"&orderId="+_this.orderId +'&repayPlanId='+ repayPlanId+"&merchantId="+ _this.merchantId+"&productId="+_this.productId;
        },
        // 进入已还款计划详情
        goOrderDetailOver (orderId) {
            let _this = this;
            window.location.href = "/bjym/module/orderDetail.html?token="+_this.token+"&userId="+_this.userId+"&orderId="+orderId+"&merchantId="+ _this.merchantId;
        },
    },
    mounted () {
        this.getPlan();
    }  
}
</script>

<style lang="less" scoped>
    @import '../../common/css/orderInstalment.less';
</style>

