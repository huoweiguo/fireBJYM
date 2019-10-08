<template>
    <div id="app">
        <navigation>
            <a href="javascript:;" @click="goBack" slot="navigation_goback" class="navigation_goback"></a>
            <span slot="navigation_title" class="navigation_title">绑定银行卡</span>
        </navigation>
        <div class="order-bg"></div>
        <div class="card-ts">注意：请绑定您的借记卡或储蓄卡，不支持绑定信用卡。</div>
        <div class="card-txt">
            <div><span>持卡人</span><i v-text="userName">借百家</i></div>
            <!-- <div><span>持卡人</span><input type="text" v-model="userName" readonly /></div> -->
            <div><span>身份证号</span><i v-text="idCard">422801 1999 1212 131X</i></div>
            <div><span>银行卡号</span><input type="text" @input="isEmpty" @blur="cardName" maxlength="19" v-model="bankId" pattern="[0-9]*" placeholder="请输入卡号"></div>
            <div><span>银行名称</span><i>{{bankName}}</i></div>
            <div><span>预留电话</span><input type="number" @input="isEmpty" v-model="telphone" placeholder="请输入银行预留手机号"></div>
            <div>
                <span>验证码</span>
                <input type="number" v-model="chkcode" @input="isEmpty" placeholder="请输入验证码">
                <em v-show="isClick" @click="getSms">获取验证码</em>
                <em class="orange" v-show="!isClick">{{time}}s</em>
            </div>
        </div>

        <div class="bind-set">
            <button v-show="!bind" class="bind-btn1">确认绑定到账银行卡</button>
            <button v-show="bind" @click="gobind" class="bind-btn2">确认绑定到账银行卡</button>
            <a href="/bjym/module/cardList.html">支持的银行卡和限额</a>
        </div>
        <div id="xdy_toast" class="xdy_toast" v-show="unOpen"></div>
    </div>
</template>

<script>
import '../../common/css/bindCard.less';
import navigation from '../../components/navigation.vue';
import xmy from '../../../static/js/xmy.js';
export default {
    components: {
        navigation: navigation
    },
    data () {
        return {
            bankId: '',
            telphone: '',
            chkcode: '',
            time: 60,
            timer: null,
            isClick: true,
            unOpen: false,
            bind: false,
            userName: '',
            idCard:'',
            bankCode:'',
            bankName:'',
            bindCardSeqNo:'',
            token: xmy.getQueryString('token'),
            userId: xmy.getQueryString('userId'),
            getInto: xmy.getQueryString('type'),
            merchantId: xmy.getQueryString('merchantId'),
            productId: xmy.getQueryString('productId')
        }
    },

    methods: {
        trim (str) {
            return (str ? str.replace(/(^\s*)|(\s*$)/g,"") : '');
        },
        
        //根据卡号获取银行名称
        cardName () {
            let _this = this;
            if(_this.bankId == ""){
                xmy.toast("输入银行卡号");
                return false;
            }
            if(_this.bankId.length>15){
                xmy.getData({
                    url: '/gateway/preloan/cabin/bank',
                    data: {
                        cardNo: _this.bankId
                    },
                    success: function(res){
                        if(res.respCode == "000000"){
                            _this.bankCode = res.bankCode;
                            _this.bankName = res.bankName;
                        }else{
                            xmy.toast(res.respMsg);
                        }
                    }
                });
            }else{
                xmy.toast("请输入正确的银行卡号");
            }
        },

        //获取绑卡短信验证码
        getSms () {
            let _this = this,
                tel = this.trim(_this.telphone);
            
            if (!xmy.chkPhone(tel)) {
                xmy.toast("手机号码格式不正确");
                return false;
            }

            if(_this.bankId == ""){
                xmy.toast("请输入银行卡号");
                return false;
            }

            xmy.getData({
                url: '/gateway/preloan/bankCard/verifyCode',
                type: 'POST',
                data: {
                    userType: "B",
                    idCardName: _this.userName,
                    idCard: _this.idCard,
                    bankCard: _this.bankId,
                    bankCode: _this.bankCode,
                    bankName: _this.bankName,
                    phoneNum: _this.telphone,
                    productId: _this.productId
                },
                success: function(res){
                    if(res.respCode == "000000"){
                        _this.isClick = false;
                        _this.bindCardSeqNo = res.data;
                        clearInterval(_this.timer);
                        _this.timer = setInterval(function(){
                            if(_this.time <= 0){
                                _this.time = 60;
                                _this.isClick = true;
                                clearInterval(_this.timer);
                            } else {
                                _this.isClick = false;
                                _this.time--;
                            }
                        },1000);
                    }else{
                        xmy.toast(res.respMsg);
                    }
                }

            });
        },

        //确认绑卡
        gobind () {
            let _this = this;
            _this.bind = false;

            xmy.getData({
                url: '/gateway/preloan/bankCard/bindConfirm',
                type: 'POST',
                data: {
                    sysSeqId: _this.bindCardSeqNo,
                    verifyCode: _this.chkcode
                },

                success: function(res){
                    _this.bind = true;
                    if(res.respCode === "000000"){
                        
                        //绑卡成功
                        xmy.toast(res.respMsg);
                        setTimeout(function(){
                            window.location.href = 'javascript:window.history.go(-1)'
                        },1000);
                        
                    }else{
                        xmy.toast(res.respMsg);
                    }
                }
            });
        },
        
        goBack () {
            let _this = this;
            if(_this.getInto == 1){
                window.location.href = '/api/static/app/goauth';
            }else{
                // window.location.href = 'javascript:window.history.go(-1)'
                window.history.back();
            }
        },

        buried (status) {
            let _this = this;
            if(status){
                _this.buriedNo = 'Auth_RealName_Bank_Success';
            }else{
                _this.buriedNo = 'Auth_RealName_Bank_Fail';
            }
            xmy.getData({
                url: '/gateway/api/report/userBuried/logging',
                type: 'POST',
                data: {
                    buriedNo: _this.buriedNo
                },
                success:function(s){
                    if(status){
                        window.location.href = '/api/static/app/goauth';
                    }
                }
            })
        },

        isEmpty () {
            let bankid = this.trim(this.bankId || ""),
                tel = this.trim(this.telphone || ""),
                chkcode = this.trim(this.chkcode || "");
            if(bankid != '' && tel != '' && chkcode != ''){
                this.bind = true;
            } else {
                this.bind = false;
            }
        }
    },

    mounted () {
        let _this = this;


        //初始化绑卡信息
        xmy.getData({
            url: '/gateway/preloan/bankCard/bindCardLog/initBandCardInfo',
            type: 'POST',
            data: {
                userType: "B"
            },
            success: function(res){
                if(res.respCode == "000000"){
                    _this.userName = res.data.userName;
                    _this.idCard = res.data.idCard;
                    _this.telphone = res.data.userPhone;
                }else{
                    xmy.toast(res.respMsg);
                }
            }
        });
    }
}
</script>

