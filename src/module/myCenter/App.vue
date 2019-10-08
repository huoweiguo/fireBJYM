<template>
    <div id="app">
        <img src="https://shilupan-huopan.oss-cn-shanghai.aliyuncs.com/huopan/bannerxq.jpg"/>
        <!-- <ul> -->
            <!-- <li>
                <a :href="balance">
                    <img class="left" src="../../../static/images/icon1_wdye@2x.png"/>
                    <span>我的余额</span>
                    <img src="../../../static/images/rightarrow_1f.png"/>
                    <em>{{accountBalance}}</em>
                </a>
            </li> -->
            <!-- <li v-for="(item,index) in menuList" :key="index" v-if="item.id!=3&&item.id!=1&&item.id!=6&&item.id!=7">
                <a v-if="item.id == 4" :href="myCard"> -->
                    <!-- <img class="left" :src="item.logo"/> -->
                    <span>{{item.name}}</span>
                    <img src="../../../static/images/rightarrow_1f.png"/>
                <!-- </a> -->
                <!-- <a v-if="item.id == 3" :href="order">
                    <img class="left" :src="item.logo"/>
                    <span>{{item.name}}</span>
                    <img src="../../../static/images/rightarrow_1f.png"/>
                    <em style="color:#FF7F26;">{{repayment}}</em>
                </a> -->
                <!-- <a v-if="item.id == 2" href="/api/static/xmy_app/stars"> -->
                    <!-- <img class="left" :src="item.logo"/> -->
                    <!-- <span>{{item.name}}</span>
                    <img src="../../../static/images/rightarrow_1f.png"/>
                </a> -->
                <!-- <a v-if="item.id == 5" href="/api/static/xmy_app/khfk"> -->
                    <!-- <img class="left" :src="item.logo"/> -->
                    <!-- <span>{{item.name}}</span>
                    <img src="../../../static/images/rightarrow_1f.png"/>
                </a> -->
                <!-- <a v-if="item.id == 6" :href="shareLink">
                    <img class="left" :src="item.logo"/>
                    <span>{{item.name}}</span>
                    <img src="../../../static/images/rightarrow_1f.png"/>
                </a> -->
                <!-- <a v-if="item.id == 7" :href="creditCard">
                    <img class="left" :src="item.logo"/>
                    <span>{{item.name}}</span>
                    <img src="../../../static/images/rightarrow_1f.png"/>
                </a> -->
                <!-- <a v-if="item.id == 8" href="/api/static/xmy_app/phone"> -->
                    <!-- <img class="left" :src="item.logo"/> -->
                    <!-- <span>{{item.name}}</span>
                    <img src="../../../static/images/rightarrow_1f.png"/>
                </a> -->
                
            <!-- </li> -->
            <!-- <li>
                <a href="/api/static/xmy_app/setting"> -->
                    <!-- <img class="left" src="../../../static/images/icon8_wdsz@2x.png"/> -->
                    <!-- <span>我的设置</span>
                    <img src="../../../static/images/rightarrow_1f.png"/>
                </a>
            </li> -->
        <!-- </ul> -->
    </div>
</template>
<script>
import xmy from '../../../static/js/xmy.js';
export default {
    // data () {
    //     return {
    //         accountBalance:'',
    //         token: xmy.getQueryString('token'),
    //         userId: xmy.getQueryString('userId'),
    //         appVersion: xmy.getQueryString('appVersion'),
    //         device: xmy.getQueryString('device'),
    //         channelType: xmy.getQueryString('channelType'),
    //         appId: 'xiaomuyu',
    //         repayment:'',
    //         shareLink:'',
    //         myCard:"/api/static/bjym/module/bankCard.html?userId="+xmy.getQueryString('userId')+"&token="+xmy.getQueryString('token'),
    //         balance:'',
    //         order:'',
    //         bankCardAuth:'',
    //         menuList:[]
    //     }
    // },
    // methods: {
    //     getMenu () {
    //         let _this = this;
    //         if(_this.device == "iOS"){
    //             _this.appId = xmy.getQueryString('appId')
    //         }
    //         $.ajax({
    //         url: '/gateway/api/proxy/jbj/personalList?t='+(new Date()).getTime(),
    //         type: 'post',
    //         data: {
    //             token: _this.token,
    //             appId: _this.appId,
    //             appVersion: _this.appVersion,
    //             channelType: _this.channelType,
    //             device: _this.device
    //         },
    //         success: function(res){
    //             if(res.respCode == "000000"){
    //                 _this.menuList = res.voList;
    //             }else{
    //                 xmy.toast(res.respMsg)
    //             }
    //         }
    //     })
    //     }
    // },
    // mounted () {
    //     let _this = this;
    //     _this.getMenu ();
    //     $.ajax({
    //         url: '/gateway/api/proxy/jbj/getUserDetail?t='+(new Date()).getTime(),
    //         type: 'post',
    //         data: {
    //             token: _this.token,
    //             userId: _this.userId,
    //             userType: "B"
    //         },
    //         success: function(res){
    //             if(res.respCode == "000000"){
    //                 _this.accountBalance = res.accountBalance;
    //                 if(res.user.repaymentNum!=0&&res.user.repaymentNum!=''){
    //                     _this.repayment = res.user.repaymentNum+"笔订单待还";
    //                 }else{
    //                     _this.repayment = '';
    //                 }
    //                 // _this.shareLink = "http://proxy.xiaomuyu.net:8704/myd/share.html?userId="+_this.userId+"&token="+_this.token;
    //                 // _this.creditCard = "http://proxy.xiaomuyu.net:8704/myd/creditCard.html?userId="+_this.userId+"&token="+_this.token+"&userType=B";
    //                 _this.bankCardAuth = res.userAuthInfo.bankCardAuth;
    //                 _this.balance = "/api/static/bjym/module/balance.html?userId="+_this.userId+"&token="+_this.token;
    //                 _this.order = "/api/static/bjym/module/order.html?userId="+_this.userId+"&token="+_this.token+"&linkType=mycenter";
    //             }else{
    //                 xmy.toast(res.respMsg)
    //             }
    //         }
    //     })
    // }
}
</script>
<style lang="less" scoped>
    @import '../../common/css/myCenter.less';
</style>