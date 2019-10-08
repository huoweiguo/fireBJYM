<template>
    <div id="app">
        <div class="circle_rotate">
            <div class="rotate_img"></div>
            <div class="computed_init">
                <p>智能计算您的额度</p>
                <span v-show="!com_result">计算中{{tickTime}}s</span>
                <div v-show="com_result" class="money_count">
                    <i>&yen;</i>
                    <div id="num-roll" class="num-roll"></div>
                </div>
            </div>
        </div>
        <div class="result_refuse" v-show="isRefuse">
            <p>提交失败，请重新提交！</p>
            <div class="btn">
                <a href="javascript:;" @click="gohome">返回首页</a>
                <span @click="reSubmit">重新提交</span>
            </div>
        </div>


        <div class="result" v-show="isResult == 2">
            <div class="loan_result">借款结果</div>
            <div class="small-text">
                <img src="../../../static/images/icon_refuse@2x.png">
                <h3>{{refuse_reson}}</h3>
                <h5>{{refuse_msg}}</h5>
                <div class="otherProduct" v-show="productList.length>0">
                    <img src="./images/left@3x.png"/>
                    新口子 · 秒下款
                    <img src="./images/right@3x.png"/>
                </div>
                <template v-for="(item,index) in productList" >
                    <img class="productBanner" :key="index" @click="goAuth(item.productId,item.productName)" :src="item.imageUrl"/>
                </template>
                
                <div class="result_adv" @click="gohome"><a href="javascript:;">返回首页</a></div>
            </div>
        </div>

        <div class="result" v-show="isResult == 3">
            <div class="loan_result">借款结果</div>
            <img v-show="!success" class="result_img" src="../../../static/images/icon_refuse@2x.png">
            <img v-show="success" class="result_img" src="../../../static/images/icon_ok@2x.png">
            <h2>{{message}}</h2>
            <div v-show="success && succp" class="loan_small_txt">
                到期准时还款，再次借款每笔
                <p>提额<span>最高50%</span>降息<span>最高30%</span></p> 
            </div>
            <h5 v-show="showMsg">{{faildMsg}}</h5>
            <div class="success"  v-show="success && bank_suc">
                <div class="detail">
                    <h4>到账详情</h4>
                    <div class="process">
                        <div class="iconText">
                            <span>发起借款申请</span>
                            <h6>银行处理中</h6>
                            <span>预计2小时内到账</span>
                            <span>到账成功</span>
                        </div>
                        <div class="iconLine">
                            <em></em>
                            <strong></strong>
                            
                            <img src="../../../static/images/round_blue@2x.png"/>
                            <i></i>
                            <span></span>
                        </div>
                    </div>
                </div>
                
                <ul>
                    <li>
                        <span>借款金额</span>
                        <em>¥{{tradeAmt}}</em>
                    </li>
                    <li>
                        <span>到账金额</span>
                        <em>¥{{actualAmt}}</em>
                    </li>
                    <li>
                        <span>到账方式</span>
                        <em v-show="isCard">{{bankName}} 尾号{{bankCard}}</em>
                        <em v-show="!isCard">账户余额</em>
                    </li>
                </ul>
            </div>
            <div class="small-btn">
                <a href="/app-huopan/gohome" class="r-btn2">返回首页</a>
            </div>
        </div>
    </div>
</template>


