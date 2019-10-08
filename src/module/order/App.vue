<template>
    <div id="app">
        <div class="body-mask"></div>
        <!-- <navigation>
            <span slot="navigation_title" class="navigation_title">还款</span>
        </navigation> -->
        <div class="loan_mask" v-show="showMask" @click="showMask = false"></div>
        <!-- 借款记录 -->
        <div v-show="isAll">
            <ul class="order_list">
                <template v-for="item in allList">
                    <!-- 锁定中 -->
                    <li v-if="item.status == 'P'" :key="item.sysSeqId" @click="showMask = true">
                        <div class="li_nav">
                            <div class="order_user">
                                {{item.productName}}
                            </div>

                            <div class="order_status">
                                处理中
                                <img src="../../../static/images/rightarrow_1f.png"/>
                            </div>
                        </div>
                        <div class="li_desc">
                            <div class="order_money">
                                <p>未还金额(元)</p>
                                <span>{{item.pendingRepayAmt}}</span>
                            </div>
                            <div class="order_date">
                                <p>借款期限：{{item.loanPeriods}}</p>
                                <p>借款时间：{{item.loanDate}}</p>
                            </div>
                        </div>
                    </li>
                    <!-- 单期已结清 -->
                    <li v-if="item.status == 'S' && item.loanPeriods[item.loanPeriods.length-1] == '天'" :key="item.sysSeqId" @click="goOrderDetailOver(item.singleRepayPlanId)">
                        <div class="li_nav">
                            <div class="order_user">
                                {{item.productName}}
                            </div>

                            <div class="order_status">
                                已结清
                                <img src="../../../static/images/rightarrow_1f.png"/>
                            </div>
                        </div>
                        <div class="li_desc">
                            <div class="order_money">
                                <p>已还总额(元)</p>
                                <span>{{item.repayAmt}}</span>
                            </div>
                            <div class="order_date">
                                <p>借款期限：{{item.loanPeriods}}</p>
                                <p>借款时间：{{item.loanDate}}</p>
                            </div>
                        </div>
                    </li>
                    <!-- 多期已结清 -->
                    <li v-if="item.status == 'S' && item.loanPeriods[item.loanPeriods.length-1] != '天'" :key="item.sysSeqId" @click="goOrderDetailAllOver(item.sysSeqId,item.repayAmt)">
                        <div class="li_nav">
                            <div class="order_user">
                                {{item.productName}}
                            </div>

                            <div class="order_status">
                                已结清
                                <img src="../../../static/images/rightarrow_1f.png"/>
                            </div>
                        </div>
                        <div class="li_desc">
                            <div class="order_money">
                                <p>已还总额(元)</p>
                                <span>{{item.repayAmt}}</span>
                            </div>
                            <div class="order_date">
                                <p>借款期限：{{item.loanPeriods}}</p>
                                <p>借款时间：{{item.loanDate}}</p>
                            </div>
                        </div>
                    </li>
                    <!-- 使用中单期 -->
                    <li v-if="(item.status == 'W' || item.status == 'B') && item.loanPeriods[item.loanPeriods.length-1] == '天'" :key="item.sysSeqId" @click="goOrderDetail(item.sysSeqId,item.singleRepayPlanId,item.productId)">
                        <div class="li_nav">
                            <div class="order_user">
                                {{item.productName}}
                            </div>

                            <div class="order_status">
                                <em>使用中</em>
                                <img src="../../../static/images/rightarrow_1f.png"/>
                            </div>
                        </div>
                        <div class="li_desc">
                            <div class="order_money">
                                <p>未还总额(元)</p>
                                <span>{{item.pendingRepayAmt}}</span>
                            </div>
                            <div class="order_date">
                                <p>借款期限：{{item.loanPeriods}}</p>
                                <p>借款时间：{{item.loanDate}}</p>
                            </div>
                        </div>
                    </li>
                    <!-- 使用中多期 -->
                    <li v-if="(item.status == 'W' || item.status == 'B') && item.loanPeriods[item.loanPeriods.length-1] != '天'" :key="item.sysSeqId" @click="goOrderDetailAll(item.sysSeqId,item.pendingRepayAmt,item.productId)">
                        <div class="li_nav">
                            <div class="order_user">
                                {{item.productName}}
                            </div>

                            <div class="order_status">
                                <em>使用中</em>
                                <img src="../../../static/images/rightarrow_1f.png"/>
                            </div>
                        </div>
                        <div class="li_desc">
                            <div class="order_money">
                                <p>未还总额(元)</p>
                                <span>{{item.pendingRepayAmt}}</span>
                            </div>
                            <div class="order_date">
                                <p>借款期限：{{item.loanPeriods}}</p>
                                <p>借款时间：{{item.loanDate}}</p>
                            </div>
                        </div>
                    </li>
                </template>
            </ul>
            <div class="nodata" v-show="visAll">
                <img src="../../../static/images/group7@2x.png">
                暂无借款
                <a href="/app-huopan/gohome" class="btn">去借款</a>
            </div>
        </div>
        <div class="popup" v-show="showMask">
            <img src="../../../static/images/pic1@3x.png"/>
            <a href="javascript:;" @click="showMask = false">知道了</a>
        </div>
    </div>
