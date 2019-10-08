<template>
    <div id="app">
        <div class="body-mask"></div>
        <navigation v-show="!showImg">
            <a href="javascript:;" @click="goAfter" slot="navigation_goback" class="navigation_goback"></a>
            <span slot="navigation_title" class="navigation_title">借款详情</span>
        </navigation>
        <navigation v-show="showImg">
            <a href="javascript:;" @click="showImg = false" slot="navigation_goback" class="navigation_goback"></a>
            <span slot="navigation_title" class="navigation_title">借款协议</span>
        </navigation>
        <div v-show="!showImg">
            <div class="orderNav">
                <span>已结清金额(元)</span>
                <h2>
                    {{lists.actualRepayAmt}}
                    <img src="../../../static/images/icon_yjq@3x.png"/>
                </h2>
                <span>好借好还，再借不难</span>
            </div>
            <div class="playList">
                <h2>借款明细</h2>
                <div class="list">
                    借款金额
                    <span>{{lists.periodsPortAmt}}元</span>
                </div>
                <div class="list">
                    借款产品
                    <span>{{lists.productName}}</span>
                </div>
                <div class="list">
                    借款期限
                    <span>{{lists.loanPeriodsTimes}}{{lists.loanPeriodsUnit === 'WEEK' ? '周' : lists.loanPeriodsUnit === 'DAY' ? '天' : lists.loanPeriodsUnit === 'MONTH' ? '月' : '' }}|{{lists.periodsStartDate}}至{{lists.periodsEndDate}}</span>
                </div>
                <!-- <div class="list">
                    还款方式
                    <span>{{lists.sysDate}}</span>
                </div> -->
                <div class="list">
                    借款时间
                    <span>{{lists.periodsStartDate}}</span>
                </div>
                <div class="list">
                    订单编号
                    <span>{{lists.billOrderId}}</span>
                </div>
                <div class="list" v-if="lists.contractShowFlag != 'N'" @click="showImg = true">
                    借款合同
                    <a href="javascript:;">查看</a>
                </div>
            </div>
            <div class="playList">
                <h2>还款明细</h2>
                <div class="list">
                    本金
                    <span>{{lists.periodsPortAmt}}元</span>
                </div>
                <div class="list">
                    利息
                    <span>{{lists.periodsInterestRateAmt}}元</span>
                </div>
                <div class="list">
                    逾期费
                    <span>{{lists.periodsPenalSumAmt}}元</span>
                </div>
                <div class="list">
                    手续费
                    <span>{{lists.periodsViolateAmt}}元</span>
                </div>
                <div class="list" v-if="lists.periodsSubstractAmt">
                    减免金额
                    <span>{{lists.periodsSubstractAmt}}元</span>
                </div>
                <div class="list">
                    还款时间
                    <span>{{lists.loanSettleTimeFormat}}</span>
                </div>
            </div>
        </div>
        <div class="popup" v-show="showImg">
            <img id="loanProtocolPicUrl" :src="lists.loanProtocolPicUrl"/>
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
            billOrderId: xmy.getQueryString('orderId'),
            lists: {},
            showImg: false,
            merchantId: xmy.getQueryString('merchantId'),
            goBack: ''
        }
    },
    methods: {
        getList:function(){
            var _this = this;
            xmy.getData({
                url: '/gateway/postloan/plan/'+ _this.billOrderId,
                success:function(res){
                    if(res.respCode == '000000'){
                        _this.lists = res.data
                    }else{
                        xmy.toast(res.respMsg)
                    }
                }
            })
        },
        goAfter(){
            var _this = this;
            if(_this.lists.singleFlag == 'N'){
                window.history.go(-1);
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
    @import '../../common/css/orderDetail.less';
</style>

