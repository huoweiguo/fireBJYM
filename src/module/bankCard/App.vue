<template>
    <div id="app" class="main">
        <navigation>
            <a href="javascript:;" @click="goCenter" slot="navigation_goback" class="navigation_goback"></a>
            <span slot="navigation_title" class="navigation_title">银行卡</span>
        </navigation>
        <div class="none" v-show="noCard">
            <img src="../../../static/images/icon_bank_card@2x.png"/>
            <p>还没有银行卡</p>
        </div>
        <div class="tip">
            <img src="../../../static/images/icon_taps2_blue@3x.png"/>
            默认卡是你借款成功后的到账银行卡！
        </div>
        <ul id="cardLi">
            <li v-for="(item,index) in cardList" :key="index" @click="changeCard(item.bankName,item.realBankCard,item.bankCard,item.userBankId,item.isDefault)">
                <img :src="item.bankLog"/>
                <div class="cardContent">
                    <h2><strong>{{item.bankName}}</strong><span v-show="item.isDefault == 'Y'" >默认卡</span></h2>
                    <h3>储蓄卡</h3>
                    <h4><span>****</span><span>****</span><span>****</span><em>{{item.bankCard.substring(item.bankCard.length-4)}}</em></h4>
                </div>
            </li>
        </ul>
        <div class="addCard">
            <a :href="bindCard">
                <img src="../../../static/images/more_add@2x.png"/>
                <span>添加银行卡</span>
                <img class="go" src="../../../static/images/rightarrow_1f.png"/>
            </a>
        </div>
        <div class="apply_btn" v-show="cardList.length > 0 && type == 1" style="display:none;">
            <a href="javascript:;" v-on:click="goinit">立即申请</a>
        </div>
        <!-- <div class="tip">还款时，将优先从默认银行卡扣款，如还款失败可尝试更换默认银行卡后再试。</div> -->
        <div class="body-mask" v-show="isChk"></div>
        <div class="operation" :class="{'showChange': isChk}">
            <!-- <div class="list" @click="relieve">解绑绑定银行卡</div> -->
            <div class="list" @click="major" v-show="isMain">设置默认银行卡</div>
            <div class="cancel" @click="abolish">取消</div>
        </div>
        <div class="show-mask" v-show="isChk"></div>
        <div class="popup" v-show="showTip">
            <div class="inquiry" v-show="success">
                <h2>{{question}}</h2>
                <p>{{makeSure}}</p>
                <div class="btn">
                    <button class="cancelBtn" @click="abolish">取消</button>
                    <button class="sureBtn" v-show="defaulted" @click="majorSure">确定</button>
                </div>
            </div>
            <div class="notice" v-show="showResult">
                <h2>{{resultTittle}}</h2>
                <p>{{resultReason}}</p>
                <div class="btn">
                    <span @click="abolish">知道了</span>
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
    data () {
        return{
            isChk:false,
            cardList: [],
            bindCard:'',
            token: xmy.getQueryString('token'),
            userId: xmy.getQueryString('userId'),
            merchantId: xmy.getQueryString('merchantId'),
            appId: xmy.getQueryString('appId'),
            type: xmy.getQueryString('type'),
            productId: xmy.getQueryString('productId'),
            productName: xmy.getQueryString('productName'),
            userName: xmy.getQueryString('userName'),
            orderId: xmy.getQueryString('orderId'),
            repaytype: xmy.getQueryString('repaytype'),
            amt: xmy.getQueryString('amt'),
            periods: xmy.getQueryString('periods'),
            repayPlanId: xmy.getQueryString('repayPlanId'),
            noCard: false,
            cardId:'',
            isDefault:'',
            isMain:true,
            realBankCard:'',
            showTip: false,
            success: false,
            question:'',
            cardName:'',
            makeSure:'',
            lastName:'',
            showResult:false,
            resultTittle:'',
            resultReason:'',
            defaulted:false,
            clear: false,
            doNothing: true
        }
    },
    methods:{
        goCenter () {
            if (this.type == 1) {
                //跳转产品认证页面
                window.location.href = '/app-huopan/authchk';
            } else if (this.type == 2) {
                //跳转到还款
                window.history.back();
                // window.location.href = '/bjym/module/repay.html?token='+this.token+'&merchantId='+ this.merchantId+'&userId='+this.userId+'&orderId='+this.orderId+'&amt='+this.amt+'&repaytype='+this.repaytype +'&periods='+this.periods+'&repayPlanId='+this.repayPlanId;
            } else if (this.type == 3) {
                //跳转到个人中心
                window.location.href = '/app-huopan/center';
            }else{
                window.history.back();
            }
        },
        // 判断是否是默认卡
        operationCard () {
            let _this = this;
            if(_this.isDefault == "Y"){
                _this.isMain = false;
                _this.isChk = false;
                // 提示默认银行卡不可操作
                _this.showTip = true;
                _this.success = false;
                _this.showResult = true;
                _this.resultTittle = "默认银行卡不可操作";
                
            }else{
                _this.showResult = false;
                _this.isMain = true;
                _this.isChk = true;
            }
        },
        // 解除绑定银行卡询问
        relieve () {
            let _this = this;
            _this.isChk = false;
            _this.clear = true;
            _this.defaulted = false;
            _this.success = true;
            _this.question = "确认解除绑定？";
            _this.makeSure = "您确定要把"+_this.cardName+"("+_this.lastName+") 的银行卡解除绑定，解绑后，该银行卡将不再列表中展示，需重新绑定银行卡？"
            _this.showTip = true;
        },
        // 确认解除绑定银行卡
        // relieveSure () {
        //     let _this = this;
        //     $.ajax({
        //         url: '/gateway/api/order/unbindCard/direct?t='+(new Date()).getTime(),
        //         type: "post",
        //         data: {
        //             userId: _this.userId,
        //             token: _this.token,
        //             bankId: _this.cardId
        //         },
        //         success:function(res){
        //             if(res.respCode == "000000"){
        //                 _this.success = false;
        //                 _this.showResult = true;
        //                 _this.resultTittle = "已解除该银行卡的绑定";
        //                 // window.location.reload()
        //             }else{
        //                 _this.success = false;
        //                 _this.showResult = true;
        //                 _this.resultTittle = "解除银行卡绑定失败"
        //                 _this.resultReason = res.respMsg;
        //                 // window.location.reload()
        //             }
        //         }
        //     })
        // },
        // 设为主卡询问
        major () {
            let _this = this;
            _this.isChk = false;
            _this.defaulted = true;
            _this.showResult = false;
            _this.clear = false;
            _this.showTip = true;
            _this.success = true;
            _this.question = "设置为默认卡？";
            _this.makeSure = "默认卡是你借款成功后的到账银行卡";
        },
        // 设为主卡
        majorSure () {
            let _this = this;
            xmy.getData({
                url: '/gateway/preloan/bankCard/default/change',
                type: "POST",
                data: {
                    userBankId: _this.cardId
                },
                success:function(res){
                    if(res.respCode == "000000"){
                        _this.success = false;
                        _this.showResult = true;
                        _this.resultTittle = "已设置为默认卡";
                        _this.getCardList();
                    }else{
                        _this.success = false;
                        _this.showResult = true;
                        _this.resultTittle = "设置默认卡失败"
                        _this.resultReason = res.respMsg;
                    }
                }
            })
        },
        // 取消
        abolish () {
            this.isChk = false;
            this.showTip = false;
        },
        // 操作卡
        changeCard (bankName,realBankCard,bankCard,userBankId,isDefault) {
             _this.cardName = bankName;
            _this.lastName = bankCard;
            _this.realBankCard = realBankCard;
            _this.cardId = userBankId;
            _this.isDefault = isDefault;
            _this.operationCard();
        },

        // 获取银行卡列表
        getCardList () {
            var _this = this;
            _this.bindCard = "/bjym/module/bindCard.html?userId="+_this.userId+'&token='+_this.token+'&merchantId='+_this.merchantId+"&productId="+_this.productId;
            xmy.getData({
                url: '/gateway/preloan/bankCard/query',
                type: 'POST',
                contentType: "application/json",
                data: JSON.stringify({
                    token: _this.token,
                    userId: _this.userId,
                    merchantId: _this.merchantId,
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
                        
                    }else{
                        xmy.toast(res.respMsg);
                    }
                }
            });
        },

        //跳转到风控页面
        goinit () {
            window.location.href = '/bjym/module/init.html?token='+this.token+'&userId='+this.userId+'&merchantId='+this.merchantId+'&productId='+this.productId+'&productName='+this.productName+'&userName='+this.userName;
        }
    },
    mounted () {
        this.getCardList();
    }
}
</script>
<style lang="less">
@import '../../common/css/bankCard.less';
</style>