</template>

<script>
    // import navigation from '../../components/navigation.vue';
    import xmy from '../../../static/js/xmy.js';
    export default {
        // components: {
        //     navigation: navigation
        // },

        data () {
            return {
                isAll: true,
                showAll: false,
                visAll: false,
                allList: [],
                curAll: 1,
                token: xmy.getQueryString('token'),
                userId: xmy.getQueryString('userId'),
                isWetherAll: true,
                merchantId: xmy.getQueryString('merchantId'),
                showMask: false
            }
        },  


        methods: {

            //渲染全部订单列表
            renderAll () {
                let _this = this;
                
                if(this.showAll || this.visAll){
                    return false;
                }
                xmy.getData({
                    url: '/gateway/postloan/loan/queryBillOrderList',
                    type:"POST",
                    contentType: "application/json",
                    data: JSON.stringify({
                        token: _this.token,
                        userId: _this.userId,
                        size: 10,
                        merchantId: xmy.getQueryString('merchantId'),
                        current: _this.curAll
                    }),
                    success: function(res){
                        if(res.respCode === '000000'){
                            _this.allList = _this.allList.concat(res.data.records);
                            if(_this.curAll == 1 && res.data.records.length == 0){
                                _this.visAll = true;
                            }

                            if(res.data.records.length > 0 && res.data.records.length < 10){
                                _this.showAll = true;
                            }else{
                                _this.showAll = false;
                            }
                            console.log(_this.allList);
                        }else{
                            for(var i=0;i<res.data.records.length;i++){
                                if(res.data.records[i].status != 'F'){
                                    _this.visAll = true;
                                }
                            }
                            
                        }

                        _this.isWetherAll =  true;
                    }
                });
            },
            // 进入详情页,单期，已结清
            goOrderDetailOver (orderId) {
                let _this = this;
                window.location.href = "/bjym/module/orderDetail.html?token="+_this.token+"&userId="+_this.userId+"&orderId="+orderId+"&merchantId="+ _this.merchantId;
            },
            // 进入详情页，多期，已结清
            goOrderDetailAllOver (orderId,amt) {
                let _this = this;
                window.location.href = "/bjym/module/instalmentDetail.html?token="+_this.token+"&userId="+_this.userId+"&orderId="+orderId+"&amt="+amt+"&merchantId="+ _this.merchantId;
            },
            // 进入详情页,单期，使用中
            goOrderDetail (orderId,repayPlanId,productId) {
                let _this = this;
                window.location.href = "/bjym/module/repayDetail.html?token="+_this.token+"&userId="+_this.userId+"&orderId="+orderId +'&repayPlanId='+ repayPlanId+"&merchantId="+ _this.merchantId+"&productId="+productId;
            },
            // 进入详情页，多期，使用中
            goOrderDetailAll (orderId,amt,productId) {
                let _this = this;
                window.location.href = "/bjym/module/orderInstalment.html?token="+_this.token+"&userId="+_this.userId+"&orderId="+orderId+"&amt="+amt+"&merchantId="+ _this.merchantId+"&productId="+productId;
            }
        },

        mounted () {
            var _this = this;
            // 进入埋点
            // let typeLink = xmy.getQueryString('linkType');
            // if(typeLink == 'mycenter'){
            //     let params={
            //         token: _this.token,
            //         userId: _this.userId,
            //         buriedNo: 'Center_Order_Enter'
            //     }
            //     xmy.buried(params);
            // }
            // if(_this.orderTab == 3){
            //     _this.isAll = false;
            //     _this.isDeal = true;
            //     _this.isComplete = false;
            // }
            this.renderAll();
            // this.renderDeal();
            // this.renderComplete();

            window.onscroll = function(){
                let scrollTop = xmy.getScrollTop(),
                    clientHeight = xmy.getClientHeight(),
                    scrollHeight = xmy.getScrollHeight();

                if(scrollTop + clientHeight + 70 > scrollHeight) {
                    if(_this.isAll && _this.isWetherAll){
                        _this.isWetherAll =  false;
                        _this.curAll++;
                        _this.renderAll();
                    }

                    // if(_this.isDeal && _this.isWetherDeal){
                    //     _this.isWetherDeal =  false;
                    //     _this.curDeal++;
                    //     _this.renderDeal();
                    // }

                    // if(_this.isComplete && _this.isWetherComplete){
                    //     _this.isWetherComplete =  false;
                    //     _this.curComplete++;
                    //     _this.renderComplete();
                    // }
                    
                }
            };

            /*
                layer.open({
                    type: 2,
                    content: '加载中...',
                    shadeClose: false,
                    className: 'layer-loading'
                });

                setTimeout(function(){
                    layer.closeAll();
                },2000);
            */
        }
    }   
</script>

<style lang="less" scoped>
    @import '../../common/css/order.less';
</style>
