<template>
    <div id="app">
        <div class="content-pro">
            <navigation v-show="sequence == 1">
                <a href="javascript:;" @click="showMask = true" slot="navigation_goback" class="navigation_goback"></a>
                <span slot="navigation_title" class="navigation_title">运营商认证</span>
            </navigation>
            <navigation  v-show="sequence == 2">
                <a href="javascript:;" @click="start" slot="navigation_goback" class="navigation_goback"></a>
                <span slot="navigation_title" class="navigation_title">数据采集服务协议</span>
            </navigation>
            <navigation  v-show="sequence == 3">
                <a href="javascript:;" @click="showMask = true" slot="navigation_goback" class="navigation_goback"></a>
                <span slot="navigation_title" class="navigation_title">认证结果</span>
            </navigation>
            <div class="operators" v-show="first == 1">
                <img class="image" src="../../../static/images/banner3_xyk@3x.png"/>
                <ul>
                    <li>
                        <span>姓名</span>
                        <em>{{userName}}</em>
                    </li>
                    <li>
                        <span>身份证号</span>
                        <em>{{idCard}}</em>
                    </li>
                    <li>
                        <span>手机号</span>
                        <em>{{mobile}}</em>
                    </li>
                </ul>
                <p @click="look">*点击“同意授权”即表示同意<a href="javascript:;">《用户授权服务协议》</a></p>
                <h3>温馨提示</h3>
                <p>1、认证手机号需本人运营商实名认证手机号码；</p>
                <p>2、认证号码必须与注册号码一致；</p>
                <p>3、若遗忘服务密码，可以致电运营商服务电话设置；</p>
                <p>4、若身份信息有误可点击进行修改</p>
                <div class="btn" @click="submitAuth">
                    <a href="javascript:;">同意授权</a>
                </div>
                
                <h4>我们会保护您的信息安全</h4>
            </div>
            <div class="agreement" v-show="first == 2">
                <p style="text-alian:center;"><strong>数据采集服务协议</strong></p>
                <p><strong>【重要提示】：</strong></p>
                <p>本协议是您（下称“用户”、“您”）与杭州魔蝎数据科技有限公司（下称“魔蝎科技”、“我们”）之间就“个人数据授权”相关事宜（下称“本服务”）所订立的协议。</p>
                <p>本协议包含了魔蝎科技采集、存储、保护和使用您的个人数据的条款，我们建议您完整地阅读本协议条款，特别是以下划线、加粗等方式标注与您的权益存在或可能存在重大关系的部分。除非您接受本协议所有条款，否则请不要使用本服务。您点击"同意并继续"按钮后，即表示您已同意我们按照本协议来合法使用和保护您的个人数据。</p>
                <p><strong>为充分保障您个人数据的安全性，魔蝎科技在此特别声明：</strong></p>
                <p>1、鉴于您须授权该服务提供方（下称“商业伙伴”）并由该商业伙伴告知本服务后方能进入并使用本服务，自您使用本服务时起即视为您与商业伙伴之间已存在合法的、充分的、必要的、不可撤销的授权。即您已明确授权商业伙伴为验证您的真实身份及评估您的信用数据而对您的身份数据及相关信用数据进行获取、验证、存储并在约定范围内使用。</p>
                <p>2、魔蝎科技仅是您授权采集和使用您个人数据的技术服务提供方。您在此明确授权并同意，由魔蝎科技对您的数据进行加工、储存、整理并提供给商业伙伴在约定范围内使用。魔蝎科技会采取合理、必要措施以保护您的隐私和数据安全。</p>
                <p>3、未经您的授权，魔蝎科技不会将您的数据提供给任何其他第三方。</p>
                <p>4、魔蝎科技不会保存您的账户密码数据，魔蝎科技仅会在您每次单独授权的情况下，方予采集相应数据，魔蝎科技绝不会主动自行采集您的任何数据。</p>
                <p><strong>第一章 数据采集服务</strong></p>
                <p>第一条 魔蝎科技无法确保您授权采集的个人数据源的准确性、数据采集通道是否持续通畅，因此魔蝎科技无法保证本服务提供的持续性、及时性、准确性，如您发现经魔蝎科技采集的个人数据提供给经您授权的商业伙伴存在不准确或错误的情况，可联系您数据所在平台进行更正或删除。</p>
                <p>第二条 您在此充分地、有效地、不可撤销地明示同意并授权魔蝎科技采集以下个人数据：</p>
                <p>1. 用户注册魔蝎科技账号时，用户根据魔蝎科技要求提供个人注册数据，包括但不限于姓名、性别、身份证号码、住址、联系方式等；</p>
                <p>2. 在用户使用魔蝎科技产品服务，或访问魔蝎科技产品时，魔蝎科技自动接收并记录的用户的浏览器和设备上的信息，包括但不限于用户的宽带类型、IP地址、浏览器的类型、使用的语言、访问日期和时间、软硬件特征等数据；</p>
                <p>3. 用户使用魔蝎科技产品时进行授权登录、导入账单等操作所提供的通讯运营商相关数据，具体如下：</p>
                <p>通讯运营商数据：登记姓名、手机号码、身份证号码（如有）、套餐数据、账单数据、通话记录等其他经用户明确授权的相关数据；</p>
                <p>4. 在用户使用魔蝎科技提供的相关业务时，用户应确认已对商业伙伴（如借贷平台、消费金融公司等，本协议统称为商业伙伴）进行有效的、充分的、不可撤销的授权，授权该等商业伙伴查询、验证用户的相关个人数据并以信用评估为目的进行使用。为实现用户与商业伙伴之间的合作目的，用户及商业伙伴有效地、充分地、不可撤销地授权魔蝎科技对上述商业伙伴需要查询、验证或评估的用户个人数据进行收集、查询验证、处理、并提供给商业伙伴在约定范围内进行使用。魔蝎科技可通过用户提供的个人数据向依法设立的征信机构查询用户的相关信用信息，包括但不限于任何信用记录、信用分、信用报告等信息；</p>
                <p>5. 魔蝎科技不会采集用户的宗教信仰、基因、指纹、血型、疾病和病史数据以及法律、行政法规规定禁止采集的其他个人数据；</p>
                <p><strong>第二章 数据使用</strong></p>
                <p>第三条 魔蝎科技将以高度的勤勉、审慎义务对待用户个人数据，为使用户个人知晓用户数据所经流程，魔蝎科技特告知如下：魔蝎科技在授权范围内为实现用户与商业伙伴之间的合同目的，对授权采集的用户数据将进行以下处理与使用：</p>
                <p>1. 将数据进行归类整合和处理（数据清洗、加密与掩码等），以提供给商业伙伴对用户信用情况进行评估，并依此对魔蝎科技的服务进行改进；</p>
                <p>2. 比较数据的准确性并与第三方进行验证，例如，将用户向魔蝎科技提交的身份数据与身份验证的服务机构进行验证；</p>
                <p>3. 为服务用户的目的，魔蝎科技可能通过使用用户的个人数据，向用户提供用户感兴趣的通知、营销活动及其他商业性电子信息；</p>
                <p>4. 经用户明确同意并授权的其他用途：魔蝎科技将根据所采集的用户数据信息加工形成用户画像并进行安全存储及后续合法使用；</p>
                <p>5. 除本协议明确阐述和相关法律规定外，魔蝎科技不会向任何无关第三方提供、披露或交易用户的个人数据；</p>
                <p><strong>第三章 除外责任</strong></p>
                <p>第四条 因您的过错导致的任何损失，该过错包括但不限于:操作不当、遗忘或泄露密码、密码被他人破解、您使用的计算机系统被第三方侵入、用户委托他人代理使用本服务时他人恶意或不当操作而造成的损失，由您自行负责。除非魔蝎科技单方故意或重大过失直接导致您遭受损失外，魔蝎科技不承担责任。</p>
                <p><strong>第四章 账户安全及管理</strong></p>
                <p>第五条 您了解并同意，确保基于使用本服务所需账户及密码的机密安全对您非常重要，您应妥善保管这些机密数据。您将对利用该账户及密码所进行的一切行动及言论，负完全的责任，并同意以下事项：</p>
                <p>1、您不对其他任何人泄露账户或密码，亦不可使用其他任何人的账户或密码。因黑客、病毒或您的保管疏忽等非魔蝎科技原因导致您的账户遭他人非法使用的，魔蝎科技不承担责任；</p>
                <p>2、冒用他人账户及密码的，魔蝎科技及第三方机构保留追究实际使用人连带责任的权利。</p>
                <p>第六条 如发现有第三人冒用或盗用您的账户及密码，或其他任何未经您合法授权而使用的情形，您应立即以有效方式通知魔蝎科技，且马上修改密码，要求魔蝎科技暂停相关服务。同时，您理解魔蝎科技对您的请求采取行动需要合理期限，在此之前，魔蝎科技对第三人使用该服务所导致的损失不承担责任。</p>
                <p>第七条 魔蝎科技有权基于单方独立判断，在认为可能不利于魔蝎科技提供服务或涉及违法违规等情形时，可不经通知而先行暂停、中断或终止向您提供本协议项下的全部或部分服务，且无需因此对您或任何第三方承担任何责任。</p>
                <p>前述情形包括但不限于：</p>
                <p>1、魔蝎科技认为您不是采集数据的数据权利人时；</p>
                <p>2、魔蝎科技发现异常情况或有合理理由怀疑操作有风险或有违法之虞时；</p>
                <p>3、魔蝎科技认为账户存在未经授权的使用或其他魔蝎科技认为有风险之情形；</p>
                <p>4、魔蝎科技认为您已经违反本协议中规定的各类规则；</p>
                <p>5、魔蝎科技基于上述原因之外，根据其单独判断需先行暂停、中断或终止向您提供本协议项下的全部或部分用户服务。</p>
                <p><strong>第五章 服务中断或故障</strong></p>
                <p>第八条 您同意，因下列原因导致魔蝎科技及第三方机构无法正常提供服务的，魔蝎科技及第三方机构不承担责任；</p>
                <p>1、魔蝎科技及第三方机构等承载本服务系统停机维护期间；</p>
                <p>2、您的电脑，手机软件和通信线路、供电线路出现故障的；</p>
                <p>3、您操作不当或通过非魔蝎科技及第三方机构授权或认可的方式使用本服务的；</p>
                <p>4、因病毒、木马、恶意程序攻击、网络拥堵、系铳不稳定、系统或设备故障、通讯故障、电力故障或政府行为等原因；</p>
                <p>5、由于黑客攻击、网络供应商技术调整或故障、网站升级、网站系统方面的问题等原因而造成的本服务中断或延迟；</p>
                <p>6、因台风、地震、海啸、洪水、停电、战争、恐怖袭击等不可杭力之因素，造成本服务系统障碍不能执行业务的。</p>
                <p>尽管有前款约定，魔蝎科技及第三方机构将采取合理行动积极促使服务恢复正常。</p>
                <p><strong>第六章 隐私权保护及授权条款</strong></p>
                <p>第九条 魔蝎科技不会保存您的密码。</p>
                <p>第十条 您授权魔蝎科技到相应的个人数据源网站采集您的授权个人数据；</p>
                <p>第十一条 本服务的数据获取是一次性的，除非得到您的再次授权，否则魔蝎科技不会发起登录而采集您的数据。</p>
                <p>第十二条 魔蝎科技将对用户提供的数据严格保密，除具备下列情形之一外，不会向任何外部机构披露：</p>
                <p>1. 经过用户事先同意而披露，即在获得用户明确同意的情况下，魔蝎科技才会在授权范围内向特定被授权方披露用户的个人数据；</p>
                <p>2. 应法律法规或公权力部门要求而披露，即魔蝎科技可能会根据法院、政府部门、上级监管机构等执法机构或法律法规的要求向其披露用户的个人数据；</p>
                <p>3. 当魔蝎科技涉及合并、收购或资产出售等重大交易时，魔蝎科技有权依据交易的需要将用户数据提供给交易相对方及交易各方聘请的各中介机构(包括但不限于律师、会计师等)，会在任何个人数据进行转让或受其他隐私权政策约束之前，继续确保其保密性并对受到影响方进行及时通知；</p>
                <p><strong>第七章 条款的解释、法律适用及争端解决</strong></p>
                <p>第十三条 本协议是由您与魔蝎科技共同签订的，适用于您在本服务项下的全部活动。</p>
                <p>第十四条 如本协议中的任何条款无论因何种原因完全或部分无效或不具有执行力，则应认为该条款可与本协议相分割，并可被尽可能接近各方意图的、能够保留本协议要求的经济目的的、有效的新条款所取代，而且，在此情况下，本协议的其他条款仍然完全有效并具有约束力。</p>
                <p>第十五条 本协议的有效性、履行与本协议所有事宜，将受中国法律管辖，任何争议仅适用中国法律。</p>
                <p>第十六条 本协议签订地为中国浙江省杭州市。因本协议所引起的您与魔蝎科技的任何纠纷或争议，首先应友好协商解决，协商不成的，您在此完全同意将纠纷或争议提交魔蝎科技所在辖区法院。</p>
                <p>第十七条 用户如需更正所提供的个人数据，或希望停止接受魔蝎科技提供的服务，或对本协议有任何疑问，请通过以下途径联系魔蝎科技处理：</p>
                <p><strong>邮件：win@51dojo.com</strong></p>
            </div>
            <div class="result" v-show="first == 3">
                <div v-if="result == 1">
                    <span>{{count}}~100%</span>
                    <img class="round" src="../../../static/images/icon_rzz@3x.gif"/>
                    <h2>认证中...</h2>
                    <p>请耐心等待一下，马上就好</p>
                </div>
                <div v-if="result == 2">
                    <img src="../../../static/images/icon_ok@2x.png"/>
                    <h2>认证成功</h2>
                    <p>恭喜您，已完成运营商认证</p>
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
        // 提交认证
        submitAuth:function(){
            var _this = this;
            var number = _this.getNumber(500,2000);
            xmy.getData({
                type: "GET",
                url: "/gateway/preloan/userAuth/add/carrier",
                data:{
                    tag: false
                },
                success: function(res){
                    _this.sequence = 3;
                    _this.first = 3;
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
        // 协议返回
        start:function(){
            this.sequence = 1;
            this.first = 1
        },
        // 查看协议
        look:function(){
            this.first = 2;
            this.sequence = 2;
        },
        // 返回认证页面
        reback(){
            window.location.href = "/app-huopan/authchk";
        }
    },

    mounted () {
        var _this = this;
    }
}
</script>



<style lang="less" scoped>
@import url(../../common/css/operators.less);
</style>

