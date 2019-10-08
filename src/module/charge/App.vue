<template>
    <div id="app">
        <div class="charge_nav">
            <a href="javascript:;" class="goback" @click="goback"></a>
            <span>借款详情</span>
        </div>
        <div class="charge_content">
            <div class="bg-blue"></div>
            <!--额度金额-->
            <div class="quota">
                
                <div class="qu_list">
                    <div>
                        <span>借款金额</span>
                        <p>{{account}}元</p> 
                    </div>
                    <div>
                        <span>期限</span>
                        <p>{{date}}{{dw}}</p>
                    </div>
                </div>

                <div class="loan-prod">
                    <span>借款产品</span>
                    <em>{{productName}}</em>
                </div>
            </div>


            <div class="detail-list">
                <h4>费用说明</h4>
                <ul>
                    <li><span>综合费用</span><i>{{serviceAmt}}元</i></li>
                    <li><span>到账金额</span><i>{{accountAmt}}元</i></li>
                    <li><span>到期应还</span><i>{{totalPayAmt}}元</i></li>
                    <li><span>还款计划</span><i class="look" v-on:click="lookPlan">查看</i></li>
                </ul>
            </div>

            <div class="detail-list" @click="changeCard">
                <h4>银行卡信息</h4>
                <ul>
                    <li><span>到账银行卡</span><i>尾号{{defaultCard}}</i></li>
                    <li><span>所属银行</span><i>{{defaultCardName}}</i></li>
                </ul>
            </div>

            <div class="charge_agree">
                <template v-if="contractShowFlag != 'N'" >
                    <em class="protocol">*点击“同意借款”即表示同意签署</em><a :href="protocolPage" class="a_protocol">《借款合同及相关协议》</a>
                </template>
                <div class="agree_btn_set">
                    <a href="javascript:;" class="a_btn2" v-on:click="toLoan" v-show="isClick">同意借款</a>
                    <a href="javascript:;" class="a_btn2" v-show="!isClick">放款中···</a>
                </div>
            </div>

        </div>
        <!--借款结果-->
        
        <div class="result" v-show="result">
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
        

        <div class="loan_mask" v-show="toLeave"></div>
        <div class="go_combox" v-show="toLeave">
            <h3>确认不借款了吗？</h3>
            <div class="loan_small">闪电到账银行卡，只差这最后一步</div>
            <div class="loan_set">
                <a href="javascript:;" @click="sureReturn">确认</a>
                <a href="javascript:;" @click="loankv" class="loankv">考虑下</a>
            </div>
        </div>

        <div class="repay_mask" v-on:click="closePlan" v-show="toplan"></div>
        <div class="repay_box" :class="{'strenth': toplan == true}">
            <h2 class="repay_box_nav">还款计划 <a href="javascript:;" v-on:click="closePlan"></a></h2>
            <div class="repay-small">温馨提示：次日起可提前还款</div>
            <div class="replay_content">
                <span>还款金额(元)</span>
                <h2 class="repay-money">{{totalPayAmt}}</h2>
                <div class="repay-rate">总利息{{interestPayAmt}}元</div>
                <div class="repay-list">
                    <ul>
                        <li v-for="(item, index) in planList" :key="index">
                            <div class="li-index">{{index + 1}}</div>
                            <div class="li-date">{{item.periodsEndDate}}</div>
                            <div class="li-money">
                                <span>{{item.periodsShldRepayAmt}}元</span>
                                <em>含本金{{item.periodsPortAmt}}元，利息{{item.periodsInterestRateAmt}}元</em>
                            </div>
                        </li>
                    </ul>
                </div>
                
            </div>
        </div>
    </div>
</template>

