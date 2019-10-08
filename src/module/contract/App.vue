<template>
    <div id="app">
        <div class="content-pro">
            <navigation>
                <a href="javascript:window.history.go(-1);" slot="navigation_goback" class="navigation_goback"></a>
                <span slot="navigation_title" class="navigation_title">《借款协议》</span>
            </navigation>
            <!-- <navigation v-show="first == 1">
                <a href="javascript:window.history.go(-1);" slot="navigation_goback" class="navigation_goback"></a>
                <span slot="navigation_title" class="navigation_title">《借款合同及相关协议》</span>
            </navigation>
            <navigation v-show="first == 2">
                <a href="javascript:;" @click="first = 1" slot="navigation_goback" class="navigation_goback"></a>
                <span slot="navigation_title" class="navigation_title">《借款协议》</span>
            </navigation>
            <navigation v-show="first == 3">
                <a href="javascript:;" @click="first = 1" slot="navigation_goback" class="navigation_goback"></a>
                <span slot="navigation_title" class="navigation_title">《委托代扣协议》</span>
            </navigation>
            <navigation  v-show="first == 4">
                <a href="javascript:;" @click="first = 1" slot="navigation_goback" class="navigation_goback"></a>
                <span slot="navigation_title" class="navigation_title">《个人信息授权书》</span>
            </navigation> -->
            <!-- <div v-show="first == 1">
                <div class="list" @click="first = 2">
                    <a href="javascript:;">《借款协议》<img src="../../../static/images/rightarrow_1f.png"/></a>
                </div>
                <div class="list" @click="first = 3">
                    <a href="javascript:;">《委托代扣协议》<img src="../../../static/images/rightarrow_1f.png"/></a>
                </div>
                <div class="list" @click="first = 4">
                    <a href="javascript:;">《个人信息授权书》<img src="../../../static/images/rightarrow_1f.png"/></a>
                </div>
            </div> -->
            <div class="loan">
                <img :src="loanUrl"/>
            </div>
            <!-- <div class="loan" v-show="first == 2">
                <img :src="loanUrl"/>
            </div> -->
            <!-- <div class="main" v-show="first == 3">
                <h2>委托扣款协议</h2>
                <div class="tip">请您认真阅读和充分理解本协议各条款，尤其是加粗条款，您一经点击已阅读并同意本协议，即视为对本协议的理解和接受，您同意本协议对您具有法律约束力。如果您不同意本协议的任何内容或无法准确理解相关条款，请不要进行后续操作。</div>
                <div class="message">
                    <h3>甲方---放款人：简称“出借人”或“资金方”‘放款人’</h3>
                    <h3>乙方---借款人：简称“您"或“本人”或“借款人”</h3>
                    <h3>丙方--南京百家有米网络科技有限公司：简称“本平台”</h3>
                    <h3>绑定银行卡发卡银行:客户本人用于在本平台借款、还款而绑定银行卡的发卡银行，如中国农业银行等，以下简称“发卡银行”</h3>
                    <h3>借款信息:客户在本平台的借款信息以经客户确认的电子借据所载信息(包括但不限于借款金额、还款日)为准</h3>
                </div>
                <p style="color: #000117;">您通过互联网(Internet) 点击确认或以其他方式选择接受本协议，即表示您同意接受本协议的全部约定内容，确认承担由此产生的一切责任。</p>
                <p style="color: #000117;">一.您保证提供给本平台的银行卡资料(包括:卡号、姓名、证件号码、手机号码等)为您本人持有的真实、完整、合法、有效的银行卡信息，并同意本平台将以上信息送至发卡银行进行核验。</p>
                <p style="color: #000117;">二.您同意在完成发卡银行或其他支付、清算机构等规定的授权动作后，即代表您已充分理解并同意以下授权:</p>
                <p style="color: #000117;">(一)您同意并授权发卡银行或其他支付、清算机构等接受出借人委托本平台签约的支付机构或本平台合作的第三方机构，根据您在本平台的借款信息，向发卡银行发送划款指令，从您提供的绑定银行卡或曾经使用的银行卡、在第三方支付机构开立的支付账户及绑定的银行卡中扣款，用于归还贷款本息;</p>
                <p style="color: #000117;">(二)您同意授权限额不低于您在本平台的全部借款本息;</p>
                <p style="color: #000117;">(三)您承诺前述委托扣款视同您本人作出，不得向发卡银行、第三方支付机构组织及其他代理扣款的支付清算机构提出异议;</p>
                <p style="color: #000117;">(四)您同意并授权本平台在每月应还款日、从贷款逾期之日起每日，根据上述方式进行扣款，用于偿还您在本行的全部债务 直至清偿完毕。</p>
                <p style="color: #000117;">三、授权有效期:本合同初始授权有效期为自本合同生效之日起12个月或自本合同生效之日至授信额度有效期届满;授权有效期届满前，若本合同双方均无异议，可从到期之日起自动顺延一年， 顺延次数不限;如您需要撤销本授权，请联系发卡银行。</p>
                <p style="color: #000117;">四、因您提供他人银行卡资料或虚假信息，或您提供的银行卡账户或在第三方支付机构开立的支付账户余额不足或被挂失、冻结、销户等原因而引起的一切损失和法律责任，由您自行承担，与本平台无关。</p>
                <p style="color: #000117;">五、因发卡银行或其他支付、清算机构等原因导致扣款错误或延迟，给您造成直接损失的，由相关出借人承担，本平台将协助您向其追究相应责任。</p>
                <p style="color: #000117;">客户确认:本人已认真阅读本协议，在完全理解本协议的情况下，同意本协议的全部内容。</p>

            </div> -->
            <!-- <div class="main" v-show="first == 4">
                <h2>个人信息授权书</h2>
                <div class="tip">尊敬的客户：您好！</br>为了保障您的合法权益，您应当阅读并遵守本授权书。请您务必审慎阅读，并充分理解本授权书的全部内容，特别是以加粗形式提示您注意的，以及协议中中所涉及的免除或限制我们责任的条款、排除或限制您权利的条款等。若您不接受本授权书的任何条款，请您立即停止授权。同时，您同意南京百家有米网络科技有限公司以及关联公司（以下简称"我们"）根据国家法律、法规及其他规范性文件的规定变化，对本授权书进行调整、变更。您同意我们通过本平台公告、站内信、电子邮件、手机短信、无线通讯装置等电子方式或邮寄等物理方式通知您变更的内容，通知一经公告即视为您已收到。您在本授权书内容公告变更后继续使用我们服务的，表示您已充分阅读、理解并接受变更后的授权书内容；若您不同意修改后的授权内容的，您应立即停止使用本服务。</div>
                <div class="message">
                    <h3>南京百家有米网络科技有限公司：</h3>
                    <h3>本人：{{loanName}}</h3>
                    <h3>身份证号码：{{loanCard}}</h3>
                </div>
                <p style="color: #000117">拟通过本平台的网络借贷撮合服务进行资金出借，南京百家有米网络科技有限公司及其关联公司（以下简称"贵公司"）需要了解、获取、验证以及向其他任何第三方（以下简称"第三方"）提供本人的个人信息用于完成本人账户下所有资金出借的全流程事务，因此本人特做出以下授权并清楚理解其含义，授权期限为永久。其中第三方包括但不限于：贵公司的合作伙伴、有关机关（包括但不限于公安、公积金、社保、税务、民政、工商等主管部门）、金融机构及类金融机构（包括但不限于银行、中国银联、证券公司、资产管理公司、互联网金融公司、保理公司等）、第三方数据单位(包括但不限于征信机构、第三方数据库 、物流公司、物料供应商、权益产品提供商、通信运营商、电子商务平台、中国互联网金融协会、政府公共信用信息平台等第三方机构)及保险公司等。</p>
                <p style="color: #000117">一、同意并授权贵公司以及贵公司的合作伙伴在提供服务过程中依法采集、保存本人的个人信息（以下简称"个人信息"），并向贵公司合作的存管银行提供上述信息用于存管银行进行数据分析、对本人进行开户及实名认证、产品设计等用途。其中个人信息包括但不限于：</p>
                <p style="color: #000117;">(一)本人的身份信息, 包括但不限于姓名、证件号码、证件类型、住所地、电话号码以及其他身份信息、接受服务时硬件设备中存储的通讯录信息（包括通讯录中已存储的姓名以及电话号码）;</p>
                <p style="color: #000117;">(二)本人在申请、使用贵司以及第三方提供的服务时所提供以及形成的任何数据和信息，包括但不限于平台账户信息、交易信息、接受服务时使用的硬件设备信息等。</p>
                <p style="color: #000117;">二、本人拟通过贵公司的网络借贷撮合服务进行资金出借业务，贵公司需要了解、获取、验证以及向第三方提供本授权书第一条规定的个人信息，用于获取电子签名、电子合同与交易存证等为完成出借而产生的必要流程与行为，则本人特做出以下授权并清楚理解其含义：</p>
                <p style="color: #000117;">（一）若本人拟通过贵公司的网络借贷撮合服务进行资金出借业务，贵公司需要了解、获取、验证以及向第三方提供本授权书第一条（一）至（二）款规定的个人信息，用于实名认证、获取电子签名、电子合同与交易存证等为完成出借而产生的必要流程与行为，则本人特做出以下授权并清楚理解其含义：</p>
                <p style="color: #000117;">1、本人同意授权贵公司以及第三方通过依法成立的征信机构或数据服务机构对本人进行本授权书第一条（一）款身份信息实名认证；通过依法成立的电子签章提供机构、服务机构、电子合同与交易存证机构获取本人的电子签章并进行电子合同与交易的存证；以及进行其他完成出借行为所必经的流程需要获取第三方服务而进行的提供个人信息的行为（包括为出借人提供福利、奖励、积分兑换商品或权益等）；</p>
                <p style="color: #000117;">2、本人同意授权贵公司以及第三方依法采集、保存本人的信息，并向依法成立的征信机构或数据服务机构、电子签章提供机构、服务机构、电子合同与交易存证机构以及其他有必要提供的第三方机构等提供因本业务产生的本人的信息；</p>
                <p style="color: #000117;">3、本人同意授权贵公司以及第三方或其合作方依法采集、保存、整理、加工、使用、提供本人本授权书第一条的个人信息，用作贵公司的出借端的数据分析、风控建模、策略制定、数据建模、投标工具设计、营销获客等与贵公司运营相关的事项；</p>
                <p style="color: #000117;">4、本人同意授权贵公司以及第三方根据行业政策的要求，向中国互联网金融协会互联网金融服务平台、征信机构、企业评级机构提供因本业务产生的本人信息，该等信息需按照各机构的要求予以提供；</p>
                <p style="color: #000117;">5、本人同意授权依法成立的征信机构或数据服务机构、电子签章提供机构、服务机构、电子合同与交易存证机构以及其他有必要提供的第三方机构等进行以下事项：</p>
                <p style="color: #000117;">①同意授权依法成立的征信机构或数据服务机构、电子签章提供机构、服务机构、电子合同与交易存证机构以及其他有必要提供的第三方机构（以下简称"信息收集者"）等向贵公司采集本人的个人信息，且可依法整理、保存、加工、使用、提供；</p>
                <p style="color: #000117;">②同意授权依法成立的征信机构或数据服务机构、电子签章提供机构、服务机构、电子合同与交易存证机构以及其他有必要提供的第三方机构等向信息提供者采集本人身份信息。本人已被明确告知提供上述信息可能会给本人带来的不利后果，包括但不限于：采集的这些信息被信息收集者依法提供给第三方后被他人不当利用的风险，但本人仍然同意授权依法成立的征信机构或数据服务机构、电子签章提供机构、服务机构、电子合同与交易存证机构以及其他有必要提供的第三方机构等采集这些信息。</p>
                <p style="color: #000117;">6、本人同意并授权，自本人签订授权委托书后，贵公司在提供服务的过程中可多次采集、整理、使用、提供本人的信息并可多次依据本授权书而操作执行本授权书项下的活动，无需另行再次取得本人的授权。</p>
                <p style="color: #000117;">（二）若本人拟通过贵公司的网络借贷撮合服务获得小额借款，贵公司需要了解、获取、验证以及向其他第三方提供本人的本授权书第一条（一）至（七）款的个人信息用于核验身份与填报材料信息、获取电子签名、审核借款申请条件、提供个性化借款撮合服务、电子合同与交易存证、贷后管理及风险控制等为完成借款以及履行还款而产生的必要流程与行为等，则本人特做出以下授权并清楚理解其含义：</p>
                <p style="color: #000117;">1、本人同意授权贵公司可多次通过依法成立的征信机构、与贵公司合作的大数据信息核查及反欺诈核查公司、风控服务提供商查询、验证本人的征信信息、个人信息，通过依法成立的电子签章提供机构、服务机构、电子合同与交易存证机构获取本人的电子签章并进行电子合同与交易的存证；以及为提供完整、优质的借款服务而向其他必要的第三方（包括但不限于确认借款意愿或借款信息的客户回访外包服务机构、短信服务机构）提供个人信息的行为；</p>
                <p style="color: #000117;">2、本人同意授权贵公司依法采集、保存本人的个人信息，并向依法成立的征信机构、电子签章提供机构、服务机构、电子合同与交易存证机构、与贵公司合作的大数据信息核查以及反欺诈核查公司、风控服务提供商、第三方催收公司、律师事务所、客户回访外包单位以及其他有必要提供的第三方服务机构等提供因本业务产生的本人的个人信息（包括本人在本业务中产生的不良信息），且无需另行告知本人；</p>
                <p style="color: #000117;">3、本人同意授权贵公司或其合作方依法采集、保存、整理、加工、使用、提供本人的个人信息（含本人在本业务中产生的不良信息），用作贵公司的借款端的数据分析、风控建模、策略制定、产品设计、黑白名单库、智能技术研究、营销获客等与贵公司运营相关的事项；</p>
                <p style="color: #000117;">4、本人同意授权贵公司根据行业政策的要求，向中国互联网金融协会互联网金融服务平台、征信机构、企业评级机构、风险信息共享平台等提供因本业务产生的本人信息，该等信息需按照各机构的要求予以提供；</p>
                <p style="color: #000117;">5、本人同意授权依法成立的征信机构、电子签章提供机构、服务机构、电子合同与交易存证机构、贵公司合作的大数据信息核查以及反欺诈核查公司、风控服务提供商、第三方催收公司、律师事务所、客户回访外包单位以及其他有必要提供的第三方服务机构等进行以下事项：</p>
                <p style="color: #000117;">①同意授权上述机构/公司向有关机构采集、查询、核验本人的个人信息，且可依法整理、保存、加工、使用、提供，并出具个人信用报告、验证结果等；</p>
                <p style="color: #000117;">②同意授权拥有本人不良信息的贵公司及第三方可以向依法成立的征信机构、其合作的大数据信息核查及反欺诈核查公司、风控服务提供商提供本人的不良信息，且不再另行告知本人。</p>
                <p style="color: #000117;">6、本人知悉本授权书中第一条（八）款中的个人信息收集用于本人实际收到借款且逾期时贵公司的贷款中后期管理使用。本人同意并授权，自本人签订授权委托书后，贵公司在提供服务的过程中可多次采集、整理、使用、提供本人的信息并可多次依据本授权书而操作执行本授权书项下的活动，无需另行再次取得本人的授权。</p>
                <h3>特别声明：</h3>
                <p style="color: #000117">本授权书自授权书签订之日起即时生效，且效力具有独立性，不因相关业务合同或条款无效或被撤销而无效或失效，本授权一经做出，便不可撤销。</p>
                <p style="color: #000117;">本人已知悉本授权书全部内容（特别是加粗字体内容）的含义及因此产生的法律效力，自愿作出以上授权。本授权书是本人真实意思表示，本人同意承担由此带来的一切法律后果。</p>

            </div> -->
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
            loanUrl: xmy.getQueryString("loanUrl"),
            loanuserId: xmy.getQueryString("userId"),
            loanName: '',
            loanCard: '',
            first: 1
        }
    },

    methods: {
        // 获取个人信息
        getMessage(){
            var _this = this;
            $.get("/user/info/get?userId="+loanuserId,function(r){
                if(r.respCode=="000000"){
                    _this.loanName = r.data.username;
                    _this.loanCard = r.data.idCard;
                }
            });
        }
    },

    mounted () {
        var _this = this;
    }
}
</script>



