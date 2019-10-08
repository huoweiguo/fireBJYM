<template>
    <div id="app">
        <div class="body-mask"></div>
        <navigation>
            <a href="javascript:;" v-on:click="goAfter" slot="navigation_goback" class="navigation_goback"></a>
            <span slot="navigation_title" class="navigation_title">还款详情</span>
        </navigation>
        <div class="orderNav">
            <span>{{list.currentRepayDate}}应还款(元)</span>
            <h2>
                {{list.currentShouldRepayTotalAmt}}
            </h2>
            <p v-if="list.overdueStatus != 'O'&&!list.todayIsRepay">
                <img src="../../../static/images/icon_taps2_blue@3x.png"/>
                确保到期资金充足，请按时还款
            </p>
            <p class="orderOver" v-if="list.overdueStatus != 'O'&&list.todayIsRepay">
                <img src="../../../static/images/icon_taps2_red@3x.png"/>
                今天是还款日，请尽快还款以免对征信产生影响
            </p>
            <p class="orderOver" v-if="list.overdueStatus == 'O'">
                <img src="../../../static/images/icon_taps2_red@3x.png"/>
                您已逾期{{list.overdueDay}}天，请尽快还款以免对征信产生影响
            </p>
            <a href="javascript:;" @click="repaySure">确认还款</a>
        </div>
        <ul @click="goRollOver" v-if="list.isExtends">
            <li>
                <div class="orderLeft">
                    <h2>展期还款</h2>
                    <p>如您无法如期全额还款，可申请展期，延长还款时间</p>
                </div>
                <img src="../../../static/images/rightarrow_1f.png"/>
            </li>
        </ul>
        <div class="playList">
            <div class="listNav">
                <h2>
                    {{list.currentShouldRepayTotalAmt}}元
                    <span v-if="list.totalPeriods > 1">第{{list.currentPeriods}}期</span>
                    <span v-if="list.totalPeriods == 1">{{list.currentPeriods}}天</span>
                </h2>
                <p>{{list.currentLoanDate}}借 | <template v-if="list.totalPeriods > 1">共{{list.totalPeriods}}期</template>
                <template v-if="list.totalPeriods == 1">{{list.currentPeriods}}天</template>
                </p>
            </div>
            <div class="list">
                本金
                <span>{{list.currentCapital}}</span>
            </div>
            <div class="list">
                利息
                <span>{{list.currentInterest}}</span>
            </div>
            <div class="list">
                逾期费
                <span>{{list.overdueAmt}}</span>
            </div>
            <div class="list" v-if="list.actualRepayAmt">
                已还金额
                <span>{{list.actualRepayAmt}}</span>
            </div>
            <div class="list" v-if="list.periodsSubstractAmt">
                减免金额
                <span>{{list.periodsSubstractAmt}}</span>
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
            list: [],
            repayPlanId: xmy.getQueryString('repayPlanId'),
            orderId: xmy.getQueryString('orderId'),
            merchantId: xmy.getQueryString('merchantId'),
            periods: '',
            goBack: '',
            productId: xmy.getQueryString("productId")
        }
    },
    methods: {
        getList(){
            var _this = this;
            xmy.getData({
                url: '/gateway/postloan/loan/queryBillRepayPlanDetail',
                type: 'POST',
                contentType: "application/json",
                data: JSON.stringify({
                    token: _this.token,
                    repayPlanId: _this.repayPlanId
                }),
                success:function(res){
                    if(res.respCode == '000000'){
                        _this.list = res.data;
                        _this.periods = _this.list.currentPeriods;
                    }else{
                        xmy.toast(res.respMsg);
                    }
                }
            })
        },
        // 去展期
        goRollOver (){
            var _this = this;
            window.location.href = "/bjym/module/rollOver.html?token="+_this.token+"&orderId="+ _this.orderId+"&repayPlanId="+_this.repayPlanId+"&merchantId="+ _this.merchantId+"&userId="+_this.userId+"&amt="+ _this.list.currentShouldRepayTotalAmt+'&periods='+_this.periods+"&productId="+_this.productId;
        },
        // 确认还款
        repaySure(){
            var _this = this;
            window.location.href = "/bjym/module/repay.html?token="+_this.token+"&orderId="+ _this.orderId+"&repaytype=repayall&merchantId="+ _this.merchantId+"&userId="+_this.userId+"&amt="+ _this.list.currentShouldRepayTotalAmt+'&periods='+_this.periods+"&repayPlanId="+_this.repayPlanId+"&productId="+_this.productId;
        },
        goAfter () {
            var _this = this;
            if(_this.list.totalPeriods > 1){
                window.history.back();
            }else{
                window.location.href = '/app-huopan/goOrder';
            }
        }
    },
    mounted () {
        this.getList();
    }
}
</script>

<style lang="less" scoped>
    @import '../../common/css/repayDetail.less';
</style>