<script>
    import xmy from '../../../static/js/xmy.js';
    
    export default {
        data () {
            return {
                timer: null,
                tickTime: 20,
                com_result: false,
                money: 0,
                token: xmy.getQueryString('token'),
                userId: xmy.getQueryString('userId'),
                productId: xmy.getQueryString('productId'),
                productUserId: xmy.getQueryString('productUserId'),
                productName: xmy.getQueryString('productName'),
                userName: xmy.getQueryString('userName'),
                merchantId: xmy.getQueryString('merchantId'),
                isRefuse: false,
                refuse_msg: '',
                refuse_reson: '',
                isResult: 1,
                ajaxEV: null,
                bannerUrl: '',
                showMask: false,
                timeOut: "",
                tipText: 2,
                productList: [],
                success: false,
                message: '',
                faildMsg: '',
                bank_suc: false,
                succp: true,
                isCard: true,
                bankName: '',
                tradeAmt: '',
                bankCard: '',
                actualAmt: '',
                loanApplySeqId: ''
            }
        },

        methods: {
            
            cuntdown () {
                var _this = this;
                clearInterval(this.timer);
                this.timer = setInterval(function(){
                    if(_this.tickTime > 0){
                         _this.tickTime--;
                    }

                    if(_this.tickTime <= 0){
                        _this.isRefuse = true;
                        _this.ajaxEV.abort();
                    }
                },1000);
            },

            //计算后的额度显示效果
            scrollNumber () {
                var r1=new DigitRoll({
                    container:'#num-roll'
                });
                r1.roll(this.money);
            },

            //借款试算
            verificationCredit() {
                let _this = this;
                _this.ajaxEV = xmy.getData({
                    url: '/gateway/preloan/risk/getRiskResult',
                    type: 'POST',
                    data: {
                        productId: _this.productId,
                        userName: _this.userName,
                        productName: _this.productName,
                        channelId: '01'
                    },
                    success: function(res){
                        
                        if(res.respCode == '6666666'){

                            setTimeout(function() {
                                if (_this.tickTime <= 2) {
                                    return false;
                                }
                                
                                _this.money = res.data.productLoanAmt;
                                _this.com_result = true;
                                clearInterval(_this.timer);
                                _this.scrollNumber();
                                _this.tickTime = 20;
                                _this.loanApplySeqId = res.data.sysSeqId;
                                // 直接放款结果展示
                                if(res.data.auditProcess == 3){
                                    setTimeout(function(){
                                        _this.toLoan();
                                    },1500)
                                }else{
                                    setTimeout(function(){
                                        window.location.href = '/bjym/module/charge.html?token='+_this.token+'&userId='+_this.userId+'&productId='+_this.productId+'&merchantId='+_this.merchantId+'&productName='+_this.productName+'&loanApplySeqId='+res.data.sysSeqId+'&userName='+_this.userName;
                                    },1500);
                                }
                            }, 1500);
                                                        
                            
                        } else {
                            if(res.respCode == '555555'||res.respCode == '777777'){
                                _this.getProduct();
                            }
                            clearInterval(_this.timer);
                            _this.tickTime = 20;
                            _this.isResult = 2;
                            _this.refuse_msg = '小主别灰心，为您推荐更多借款产品';
                            _this.refuse_reson = res.respMsg;
                         
                        }
                    }
                });
            },
            // 借款申请
            toLoan () {
                var _this = this;
                xmy.getData({
                    url: '/gateway/postloan/loan/confirmLoan',
                    type: 'POST',
                    contentType: 'application/json',
                    data: JSON.stringify({
                        loanApplyId: _this.loanApplySeqId
                    }),
                    success: function (res) {
                        if (res.respCode === '000000') {
                            setTimeout(function(){
                                _this.queryLoanResult();
                            },2500);
                        } else if (res.respCode === '060028'){
                            _this.message = '来晚了！今日额度已放完';
                            _this.success = false;
                            _this.succp = false;
                            _this.showMsg = true;
                            _this.faildMsg = '小主别灰心，明日早点来借款吧';
                            _this.isResult = 3;
                            _this.isFk = false;
                        } else if(res.respCode === '060021'){
                            _this.success = true;
                            _this.succp = false;
                            _this.message = '处理中...';
                            _this.bank_suc = false;
                            _this.showMsg = true;
                            _this.isCard = false;
                            _this.faildMsg = '系统正在放款中，到账时间大约需要24小时 有任何问题请联系在线客服，感谢您的支持';
                            _this.isResult = 3;
                            _this.isFk = false;
                        }else{
                             _this.message = '借款失败';
                            _this.success = false;
                            _this.succp = false;
                            _this.showMsg = true;
                            _this.faildMsg = res.respMsg;
                            _this.isResult = 3;
                            _this.isFk = false;
                        }
                    }
                });
            },
            // 借款结果
            queryLoanResult () {
                var _this = this;
                xmy.getData({
                    url: '/gateway/postloan/loan/queryLoanResult',
                    type: 'POST',
                    contentType: "application/json",
                    data: JSON.stringify({
                        loanApplyId: _this.loanApplySeqId
                    }),
                    success: function (res) {
                        if (res.respCode === '000000') {

                            if (res.data.loanStatus === 'W') {
                                _this.isResult = 3;
                                _this.message = '借款成功';
                                _this.success = true;
                                _this.succp = true;
                                _this.bank_suc = true;
                                _this.isCard = true;
                                _this.actualAmt = res.data.paymentAmt;
                                _this.bankName = res.data.bankName;
                                _this.tradeAmt = res.data.protAmt;
                                _this.bankCard = res.data.bankCode.substring(res.data.bankCode.length - 4);
                                _this.isFk = false;
                            } else if (res.data.loanStatus === 'P' || res.data.loanStatus === 'I') {
                                _this.success = true;
                                _this.succp = false;
                                _this.message = '处理中...';
                                _this.bank_suc = false;
                                _this.showMsg = true;
                                _this.isCard = false;
                                _this.faildMsg = '系统正在放款中，到账时间大约需要24小时 有任何问题请联系在线客服，感谢您的支持';
                                _this.isResult = 3;
                                _this.isFk = false;
                            } else {
                                _this.message = '借款失败';
                                _this.success = false;
                                _this.succp = false;
                                _this.showMsg = true;
                                _this.faildMsg = res.data.loanMsg;
                                _this.isResult = 3;
                                _this.isFk = false;
                            }
                           
                        }
                    }
                });
            },
            // 得到产品
            getProduct:function(){
                var _this = this;
                xmy.getData({
                    url: '/gateway/preloan/product/query/list/refuse',
                    data:{
                        productId: _this.productId
                    },
                    success:function(res){
                        if(res.respCode == '000000'){
                            _this.productList = res.data;
                        }
                    }
                })
            },
            // 进入产品详情
            goAuth:function(productId,productName){
                window.location.href = '/app-huopan/goauth?productId='+productId+'&productName='+productName;
            },
            
            //风控返回首页
            gohome () {
               window.location.href = '/app-huopan/gohome';
            },

            //重新获取额度
            reSubmit () {
                this.tickTime = 20;
                this.isRefuse = false;
                this.verificationCredit();
            }
        },
        

        mounted() {
            
            //获取额度
            this.verificationCredit();

            //倒计时
            this.cuntdown();
        }
    }
</script>

<style lang="less">
    @import url('../../common/css/init.less');
</style>