<style lang="less" scoped>
    .list{
        height: 0.5rem;
        padding: 0 0.2rem;
        border-bottom:1px solid #f5f5f5;
        a{
            display: block;
            height: 0.5rem;
            line-height: 0.5rem;
            font-family: 'PingFangSC-Light';
            font-size: 0.14rem;
            color: #000117;
            text-decoration: none;
            img{
                display: block;
                height: 0.1rem;
                float: right;
                margin-top: 0.2rem;
            }
        }
    }
    .loan{
        img{
            display: block;
            width: 100%;
        }
    }
    .main{
        // background: #f5f5f5;
        padding: 0 0.25rem 0.15rem;
        h2{
            height: 0.6rem;
            line-height: 0.6rem;
            text-align: center;
            font-family: 'PingFangSC-Light';
            font-size: 0.18rem;
            color: #000117;
            letter-spacing: 0;
            border-bottom: 1px solid #c8c8c8;
            margin-bottom: 0.13rem;
        }
        h4 {
            height: 0.35rem;
            line-height: 0.35rem;
            text-align: center;
            font-family: 'PingFangSC-Light';
            font-size: 0.14rem;
            color: #000117;
            letter-spacing: 0;
            margin-bottom: 0.05;
        }
        .tip{
            font-family: 'PingFangSC-Light';
            font-size: 0.12rem;
            color: #9B9B9B;
            letter-spacing: 0;
            line-height: 0.25rem;
            margin-bottom: 0.15rem;
        }
        .message{
            margin-bottom: 0.15rem;
            font-family: 'PingFangSC-Light';
            font-size: 0.12rem;
            letter-spacing: 0;
            line-height: 0.25rem;
        }
        h3{
            font-family: 'PingFangSC-Light';
            font-size: 0.14rem;
            letter-spacing: 0;
            line-height: 0.25rem;
            font-weight: normal;
            color: #000117;
            margin-top: 0.05rem;
        }
        span{
            display: block;
            color: #9B9B9B;
        }
        p{
            font-family: 'PingFangSC-Light';
            font-size: 0.12rem;
            letter-spacing: 0;
            line-height: 0.25rem;
            font-weight: normal;
            color: #9B9B9B;
            text-indent: 1em;
            word-break:break-all;
        }
        table{
            width: 100%;
            margin: 0.05rem 0;
            td{
                height: 0.25rem;
                font-family: 'PingFangSC-Light';
                font-size: 0.12rem;
                letter-spacing: 0;
                line-height: 0.25rem;
                font-weight: normal;
                color: #9B9B9B;
                border: 1px solid #cccccc;
                em{
                    display: inline-block;
                    width: 40%;
                    margin-left: 0.05rem;
                }
                span{
                    margin-left: 0.05rem;
                }
            }
        }
    }

</style>

