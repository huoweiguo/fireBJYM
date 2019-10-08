<template>
    <div id="app_body" style="-webkit-overflow-scrolling: touch;">
        <div>
            <a href="javascript:window.history.go(-1);" slot="navigation_goback" class="navigation_goback"></a>
            <span slot="navigation_title" class="navigation_title">借款列表</span>
        </div>
        <div class="iframe_content">
            <iframe :src="url"></iframe>
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
                url: xmy.getCookie("diversionUrl")
            }   
        },

        methods: {
            setIframeheight () {
                var iframeBox = document.getElementsByClassName('iframe_content')[0],
                    naviBox = document.getElementsByClassName('navigation_goback')[0].clientHeight,
                    clientH = document.documentElement.clientHeight;
                iframeBox.style.height = clientH - naviBox + 'px';
            }
        },

        mounted() {
            this.setIframeheight();
        }
    }
</script>

<style lang="less">
    #app_body{
        height: 100%;
        .navigation_goback{
            position: absolute;
            left: 0rem;
            top: 0rem;
            display: block;
            width: 0.6rem;
            height: 0.45rem;
            background: url(../../../static/images/leftarrow_1f.png) no-repeat 0.1rem 0.13rem;
            background-size: auto 0.16rem;
            z-index: 100;
        }
        iframe{
            width: 100%;
            height: 100%;
            border: none;
        }
        .iframe_content{
            width: 100%;
            overflow-y:scroll; 
            -webkit-overflow-scrolling: touch;
        }
    }
</style>