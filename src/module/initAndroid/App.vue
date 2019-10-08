<template>
    <div id="app">
        <div class="result">
            <div class="loan_result">借款结果</div>
            <div class="small-text">
                <img src="../../../static/images/icon_refuse@2x.png">
                <h3>{{refuse_reson}}</h3>
                <h5>小主别灰心，为您推荐更多借款产品</h5>
                <!-- <div class="result_adv" @click="goList"><a href="javascript:;">({{tipText}}s)后将跳转到推荐借款</a></div> -->
                <div class="result_adv" @click="refuseGoHome"><a href="javascript:;">返回首页</a></div>

                <!-- <div class="smallBtn">
                    <a href="javascript:;" @click="showPop">返回首页</a>
                </div> -->
            </div>
        </div>
        <!-- <div class="show-mask" v-show="showMask"></div>
        <div class="realy" v-show="showMask">
            <h2>已有3888897人借款成功</h2>
            <p>你知道吗？申请过的人都已成功</p>
            <div class="btn">
                <button class="cancelBtn" @click="refuseGoHome">再看看</button>
                <button class="sureBtn"  @click="goList">查看推荐</button>
            </div>
        </div> -->
    </div>
</template>

<script>
    import xmy from '../../../static/js/xmy.js';
    
    export default {
        data () {
            return {
                token: xmy.getQueryString('token'),
                userId: xmy.getQueryString('userId'),
                refuse_reson: decodeURI(xmy.getQueryString('refuseReson'))?decodeURI(xmy.getQueryString('refuseReson')):"失败了",
                showMask: false,
                timeOut: "",
                tipText: 2
            }
        },

        methods: {
            // 返回首页打开弹窗并且清除定时器
            // showPop () {
            //     var _this = this;
            //     _this.showMask = true;
            //     clearInterval(_this.timeOut);
            // },
            // 查看推荐,点击查看更多推荐按钮埋点
            // 点击banner埋点
            // goList () {
            //     var _this = this;
            //     clearInterval(_this.timeOut);
            //     let params={
            //         token: _this.token,
            //         userId: _this.userId,
            //         buriedNo: 'Click_Loan_Super_Banner'
            //     }
            //     xmy.buried(params,function(){
            //         window.location.href = "/api/static/bjym/module/loanList.html?token="+_this.token+"&userId="+_this.userId;
            //     });
            // },
            // 借款失败返回首页埋点
            refuseGoHome () {
                var _this = this;
                xmy.buried({
                    token: _this.token,
                    userId: _this.userId,
                    buriedNo: 'Click_Back_Home'
                },function(){
                    window.location.href = "/api/static/app/gohome";
                });
            },
        },
        

        mounted() {
            var _this = this;
            // _this.timeOut = setInterval(function(){
            //     _this.tipText--;
            //     if(_this.tipText <= 0) {
            //         clearInterval(_this.timeOut);
            //         window.location.href = "/api/static/bjym/module/loanList.html?token="+_this.token+"&userId="+_this.userId;
            //     }
                
            // },1000)
        }
    }
</script>

<style lang="less">
    @import url('../../common/css/init.less');
</style>

