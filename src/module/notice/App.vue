<template>
    <div id="app">
        <!-- <navigation>
            <a href="javascript:;" @click="goback" slot="navigation_goback" class="navigation_goback"></a>
            <span slot="navigation_title" class="navigation_title">公告详情</span>
        </navigation> -->
        <div class="notice">
            <div class="notice-content">
                <!-- <h2>{{title}}</h2>
                <div class="n-time">{{time}}</div> -->
                <div class="n-text" v-html="context"></div>
            </div>
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
                context: '',
                title: '',
                time: '',
                noticeId: xmy.getQueryString('noticeId'),
                token: xmy.getQueryString('token'),
                userId: xmy.getQueryString('userId'),
                merchantId: xmy.getQueryString('merchantId')
            }
        },

        methods: {
            goback () {
                window.location.href = '/app-huopan/noticelist';
            }
        },

        mounted () {
            var _this = this;
            xmy.getData({
                url: '/gateway/preloan/noticeInfo/info',
                type: 'POST',
                contentType: 'application/json',
                data: JSON.stringify({
                    noticeId: _this.noticeId,
                    userId: _this.userId,
                    token: _this.token,
                    merchantId: _this.merchantId
                }),
                success: function (res) {
                    _this.title = res.data.title;
                    _this.time = res.data.updateTime;
                    _this.context = decodeURIComponent(res.data.content);
                }
            });
        }
    }
</script>

<style lang="less">
    .notice {
        padding:0.22rem 0.15rem;
        .notice-content{
            width: 100%;
            overflow: hidden;
        }
        h2 {
            color: #000117;
            text-align: center;
            font-size: 0.2rem;
            line-height: 0.3rem;
            font-weight: normal;
            margin-bottom: 0.12rem;
        }
        .n-time {
            color: #9b9b9b;
            font-size: 0.12rem;
            text-align: center;
            margin-bottom: 0.2rem;
        }
        .n-text {
            font-size: 0.14rem;
            color: #4a4a4a;
            line-height: 0.26rem;
            padding: 0.1rem 0 0.2rem;
        }
        .n-text img{
            max-width: 3.5rem;
        }
    }
</style>

