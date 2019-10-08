<template>
<div id="app">
    <div class="content-pro" id="process" v-show="!result">
        <navigation>
            <a href="javascript:;" @click="goBack" slot="navigation_goback" class="navigation_goback"></a>
            <span slot="navigation_title" class="navigation_title">立即还款</span>
        </navigation>
        <div class="repaybg"></div>
        <div class="small-txt" v-show="nextProcess">注意：银行卡仅支持储蓄卡还款。</div>
        <div class="repay-detail" v-show="nextProcess">
            <div class="repay-bank" @click="selectCard = true">
                <img :src="bankLogo"> {{bankName}} ({{cardEndNum}})
                <i class="arrow_right"></i>
            </div>
            <div class="repay-money">
                <span>&yen;</span>
                {{orderAmount}}
            </div>
        </div>
        <div class="repay_small">本次还款手续费<span>{{serviceFee}}</span>元，实扣金额<span>{{orderAmountAll}}</span>元</div>
        <div class="repay-btn">
            <a href="javascript:;" class="repay-btn7" v-show="!showBtn">还款中<span class="interPoint"><em class="inter_em gomove">...</em></span></a>
            <button class="repay-btn2" v-show="showBtn" @click="repaySure">确认</button>
        </div>
        <div class="bank-mask" v-show="selectCard" @click="closeBank"></div>
        <!--选择还款方式-->
        <div class="bank-slt" id="bank_list" :class="{'showBank': selectCard}">
            <h3 class="bank-nav">选择支付方式<span class="close-btn" @click="closeBank"></span></h3>
            <ul v-show="cardList.length > 0">
                
                <li v-for="(item,index) in cardList" :key="index" :class="{'active': isClick == false && item.isDefault == 'Y'}" :data-bankid="item.bankCard" :data-endnum="item.bankCard" :data-bankimg="item.bankLog" :data-bankname="item.bankName" :data-id="item.realBankCard">
                    <img :src="item.bankLog" class="bankImg">
                    <div class="bank-detail">
                        <span class="bank-name">{{item.bankName}}<i>({{item.bankCard.substring(item.bankCard.length-4)}})</i></span>
                        <p>银行单笔限额{{item.limitPerTransaction}}元</p>
                    </div>
                    <i class="correct"></i>
                </li>
            </ul>

            <a v-if="canBindCard === true" class="addCardPay" :href="bindCard">
                <img src="../../../static/images/more_add@2x.png" class="bankImg">
                <div class="add-card">添加银行卡支付</div>
            </a>

        </div>
    </div>

    <!--还款成功/失败信息-->
    <div class="repay-mes" v-show="result"><!--v-show="result"-->
        <navigation>
            <!-- <a href="javascript:window.history.go(-1);" slot="navigation_goback" class="navigation_goback"></a> -->
            <span slot="navigation_title" class="navigation_title">还款结果</span>
        </navigation>
        <img v-show="faild" src="../../../static/images/icon_refuse@2x.png">
        <img v-show="!faild" src="../../../static/images/icon_ok@2x.png">

        <h2 v-show="faild && repaytype == 'rollover'">
            <template v-if="deal">展期失败</template>
            <p style="color:#ff7f26;">{{faildMes}}</p>
        </h2>
        <h2 v-show="faild && repaytype != 'rollover'">
            <template v-if="deal">还款失败</template>
            <p style="color:#ff7f26;">{{faildMes}}</p>
        </h2>

        <h2 v-show="!faild && repaytype == 'rollover' && pendding">
            展期处理中
            <p>{{pendMsg}}</p>
        </h2>
        <h2 v-show="!faild && repaytype != 'rollover'&& pendding">
            还款处理中
            <p>{{pendMsg}}</p>
        </h2>
        <h2 v-show="!faild && repaytype == 'rollover' && !pendding">
            展期成功
            <p>好借好还，再借不难，为信用点赞</p>
        </h2>
        <h2 v-show="!faild && repaytype != 'rollover'&& !pendding">
            还款成功，为信用点赞
            <p>还款方式：{{bankName}} 还款金额：{{orderAmountAll}}元</p>
        </h2>
        <div class="again" v-show="!faild">
            <a class="look" href="/app-huopan/gohome">返回首页</a>
        </div>
        <div class="again" v-show="faild">
            <a :href="order" class="reback">返回重试</a>
        </div>
    </div>
    <div id="xdy_toast" class="xdy_toast" v-show="unOpen"></div>
</div>
</template>

