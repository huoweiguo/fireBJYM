<template>
    <div id="app" style="height: 100%;">
        
        <div class="content-pro" style="height:100%;">
            <navigation>
                <a href="javascript:window.history.back();" slot="navigation_goback" class="navigation_goback"></a>
                <span slot="navigation_title" class="navigation_title">展期还款</span>
                <a href="javascript:;" class="charge_pro">展期记录</a>
            </navigation>
            <div class="roll">
                <h2>展期还款金额（元）</h2>
                <div class="over">
                    <div class="left">
                        <small>¥</small>
                        <span>{{list.loanExTotalFee}}</span>
                    </div>
                    <div class="right">
                        <h3 v-on:click="lookFee">费用说明<img src="../../../static/images/icon_taps@3x.png"/></h3>
                        <p>管理费{{list.loanExManagementFee}}元+手续费{{list.loanExPoundageFee}}+利息{{list.loanExInterst}}元+逾期费{{list.loanExOverdueFee}}元</p>
                    </div>
                </div>
            </div>
            <ul>
                <li>
                    <span>推延还款期限</span>
                    <em class="color">{{list.exDay}}天</em>
                </li>
                <li>
                    <span v-on:click="allFee">延后总还款</span><img v-on:click="allFee" src="../../../static/images/icon_taps@3x.png"/>
                    <em>{{list.delayTotalFee}}元</em>
                </li>
                <li>
                    <span>延后还款日</span>
                    <em>{{list.periodsEndDate}}</em>
                </li>
            </ul>
            <div class="sureBtn" @click="repaySure">
                <a href="javascript:;">确认展期</a>
            </div>
            <div class="popup" v-show="showFee">
                <h2>费用说明</h2>
                <img @click="cancel" src="../../../static/images/close.png"/>
                <table>
                    <tr>
                        <th>总计费用</th>
                        <th>{{list.loanExTotalFee}}元</th>
                    </tr>
                    <tr>
                        <td>·&nbsp;&nbsp;管理费</td>
                        <td>{{list.loanExManagementFee}}元</td>
                    </tr>
                    <tr>
                        <td>·&nbsp;&nbsp;手续费</td>
                        <td>{{list.loanExPoundageFee}}元</td>
                    </tr>
                    <tr>
                        <td>·&nbsp;&nbsp;利息</td>
                        <td>{{list.loanExInterst}}元</td>
                    </tr>
                    <tr>
                        <td>·&nbsp;&nbsp;逾期费</td>
                        <td>{{list.loanExOverdueFee}}元</td>
                    </tr>
                </table>
            </div>
            <div class="popup" v-show="showAll">
                <h2>延后总还款</h2>
                <img @click="cancel" src="../../../static/images/close.png"/>
                <table>
                    <tr>
                        <th>总还款金额</th>
                        <th>{{list.delayTotalFee}}元</th>
                    </tr>
                    <tr>
                        <td>·&nbsp;&nbsp;借款金额</td>
                        <td>{{list.delayLoanFee}}元</td>
                    </tr>
                    <tr>
                        <td>·&nbsp;&nbsp;利息</td>
                        <td>{{list.loanExInterst}}元</td>
                    </tr>
                </table>
            </div>

            <div class="loan_mask" v-show="showMask" @click="cancel"></div>
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
    data (){
        return {
            showMask: false,
            showFee: false,
            showAll: false,
            token: xmy.getQueryString('token'),
            userId: xmy.getQueryString('userId'),
            billOrderId: xmy.getQueryString('orderId'),
            list: {},
            merchantId: xmy.getQueryString('merchantId'),
            repayPlanId: xmy.getQueryString('repayPlanId'),
            periods: '',
            productId: xmy.getQueryString('productId')
            
        }
    },

    methods: {
        trim (str) {
            return (str ? str.replace(/(^\s*)|(\s*$)/g,"") : '');
        },
        // 关闭弹窗
        cancel () {
            this.showMask = false;
            this.showFee = false;
            this.showAll = false;
        },
        // 费用说明
        lookFee: function(){
            var _this = this;
            _this.showMask = true;
            _this.showFee = true;
        },
        // 延后总还款
        allFee () {
            var _this = this;
            _this.showMask = true;
            _this.showAll = true;
        },
        // 获取数据
        getdate(){
            var _this = this;
            xmy.getData({
                url: '/gateway/postloan/loanEx/caculate',
                type: 'POST',
                data: {
                    billOrderId: _this.billOrderId
                },
                success:function(res){
                    if(res.respCode == '000000'){
                        _this.list = res.data;
                    }else{
                        xmy.toast(res.respMsg);
                    }
                }
            })
        },
        // 确认展期
        repaySure(){
            var _this = this;
            window.location.href = "/bjym/module/repay.html?token="+_this.token+"&orderId="+ _this.billOrderId+"&repaytype=rollover&repayPlanId="+_this.repayPlanId+"&merchantId="+ _this.merchantId+"&userId="+_this.userId+"&amt="+ _this.list.loanExTotalFee+'&periods='+_this.periods+"&productId="+_this.productId;
        }
    },

    mounted () {
        var _this = this;
        _this.getdate();
        _this.periods = xmy.getQueryString('periods');
    }
}
</script>

<style lang="less" scoped>
@import url(../../common/css/rollOver.less);
</style>

