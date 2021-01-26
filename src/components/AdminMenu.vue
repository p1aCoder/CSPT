<template>
    <el-menu
            @select="handleSelect"
            default-active="daa"
            class="el-menu-admin"
            mode="vertical"
            background-color="#545c64"
            text-color="#fff"
            active-text-color="#ffd04b"
            :collapse="isCollapse">
        <div style="height: 80px;"></div>
        <!--index 没有用但是必需字段-->
        <el-submenu  v-for="(item,i) in adminMenus" :key="i" :index="(i).toString()" style="text-align: left">
        <span slot="title" style="font-size: 17px;">
          {{item.name}}
        </span>
            <el-menu-item v-for="child in item.children" :key="child" :index="child" >
                {{ child}}
            </el-menu-item>
        </el-submenu>
    </el-menu>
</template>

<script>
    export default {
        name: 'AdminMenu',
        data() {
            return {
                isCollapse: false,
                cid: '',
                adminMenus: [{name: "肝胆胰用药", children: ["肝炎" ,"肝硬化", "脂肪肝", "肝癌" ,"黄疸", "胆结石", "胆囊炎" ,"胰腺癌" ,"胰腺炎" ,"转氨酶增高" ,"更多肝胆胰用药"]},
                    {name: "胃肠药", children: ["腹痛和腹泻", "消化不良", "食欲不振","胃炎","肠胃溃疡","便秘","便血","呕血","恶心呕吐","返酸打嗝","结肠癌","食道癌","胃癌","痔疮","更多胃肠药"]},
                    {name: "呼吸系统用药", children: ["上呼吸道感染","咳嗽咳痰","咽痛音哑","肺结核","肺癌","咯血","哮喘","更多呼吸系统用药"]},
                    {name: "解热镇痛药", children: ["感冒药","祛暑药","发热头痛","癌症疼痛","创伤性疼痛","风湿痛","肌肉关节痛","痛经","牙痛","抗痛风药","更多解热镇痛药"]},
                    {name: "神经系统用药", children: [
                            "神经衰弱","失眠多梦","偏头痛","神经官能症","眩晕、耳鸣","抑郁症","晕车和晕船症","痴呆","癫痫","神经炎","肌无力","精神分裂症","面瘫","脑动脉硬化","帕金森","更多"]},
                    {name: "心脑血管用药", children: ["高血压","高血脂", '动脉硬化', '心绞痛', "心律失常","胸闷胸痛","心慌心悸","中风偏瘫","冠心病","心功能不全","心肌病","心力衰竭","更多"]},
                    {name: "血液疾病用药", children: ["白血病","贫血","止血药","溶血栓药","调血脂和抗动脉粥样硬化药","更多血液疾病药"]},
                    {name: "泌尿系统用药", children: ["尿路结石","尿频、尿急、尿痛","肾癌","肾病综合征","肾炎","水肿","利尿药","尿毒症","尿崩症","遗尿症","更多泌尿系统药"]},
                    {name: "免疫功能调节", children: ["干燥综合症","红斑狼疮","类风湿","免疫抑制药","免疫增强药","更多免疫调节药"]},
                    {name: "内分泌及代谢", children: ["糖尿病","肥胖症","痛风","激素药","甲亢","胰岛素","口服降糖药","性激素","更多内分泌及代谢"]},
                    {name: "维生素营养药", children: ["维生素","复合维生素","叶酸类","补钙、补锌","补铁、补硒","微量元素","营养药","肠内营养药","肠外营养药","矿物质类药"]},
                    {name: "妇产科", children: ["子宫收缩药","子宫舒张药","促子宫成熟药","阴道炎","激素避孕药","外用避孕药","终止妊娠药"]},
                    {name: "五官科用药", children: ["眼科用药","青光眼","口腔科用药","牙痛","耳部用药","鼻部用药","咽喉用药"]},
                    {name: "抗肿瘤药", children: ["抗代谢","烷化剂","抗生素类","抗肿瘤辅助药","天然抗肿瘤药","植物成分药","激素类"]},
                    {name: "其它药品", children: ["皮肤用药","抗过敏药","水电解质及酸碱平衡药","疫苗","抗血清和免疫球蛋白","解毒药"]},
                    ]
            }
        },
        methods: {
            handleSelect(key, keyPath) {
                this.cid = key
                this.$emit('indexSelect')
            }

        }
    }
</script>

<style scoped>
    .el-menu-admin {
        border-radius: 5px;
        height: 100%;
    }
</style>