<script>
import navigation from '../../components/navigation.vue';
import xmy from '../../../static/js/xmy.js';
export default {
    components: {
        navigation: navigation
    },
    data (){
        return {
            token: xmy.getQueryString('token'),
            productId: xmy.getQueryString('productId'),
            userId: '',
            list: [],
            orderId: '',
            amt: '',
            nextProcess: true,
            hktel: '',
            hkcode: '',
            showTel: true,
            showBtn: true,
            unOpen: false,
            isSms: false,
            timer: null,
            getNumber: 60,
            faild: false,
            result: false,
            bankImg: '',
            orderAmount: '',
            orderAmountAll: '',
            bankLogo: '',
            bankName: '',
            sysSeqId: '',
            bankId: '',
            bankPhone: '',
            bankCard: '',
            cardName: '',
            faildMes: '',
            deal: true,
            hkuan: true,
            selectCard: false,
            serviceFee: 0,
            cardList: [],
            repayBankCard: '',
            cardEndNum: '',
            isClick: false,
            bindCard:'',
            merchantId: '',
            repaytype: '',
            periods: '',
            tradeType: '',
            order: '/app-huopan/goOrder',
            repayPlanId: '',
            pendMsg: '',
            pendding: false,
            payChannelCode: '',
            canBindCard: true
        }
    },

    methods: {
        // 返回
        goBack () {
            // window.location.href = '/app-huopan/goOrder'
            window.history.back();
        },
        trim (str) {
            return (str ? str.replace(/(^\s*)|(\s*$)/g,"") : '');
        },

        chkVal () {
            let chkCode = this.trim(this.hkcode);
            let tel = this.trim(this.bankPhone);

            if(tel != '' && chkCode != ''){
                this.showBtn = true;
            } else {
                this.showBtn = false;
            }
        },


        //选择银行卡
        closeBank () {
            this.selectCard = false;
        },  

        //确认还款
        repaySure () {
            let _this = this;
            _this.showBtn = false;
            console.log(_this.bankId);
            xmy.getData({
                url: '/gateway/postloan/repayment/pay',
                type: 'POST',
                contentType: "application/json",
                data: JSON.stringify({
                    billOrderId: _this.orderId,
                    bankCard: _this.bankId,
                    periods: _this.periods,
                    amt: _this.amt,
                    tradeType: _this.tradeType
                }),
                success: function(res){
                    _this.result = true;
                    _this.showBtn = true;
                    if(res.respCode == '000000'){
                        _this.faild = false;
                        _this.pendding = false;
                    }else if(res.respCode == '500'){
                        _this.faild = true;
                        _this.pendding = false;
                        _this.faildMes = res.respMsg;
                    } else {
                        _this.pendding = true;
                        _this.faild = false;
                        _this.pendMsg = res.respMsg;
                    }
                }
            });
        },

        //渲染卡列表
        renderCardList () {
            let _this = this;
            xmy.getData({
                url: '/gateway/preloan/bankCard/query',
                type: 'POST',
                contentType: "application/json",
                data: JSON.stringify({
                    token: _this.token,
                    userId: _this.userId,
                    merchantId: _this.merchantId,
                    // payChannelCode: _this.payChannelCode,
                    productId: _this.productId
                }),
                success: function(res){
                    if(res.respCode == "000000"){
                        if(res.data.length==0){
                            _this.noCard = true;
                        }else{
                            _this.noCard = false;
                        }
                        _this.cardList = res.data;
                        for(var i=0;i<res.data.length;i++){
                            if(res.data[i].isDefault == "Y"){
                                _this.bankId = res.data[i].bankCard;
                            }
                        }
                        // 渲染结束后再调用
                    }else{
                        xmy.toast(res.respMsg);
                    }
                    _this.$nextTick(function(){
                        _this.listEvent();
                    });
                }
            });
        },

        init(){
            var _this = this;
            xmy.getData({
                url: '/gateway/postloan/repayment/confirm',
                type: 'POST',
                data: {
                    billOrderId: _this.orderId,
                    amt: _this.amt
                },
                success:function(res){
                    if(res.respCode == '000000'){
                        _this.orderAmount = res.data.amt;
                        _this.bankLogo = res.data.bankLogo;
                        _this.bankName = res.data.bankName;
                        _this.cardEndNum = res.data.bankCode;
                        _this.serviceFee = res.data.serviceAmt;
                        _this.orderAmountAll = res.data.actAmt;
                        _this.canBindCard = res.data.canBindCard;
                        _this.payChannelCode = res.data.payChannelCode;
                        if(!_this.bankName){
                            window.location.href=_this.bindCard;
                        }
                        _this.renderCardList();
                    }else{
                        xmy.toast(res.respMsg);
                    }
                }
            })
        },

        listEvent(){
            let _this = this;
            let aLi = $('#bank_list').find('li');

            aLi.each(function(index,item){
                $(item).on("click",function(){
                    _this.isClick = true;
                    aLi.each(function(index,item){
                        aLi[index].className = '';
                    });
                    $(this).addClass('active');
                    _this.repayBankCard = $(this).attr('data-id');
                    _this.cardEndNum = $(this).attr('data-endnum').substring($(this).attr('data-endnum').length-4);
                    _this.bankName =  $(this).attr('data-bankname');
                    _this.bankLogo = $(this).attr('data-bankimg');
                    _this.bankId = $(this).attr('data-bankid');
                    
                    _this.closeBank();
                });
            });
        }
       
    },

    mounted () {
        let process = document.getElementById('process');
        let clientHeight = document.documentElement.clientHeight;
        process.style.height = clientHeight + 'px';
        var _this = this;
        _this.repaytype = xmy.getQueryString('repaytype');
        _this.merchantId = xmy.getQueryString('merchantId');
        _this.amt = xmy.getQueryString('amt');
        _this.orderId = xmy.getQueryString('orderId');
        _this.userId = xmy.getQueryString('userId');
        _this.token = xmy.getQueryString('token');
        _this.periods = xmy.getQueryString('periods');
        _this.repayPlanId = xmy.getQueryString('repayPlanId');

        if(_this.repaytype == 'rollover'){
            // 展期还款
            _this.tradeType = "ZQ";
        }else{
            // 普通还款
            _this.tradeType = "HK"
        }
        _this.init();
        this.bindCard = "/bjym/module/bankCard.html?token="+_this.token+"&orderId="+ _this.orderId+"&repaytype="+_this.repaytype+"&type=2&merchantId="+ _this.merchantId+"&userId="+_this.userId+"&amt="+ _this.amt+'&periods='+_this.periods+'&repayPlanId='+_this.repayPlanId+"&productId="+_this.productId;
        
    }
}
</script>



<style lang="less" scoped>
@import url(../../common/css/repay.less);
</style>