<script>
    import '../../common/css/charge.less';
    import xmy from '../../../static/js/xmy.js';
    export default {
        data () {
            return {
                money: "",
                account: 0,
                repayment: 0,
                date: '',
                dw: '天',
                ischk: true,
                result: false,
                success: false,
                token: xmy.getQueryString('token'),
                userId: xmy.getQueryString('userId'),
                productId: xmy.getQueryString('productId'),
                productUserId: xmy.getQueryString('productUserId'),
                publishOrderId: xmy.getQueryString('publishOrderId'),
                productName: xmy.getQueryString('productName'),
                merchantId: xmy.getQueryString('merchantId'),
                loanApplySeqId: xmy.getQueryString('loanApplySeqId'),
                message: '',
                faildMsg: '',
                bank_suc: false,
                afterTime: '',
                showMsg: false,
                isCard: true,
                bankName: '',
                tradeAmt: '',
                bankCard: '',
                actualAmt: '',
                toLeave: false,
                protocolPage: '/bjym/module/contract.html?loanUrl=https://shilupan-huopan.oss-cn-shanghai.aliyuncs.com/protocol/template/20190109152759.jpg&userId='+xmy.getQueryString('userId'),
                isClick: true,
                isCharging: true,
                toplan: false,
                defaultCardName: '',
                defaultCard: '',
                succp: true,
                planList: [],
                totalPayAmt: '',
                interestPayAmt: '',
                accountAmt: '',
                contractShowFlag: '',
                goBankCard: "/bjym/module/bankCard.html?token="+xmy.getQueryString('token')+"&type=4&merchantId="+xmy.getQueryString('merchantId')+"&userId="+xmy.getQueryString('userId')+"&loanApplySeqId="+xmy.getQueryString('loanApplySeqId')+"&productId="+xmy.getQueryString('productId')
            }
        },

        methods: {
            goback () {
                this.toLeave = true;
            },

            changeCard:function(){
                window.location.href = this.goBankCard;
            },

            toLoan () {
                var _this = this;
                if (!this.isClick) {
                    return false;
                }
                this.isClick = false;
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
                            _this.result = true;
                            _this.isFk = false;
                        } else if(res.respCode === '060021'){
                            _this.success = true;
                            _this.succp = false;
                            _this.message = '处理中...';
                            _this.bank_suc = false;
                            _this.showMsg = true;
                            _this.isCard = false;
                            _this.faildMsg = '系统正在放款中，到账时间大约需要24小时 有任何问题请联系在线客服，感谢您的支持';
                            _this.result = true;
                            _this.isFk = false;
                        }else{
                             _this.message = '借款失败';
                            _this.success = false;
                            _this.succp = false;
                            _this.showMsg = true;
                            _this.faildMsg = res.respMsg;
                            _this.result = true;
                            _this.isFk = false;
                        }
                    }
                });
            },

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
                        _this.isClick = true;
                        if (res.respCode === '000000') {

                            if (res.data.loanStatus === 'W') {
                                _this.result = true;
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
                                _this.result = true;
                                _this.isFk = false;
                            } else {
                                _this.message = '借款失败';
                                _this.success = false;
                                _this.succp = false;
                                _this.showMsg = true;
                                _this.faildMsg = res.data.loanMsg;
                                _this.result = true;
                                _this.isFk = false;
                            }
                           
                        }
                    }
                });
            },

            init () {
                var _this = this;
                xmy.getData({
                    url: '/gateway/api/order/loan/verificationCredit?t='+(new Date()).getTime(),
                    type: 'POST',
                    data: {
                        token: _this.token,
                        borrowerId: _this.userId,
                        productId: _this.productId,
                        productUserId: _this.productUserId
                    },
                    success: function(res){
                        if(res.respCode == '000000'){
                            _this.money = res.loanExtend.limit;
                            _this.account = res.loanExtend.toAccountAmount;
                            _this.repayment = res.loanExtend.repaymentAmount;
                            _this.date = res.loanExtend.loanPeriodTime;
                            _this.dw = res.loanExtend.loanPeriodUnit;
                        } 
                    }
                });
            },

            loankv () {
                this.toLeave = false;
            },

            sureReturn () {
                window.location.href = '/app-huopan/gohome';
            },

            lookOrder () {
                window.location.href = '/app-huopan/goOrder';
            },

            //查看还款计划
            lookPlan () {
                this.toplan = true;
            },

            closePlan () {
                this.toplan = false;
            },

            findBankCardByUserId () {
                var _this = this;
                xmy.getData({
                    url: '/gateway/preloan/user/findBankCardByUserId',
                    data: {
                        merchantId: _this.merchantId
                    },
                    success: function (res) {
                        if (res.respCode === '000000') {
                            var filterArr = res.data;
                            _this.defaultCard = filterArr[0].bankCard.substring(filterArr[0].bankCard.length - 4);
                            _this.defaultCardName = filterArr[0].bankName;
                        } else {
                            xmy.toast(res.respMsg);
                        }
                    }
                });
            },

            getApplyLoanResult () {
                var _this = this;
                xmy.getData({
                    url: '/gateway/preloan/loanApply/record/getApplyLoanResult',
                    data: {
                        loanApplySeqId: _this.loanApplySeqId,
                        productId: _this.productId
                    },
                    success: function (res) {
                        if (res.respCode === '000000') {
                            _this.setBase(res);
                            _this.setDefaultCard(res);
                            _this.setReplayInfo(res);
                            _this.planList = res.data.billRepayPlanList;
                            _this.contractShowFlag = res.data.contractShowFlag;
                        }
                    }
                });
            },

            setReplayInfo (res) {
                var replayInfo = res.data.replayInfo;
                this.serviceAmt = replayInfo.serviceAmt;
                this.totalPayAmt = replayInfo.totalPayAmt;
                this.interestPayAmt = replayInfo.interestPayAmt;
                this.accountAmt = replayInfo.accountAmt;
            },

            //设置基础信息
            setBase (res) {
                var loanApply = res.data.loanApply;
                this.productName = loanApply.productName;
                this.account = loanApply.productLoanAmt;
                this.dw = loanApply.periodUnit === 'W' ? '周' :  loanApply.periodUnit === 'D' ? '天' : loanApply.periodUnit === 'M' ? '月' : '';
                this.date = loanApply.loanInstalmentsNum;
            },

            //设置默认卡
            setDefaultCard (res) {
                var userBankInfos = res.data.userBankInfos;
                if(!userBankInfos){
                    window.location.href = this.goBankCard;
                }else{
                    this.defaultCardName = userBankInfos.bankName;
                    this.defaultCard = userBankInfos.bankCard.substring(userBankInfos.bankCard.length - 4);
                }
            }
        },

        mounted() {
            let _this = this;
            // this.init();

            //基础信息
            this.getApplyLoanResult();
        }
    }
</script>

