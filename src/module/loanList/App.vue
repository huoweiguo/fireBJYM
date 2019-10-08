<template>
    <div id="app_body">
        <div class="show-mask" v-show="showMask"></div>
        <navigation>
            <a href="javascript:;" @click="showMask=!showMask" slot="navigation_goback" class="navigation_goback"></a>
            <span slot="navigation_title" class="navigation_title">借款列表</span>
        </navigation>
        <!-- <img :src="bannerUrl"/> -->
        <h2>借款推荐</h2>
        <div class="product">
            <a v-for="(item,index) in productList" :key="index" href="javascript:;" :data-id="item.id" @click="goOther(item.diversionUrl,item.id)">
                <div class="head">
                    <img class="logo" :src="item.logoUrl"/>
                    <span>{{item.productName}}</span>
                    <!-- TODU:图片标签 -->
                    <template v-if="item.tagUrl">
                        <img class="tag" :src="item.tagUrl"/>
                    </template>
                </div>
                <div class="list">
                    <div class="money">
                        <span>{{item.loanMinAmt}}~{{item.loanMaxAmt}}</span>
                        <em>额度(元)</em>
                    </div>
                    <div class="date">
                        <span>{{item.loanPeriod}}</span>
                        <em>期限(天)</em>
                    </div>
                    <div class="results">
                        <p>成功率<strong>{{item.successRate}}%</strong></p>
                        <div class="bac">
                            <p :style="{width: item.successRate + '%'}"></p>
                        </div>
                        <div class="text">{{item.loanNumber}}人已放款</div>
                    </div>
                </div>
                <div class="desc">
                    <em>{{item.productDesc}}</em>
                </div>
            </a>
        </div>
        <div class="realy" v-show="showMask">
            <h2>已有3888897人借款成功</h2>
            <p>你知道吗？申请过的人都已成功</p>
            <div class="btn">
                <button class="cancelBtn" @click="goBack">确认返回</button>
                <button class="sureBtn"  @click="continueApply">继续申请</button>
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
            return {
                showMask: false,
                token: xmy.getQueryString('token'),
                userId: xmy.getQueryString('userId'),
                bannerUrl:'',
                size: "10",
                current: 1,
                isAll: false,
                isWetherAll: true,
                productList: []
            }   
        },

        methods: {
            // 点击确认返回埋点
            goBack () {
                var _this = this;
                xmy.buried({
                    token: _this.token,
                    userId: _this.userId,
                    buriedNo: 'Click_Dc_Application_Waiver'
                },function(){
                    window.location.href = "/api/static/app/gohome";
                });
            },
            // 点击继续申请
            continueApply () {
                var _this = this;
                xmy.buried({
                    token: _this.token,
                    userId: _this.userId,
                    buriedNo: 'Click_Dc_Continue_Apply'
                },function(){
                    _this.showMask = !_this.showMask;
                });
            },
            // 获取产品列表
            getList () {
                var _this = this;
                if(_this.isAll){
                    return false;
                }
                $.ajax({
                    url: '/gateway/api/user/diversion/proDivInfoList?t='+(new Date()).getTime(),
                    type: 'POST',
                    contentType: "application/json",
                    data: JSON.stringify({
                        token: _this.token,
                        isShow: "Y",
                        current: _this.current,
                        size: _this.size,
                        type: "1"
                    }),
                    success: function(res){
                        _this.isWetherAll = true;
                        _this.productList = _this.productList.concat(res.records);
                        if(_this.current ==1 && res.records.length ==0){
                            _this.isAll = true;
                        }
                        if(_this.current < res.pages){
                            _this.isAll = false;
                        }else{
                            _this.isAll = true;
                        }
                    }
                })
            },
            // 获取banner图
            getBanner () {
                var _this = this;
                $.ajax({
                    url: '/gateway/api/user/diversion/proDivImgList?t='+(new Date()).getTime(),
                    type: 'POST',
                    contentType: "application/json",
                    data: JSON.stringify({
                        token: _this.token,
                        imgType: 1,
                        isShow: "Y",
                        type: 1
                    }),
                    success: function(res){
                        _this.bannerUrl = res.data[0].imgUrl;
                    }
                })
            },
            // 点击贷超产品，添加埋点，跳转，获取产品信息接口
            goOther (diversionUrl,productId) {
                var _this = this;
                xmy.buried({
                    token: _this.token,
                    userId: _this.userId,
                    buriedNo: 'Click_Loan_Super_' + productId
                },function(){
                    var regExp = new RegExp('&amp;','ig');
                    var url = diversionUrl.replace(regExp, '&');
                    xmy.setCookie("diversionUrl",url)
                    window.location.href = "outside.html";
                });
            }
        },
        mounted () {
            var _this = this;
            _this.getBanner ();
            _this.getList ();
            xmy.buried({
                    token: _this.token,
                    userId: _this.userId,
                    buriedNo: 'Product_list_Enter'
            })
            window.onscroll = function(){
                let scrollTop = xmy.getScrollTop(),
                    clientHeight = xmy.getClientHeight(),
                    scrollHeight = xmy.getScrollHeight();
                if(scrollTop + clientHeight + 70 > scrollHeight) {
                    if(!_this.isAll && _this.isWetherAll){
                        _this.isWetherAll = false;
                        _this.current ++;
                        _this.getList ();
                    }
                }
            }
        }
    }
</script>

<style lang="less">
    @import url('../../common/css/loanList.less');
</style>