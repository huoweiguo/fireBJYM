<template>
    <div id="app">
        <div class="body-mask"></div>
        <navigation>
            <a href="/app-huopan/goOrder" slot="navigation_goback" class="navigation_goback"></a>
            <span slot="navigation_title" class="navigation_title">借款详情</span>
        </navigation>
        <div class="orderNav">
            <span>已结清金额(元)</span>
            <h2>
                {{amt}}
                <img src="../../../static/images/icon_yjq@3x.png"/>
            </h2>
            <span>对应<em>{{unclearCount}}笔</em>借款</span>
        </div>
        
        <div class="orderTitle">
            共{{unclearCount}}笔
        </div>
        <div class="playList" v-for="(item, index) in planList" :key="index">
            <div class="listNav">
                本息已还款
                <span>{{item.actualRepayAmt}}元</span>
            </div>
            <div class="list">
                借款金额
                <span>{{item.periodsPortAmt}}元</span>
            </div>
            <div class="list">
                借款期限
                <span>{{item.periodsStartDate}}至{{item.periodsEndDate}}</span>
            </div>
            <div class="list">
                借款详情
                <a href="javascript:;" @click="goOrderDetailOver(item.sysSeqId)">点击查看</a>
            </div>
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
            merchantId: xmy.getQueryString('merchantId')
        }
    },
    methods: {
        // 还款计划列表
        getPlan(){
            var _this = this;
            xmy.getData({
                url: '/gateway/postloan/plan/detail',
                type: 'GET',
                data: {
                    billOrderId: _this.orderId
                },
                success:function(res){
                    if(res.respCode == '000000'){
                        _this.unclearCount = res.data[0].loanTotalPeriods;
                        _this.planList = res.data;
                    }else{
                        xmy.toast(res.respMsg);
                    }
                }
            })
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
    @import '../../common/css/instalmentDetail.less';
</style>

