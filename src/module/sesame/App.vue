<template>
    <div id="app">
        <div class="content-pro">
            <navigation v-show="sequence == 1">
                <a href="javascript:;" @click="showMask = true" slot="navigation_goback" class="navigation_goback"></a>
                <span slot="navigation_title" class="navigation_title">芝麻认证</span>
            </navigation>
            <navigation  v-show="sequence == 2">
                <a href="javascript:;" @click="start" slot="navigation_goback" class="navigation_goback"></a>
                <span slot="navigation_title" class="navigation_title">用户授权服务协议</span>
            </navigation>
            <navigation  v-show="sequence == 3">
                <a href="javascript:;" @click="showMask = true" slot="navigation_goback" class="navigation_goback"></a>
                <span slot="navigation_title" class="navigation_title">认证结果</span>
            </navigation>
            <div class="start" v-show="first == 1">
                <img src="../../../static/images/banner1_xyk@3x.png"/>
                <ul>
                    <li>
                        <span>姓名</span>
                        <em>{{userName}}</em>
                    </li>
                    <li>
                        <span>身份证号</span>
                        <em>{{idCard}}</em>
                    </li>
                </ul>
                <p>以上信息用于确认您的身份</p>
                <div class="btn" @click="start">
                    <a href="javascript:;">开始认证</a>
                </div>
            </div>
            <div class="agree" v-show="first == 2">
                <p>1.你同意芝麻服务协议并开通芝麻信用服务;</p> 
                <p>2.基于贵友租车向你提供授信服务的需要，你授权其向芝麻信用查询你的芝麻分或信用评估结果。</p>
                <h2>特别注意</h2>
                <p>当你发生违约或其他影响信用的不良行为时:</p>
                <p>1、你授权芝麻信用可根据贵友租车的查询请求，提供可以联系到你的电话号码、地址等联系信息，用于向你追究违约责任。</p>
                <p>2、你信用状况的好坏，可能对你的贷款等信用生活服务产生影响。</p>
                <div class="btn" @click="next">
                    <a href="javascript:;">开始认证</a>
                </div>
                <p @click="look">*点击“同意授权”即表示同意<a href="javascript:;">《用户授权服务协议》</a></p>
            </div>
            <div class="submit" v-show="first == 4">
                <img src="../../../static/images/banner1_xyk@3x.png"/>
                <ul>
                    <li>
                        <span>姓名</span>
                        <em>**{{usernames}}</em>
                    </li>
                    <li>
                        <span>身份证号</span>
                        <em>{{idcards}}</em>
                    </li>
                    <li>
                        <span>手机号</span>
                        <em>{{mobile}}</em>
                    </li>
                </ul>
                <div class="btn" @click="submitAuth">
                    <a href="javascript:;">提交</a>
                </div>
            </div>
            <div class="agreement" v-show="first == 3">
                <p style="text-alian:center;"><strong>用户授权服务协议</strong></p>
                <p><strong>【重要提示】：</strong></p>
                <p>为了保障您的合法权益，请您务必审慎阅读、充分理解本授权书之条款、承诺，特别是涉及被授权方（包括但不限于杭州旭咪网络科技有限公司、及相关合作方、关联方等，下同）责任的条款，或限制您权利的条款，或加粗条款除非您已阅读、接受并自愿遵守本授权书所有的条款，否则您无权使用被授权方提供的查询服务。您的申请、使用或登录等行为，亦或点击、勾选“确认”、“同意”、“申请”或“查询”按钮，亦或填写资料申请服务等，即表明您对本授权书含义及法律后果的知悉和全面理解，并自愿签署并遵照执行。</p>
                <br>
                <p><strong>【致被授权方】</strong></p>
                <p>（本人拟向__南京百家有米网络科技有限公司__（下简称“百家有米”）申请其提供的服务（包括但不限于融资服务、借贷咨询、借贷方案推荐、助贷服务、分期服务、委托服务、代办服务等，下同），需查询本人的芝麻信用数据。现本人不可撤销的授权：1）授权被授权方收集、存储、使用本人提交或本人授权百家有米提交的姓名、身份证号码、手机号码等个人资料；2）授权被授权方根据芝麻信用管理有限公司（下简称“芝麻信用公司”）的规则向芝麻信用公司发起登录、收集、查询等服务请求，代为查询、收集、存储芝麻信用公司反馈的本人芝麻信用数据，并授权被授权方传输给百家有米用于存储、分析使用、评估本人情况；必要时授权百家有米可同时向本人所申请服务之实际提供方反馈本人之个人信息及评估结果。本人知悉，本授权书包含了被授权方收集、使用、查询、存储和传输本人芝麻信用数据的条款，并接受被授权方及百家有米的规则/评估体系/评估方法。就上述授权，本人同时确认并承诺如下：1.本人知悉，向被授权方/百家有米提供的本人之个人信息（包括但不限于姓名、手机号码、身份证号码、芝麻信用数据等，下同）属于本人重要的个人信息，本人同意并不可撤销地授权被授权方在保证本人信息安全的前提下，存储、使用本人之个人信息，并仅向本人授权的相关方提供，因被授权方提供该等服务所得收益归被授权方所有。除前述情况及因国家机关、生效判决等强制性要求而必须提供外，被授权方不应将本人之个人信息提供给任何未经本人授权的其他机构或平台。2. 本人知悉，被授权方会采用行业通行安全技术标准保护本人之个人信息，以防止个人信息及数据丢失、被盗用或遭窜改，除非遇到不可抗力（包括但不限于黑客攻击、系统故障、软硬件故障、自然灾害、政策变动等）的影响。
                    3. 本授权委托书系本人做出的单方承诺，效力具有独立性，不因其他合同的任何条款无效而失效。
                    4. 以上授权期限为本人作出本授权承诺之日起至本授权委托书约定事项完成之日止。
                    5. 若本人与被授权方发生任何纠纷或争议，首先应友好协商解决；协商不成的，本人同意将纠纷或争议提交杭州仲裁委员会仲裁解决。本授权书的成立、生效、履行、解释及纠纷解决，适用中华人民共和国大陆地区法律（不包括冲突法）。
                    6. 本人已知悉本授权书所有内容（特别是加粗字体内容）的意义以及由此产生的法律后果，自愿作出上述授权，本授权书是本人真实的意思表示，本人同意承担由此带来的一切法律后果。
                    7. 本授权书自填写个人资料，或点击、勾选“确认”、“同意”、“申请”或“查询”等按钮之日起生效。
                    特此授权。</p>
            </div>
            <div class="result" v-show="first == 5">
                <div v-if="result == 1">
                    <span>{{count}}~100%</span>
                    <img class="round" src="../../../static/images/icon_rzz@3x.gif"/>
                    <h2>认证中...</h2>
                    <p>请耐心等待一下，马上就好</p>
                </div>
                <div v-if="result == 2">
                    <img src="../../../static/images/icon_ok@2x.png"/>
                    <h2>认证成功</h2>
                    <p>恭喜您，已完成芝麻信用的认证</p>
                    <a href="javascript:;" @click="reback">返回</a>
                </div>
                <div v-if="result == 3">
                    <img src="../../../static/images/icon_refuse@2x.png"/>
                    <h2>认证失败</h2>
                    <p>{{loser}}</p>
                    <a href="javascript:;" @click="reback">返回</a>
                </div>
            </div>
            <div class="body-mask" v-show="showMask" @click="showMask = false" style="display: none;"></div>
            <div class="popup" v-show="showMask">
                <h6>不继续认证？</h6>
                <h5>马上就成功了，请再想想哟</h5>
                <div class="choose">
                    <a href="javascript:;" @click="reback">不认证了</a>
                    <a href="javascript:;" class="continue" @click="showMask = false">继续认证</a>
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
    data (){
        return {
            userName: xmy.getQueryString("userName"),
            idCard: xmy.getQueryString("idCard"),
            mobile: xmy.getQueryString("mobile"),
            usernames: '',
            idcards: '',
            sequence: 1,
            first: 1,
            result: 1,
            count: 0,
            loser: '不好意思，认证失败了呢',
            showMask: false
        }
    },

    methods: {
        trim (str) {
            return (str ? str.replace(/(^\s*)|(\s*$)/g,"") : '');
        },
        // 获取用户信息
        getUser(){
            var _this = this;
            var pat=/(\d{3})\d*(\d{4})/;
            _this.idcards=_this.idCard.replace(pat,'$1****$2');
            _this.usernames = _this.userName[_this.userName.length-1]
        },
        // 提交认证
        submitAuth:function(){
            var _this = this;
            var number = _this.getNumber(500,2000);
            xmy.getData({
                url: "/gateway/preloan/userAuth/add/sesame",
                success: function(res){
                    _this.sequence = 3;
                    _this.first = 5;
                    _this.result = 1;
                    var time;
                    time=setInterval(function(){
                        if(_this.count < 100){
                            ++ _this.count;
                        }else{
                            clearInterval(time);
                            if(res.respCode == "000000"){
                                setTimeout(function(){
                                    _this.result = 2;
                                },500)
                            }else{
                                setTimeout(function(){
                                    _this.result = 3;
                                    _this.loser = res.respMsg;
                                },500)
                            }
                        }
                    },number/100)
                }
            });
        },
        // 获取随机数
        getNumber (min, max){
            return parseInt(Math.random()*(max-min)) + min;
        },
        // 开始
        start:function(){
            this.sequence = 1;
            this.first = 2
        },
        // 查看协议
        look:function(){
            this.first = 3;
            this.sequence = 2;
        },
        // 开始认证
        next(){
            this.first = 4;
            this.sequence = 1;
        },
        // 返回认证页面
        reback(){
            window.location.href = "/app-huopan/authchk";
        }
    },

    mounted () {
        var _this = this;
        _this.getUser();
    }
}
</script>



<style lang="less" scoped>
@import url(../../common/css/sesame.less);
</style>

