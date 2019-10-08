<template>
    <div id="app">
        <navigation>
            <a href="javascript:;" v-on:click="goBack" slot="navigation_goback" class="navigation_goback"></a>
            <span slot="navigation_title" class="navigation_title">产品详情</span>
        </navigation>
        <div class="main">
            <div class="box">
                <h2>申请条件</h2>
                <div class="list"><span>· </span><em>年龄:{{product.minAge}} - {{product.maxAge}}</em></div>
                <div class="list"><span>· </span><em>申请资料:<template v-for="(item, index) in product.authOption">
                    <template v-if="index != product.authOption.length-1">{{item.authName}}、</template>
                    <template v-else>{{item.authName}}</template>
                    </template></em></div>
            </div>
            <div class="box">
                <h2>额度期限</h2>
                <div class="list"><span>· </span><em>借款额度:{{product.loanMinAmt}} - {{product.loanMaxAmt}}</em></div>
                <div class="list"><span>· </span><em>借款期限:{{product.loanPeriodsNum}}{{product.loanPeriodsUnit === 'WEEK' ? '周' : product.loanPeriodsUnit === 'DAY' ? '天' : product.loanPeriodsUnit === 'MONTH' ? '月' : ''}}</em></div>
                <div class="list"><span>· </span><em>审批方式:{{product.auditTypeDesc}}</em></div>
                <div class="list"><span>· </span><em>还款方式:{{product.repaymentTypeDesc}}</em></div>
            </div>
            <div class="box">
                <h2>费用说明</h2>
                <div class="list"><span>· </span><em>年利率:{{((product.protInterestRate?product.protInterestRate:0) * 100).toFixed(2)}}%</em></div>
                <div class="list"><span>· </span><em>违约金:{{((product.protViolateRate?product.protViolateRate:0) * 100).toFixed(2)}}%</em></div>
                <div class="list"><span>· </span><em>罚息:{{((product.protPenalSumRate?product.protPenalSumRate:0) * 100).toFixed(2)}}%</em></div>
                <div class="list"><span>· </span><strong>实际到手金额=额度金额-VIP卡费，具体到手金额以实际银行到账为准。</strong></div>
                <div class="list" style="color: #9B9B9B;"><span>·</span><em>如有疑问请咨询在线客服。</em></div>
            </div>
        </div>
        
    </div>
</template>

<script>
    import '../../common/css/productDetail.less';
    import navigation from '../../components/navigation.vue';
    import xmy from '../../../static/js/xmy.js';

    export default {
        components: {
            navigation: navigation
        },

        data (){
            return  {
                product: [],
                productId: xmy.getQueryString('productId'),
                token: xmy.getQueryString('token')
            }
        },
        methods: {
            goBack: function(){
                window.location.href = '/app-huopan/authchk';
            },

            productDetail () {
                var _this = this;
                xmy.getData({
                    url: '/gateway/preloan/product/queryProductInfo',
                    data: {
                        productId: _this.productId
                    },
                    success: function (res) {
                        res.authOption = JSON.parse(res.authOption);
                        _this.product = res;
                    }
                });
            }
        },
        mounted() {
            
            //获取产品详情
            this.productDetail();
            
        }
    }
</script>

