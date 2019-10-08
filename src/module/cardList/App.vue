<template>
    <div id="app">
        <div class="content-pro">
            <navigation>
                <a href="javascript:window.history.go(-1);" slot="navigation_goback" class="navigation_goback"></a>
                <span slot="navigation_title" class="navigation_title">支持的银行卡和限额</span>
            </navigation>
            <div class="card" v-for="(item,index) in cardList" :key="index">
				<img :src="item.bankLogo" />
				<div class="text">
					<h2>{{item.bankName}}</h2>
					<span>单笔金额≤{{item.perTransactionLimit}}元，单日金额≤{{item.perDayLimit}}元</span>
				</div>
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
    data (){
        return {
            cardList: [],
            token: xmy.getQueryString('token')
        }
    },

    methods: {
        getCardList () {
            var _this = this;
            xmy.getData({
                url: '/gateway/preloan/limitAmt/list',
                success: function (res) {
                    if (res.respCode === '000000') {
                        _this.cardList = res.data;
                    } else {
                        xmy.toast(res.respMsg);
                    }
                }
            });
        }
    },

    mounted () {
        var _this = this;
        this.getCardList();
    }
}
</script>



<style lang="less" scoped>
@import url(../../common/css/cardList.less);
</style>

