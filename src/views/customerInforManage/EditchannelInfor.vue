<template>

    <el-col :span="24" style="background-color: white">

        <div style="margin-left: 20px">

            <el-form :label-position="labelPosition" ref="ruleFormOne" :model="form" label-width="120px" :rules="rules"
                     style="margin-top: 50px;margin-bottom: 20px">

                <el-row>
                    <el-col :span="8" :xs="20">
                        <el-form-item label="渠道名称" prop="channelName">
                            <el-input v-model="form.channelName" style="width: 95%" placeholder="请输入" size="small" @blur="repeatJudgeInit"></el-input>
                        </el-form-item>

                    </el-col>

                    <el-col :span="8" :xs="20">
                        <el-form-item label="行业" prop="channelIndustry">
                            <el-select v-model="form.channelIndustry" placeholder="请选择" clearable filterable size="small"
                                       style="width: 95%">
                                <el-option
                                        v-for="item in channelIndustryData"
                                        :key="item.value"
                                        :label="item.label"
                                        :value="item.value">
                                </el-option>
                            </el-select>
                        </el-form-item>
                    </el-col>
                    <el-col :span="8" :xs="20">
                        <el-form-item label="类型" prop="channelType">
                            <el-select v-model="form.channelType" placeholder="请选择" clearable filterable size="small"
                                       style="width: 95%">
                                <el-option
                                        v-for="item in channelTypeData"
                                        :key="item.value"
                                        :label="item.label"
                                        :value="item.value">
                                </el-option>
                            </el-select>
                        </el-form-item>
                    </el-col>
                </el-row>

                <el-row>
                    <el-col :span="8" :xs="20">
                        <el-form-item label="销售负责人" prop="salesLeader">
                            <el-select v-model="form.salesLeader" placeholder="请选择" clearable filterable size="small"
                                       style="width: 95%">
                                <el-option
                                        v-for="item in salesLeaderData"
                                        :key="item.value"
                                        :label="item.label"
                                        :value="item.value">
                                </el-option>
                            </el-select>
                        </el-form-item>
                    </el-col>
                    <el-col :span="8" :xs="20">
                        <el-form-item label="部门" prop="salesDepartment">
                            <el-select v-model="form.salesDepartment" placeholder="请选择" clearable filterable size="small"
                                       style="width: 95%">
                                <el-option
                                        v-for="item in salesDepartmentData"
                                        :key="item.value"
                                        :label="item.label"
                                        :value="item.value">
                                </el-option>
                            </el-select>
                        </el-form-item>
                    </el-col>
                    <el-col :span="8" :xs="20">
                        <el-form-item label="状态" prop="status">
                            <el-radio-group v-model="form.status">
                                <el-radio :label="true">激活</el-radio>
                                <el-radio :label="false">停用</el-radio>
                            </el-radio-group>
                        </el-form-item>
                    </el-col>

                </el-row>

                <el-row>
                    <el-col :span="8" :xs="20">
                        <el-form-item label="渠道主要联系人" prop="primaryContact">
                            <el-input v-model="form.primaryContact" placeholder="请输入" style="width: 95%" size="small"></el-input>
                        </el-form-item>

                    </el-col>


                    <el-col :span="8" :xs="20">
                        <el-form-item label="电话" prop="telephone">
                            <el-input v-model="form.telephone" style="width: 95%" size="small" placeholder="请输入"  @blur="checkPhone(form.telephone)"></el-input>
                        </el-form-item>
                    </el-col>

                    <el-col :span="8" :xs="20">
                        <el-form-item label="传真">
                            <el-input v-model="form.fax" placeholder="请输入" style="width: 95%" size="small"></el-input>
                        </el-form-item>
                    </el-col>
                </el-row>
                <el-row>
                    <el-col :span="8" :xs="20">
                        <el-form-item label="网站">
                            <el-input v-model="form.site" placeholder="请输入" style="width: 95%" size="small"></el-input>
                        </el-form-item>
                    </el-col>

                    <el-col :span="8" :xs="20">
                        <el-form-item label="电子邮件" prop="email">
                            <el-input v-model="form.email" placeholder="请输入" style="width: 95%" size="small"  @blur="chenckEmail(form.email)"></el-input>
                        </el-form-item>
                    </el-col>
                    <el-col :span="8" :xs="20">
                        <el-form-item label="备注" >
                            <el-input
                                    type="textarea"
                                    style="width: 95%"
                                    :rows="2"
                                    placeholder="请输入内容"
                                    v-model="form.memo">
                            </el-input>
                        </el-form-item>
                    </el-col>
                </el-row>
            </el-form>
            <el-col :span="24" :offset="10">

                <el-button @click="saveProjectsInfo()" type="primary" style="margin-top: 20px;margin-bottom: 10px">提交
                </el-button>
                <el-button @click="close()" style="margin-top: 20px;margin-bottom: 10px">返回</el-button>

            </el-col>
        </div>

    </el-col>
</template>

<script>

    export default {
        components: {},
        data() {
            return {
                currentPage:'',
                labelProjectType: '商机',
                labelPosition: 'right',
                oppoNum: '创建商机后自动生成',
                industryData:[],
                projectStageData:[],
                winRateData:[],
                orgCodeData: [],
                customerIdData: [],
                pdIdData: [],
                saleIdData: [],
                flag:'',
                projectDirectorData:[],
                pmoIdData:[],
                preMangerIdData:[],
                salesLeaderData:[],
                channelIndustryData:[],
                repeatJudge:'',
                channelNameArray:[],
                currentChannelName:'',
                channelTypeData:[
                    /*{
                        label:'Distributor',
                        value:'0'
                    },
                    {
                        label:'ISV',
                        value:'1'
                    },
                    {
                        label:'Reseller',
                        value:'2'
                    }*/
                ],
                salesDepartmentData:[],
                channelIdData:[],
                form: {
                    channelName:'',
                    channelIndustry:'',
                    channelType:'',
                    salesLeader:'',
                    salesDepartment:"",
                    status:true,
                    primaryContact:'',
                    telephone:'',
                    fax:'',
                    site:'',
                    email:'',
                    memo:''
                },
                isShow: false,
                rules: {
                    channelName: [
                        {required: true, message: '请输入渠道名称', trigger: 'change'}
                    ],
                    channelIndustry: [
                        {required: true, message: '请选择渠道行业', trigger: 'blur'}
                    ],
                    channelType: [
                        {required: true, message: '请输入渠道类型', trigger: 'change'}
                    ],
                    salesLeader: [
                        {required: true, message: '请选择销售负责人', trigger: 'blur'}
                    ],
                    salesDepartment: [
                        {required: true, message: '请选择销售负责部门', trigger: 'blur'}
                    ],
                    status: [
                        {required: true, message: '请选择状态', trigger: 'blur'}
                    ],
                    primaryContact: [
                        {required: true, message: '请输入主要联系人', trigger: 'change'}
                    ],
                    telephone: [
                        {required: true, message: '请输入主要联系人电话', trigger: 'change'}
                    ],
                    email: [
                        {required: true, message: '请输入主要联系人电子邮箱', trigger: 'blur'}
                    ]
                }
            };
        },
        methods: {
            /**
             * 校验联系方式
             */
            checkPhone:function(event){
                var tel = /^0\d{2,3}-?\d{7,8}$/;
                var phone = /^(((13[0-9]{1})|(15[0-9]{1})|(18[0-9]{1})|(17[0-9]{1})|(16[0-9]{1})|(19[0-9]{1}))+\d{8})$/;
                if(!phone.test(event) && !tel.test(event) &&event != ""){

                    this.$notify({
                        title: '错误',
                        message: '联系方式格式不正确',
                        type: 'error'
                    });
                    this.phoneFlag = false;
                }else{
                    this.phoneFlag = true;
                }
            },

            /**
             *校验邮箱
             */
            chenckEmail:function(event){
                var mailReg = /^(\w-*\.*)+@(\w-?)+(\.\w{2,})+$/;
                if(!mailReg.test(event) && event != ""){
                    this.$notify({
                        title: '错误',
                        message: '邮箱格式不正确',
                        type: 'error'
                    });
                    this.emailFlag = false;
                }else{
                    this.emailFlag = true;
                }
            },
            repeatJudgeInit(){
                var _this = this;
                var channelNameArray=[];
                this.$myHttp({
                    method: 'post',
                    url: this.prefix + 'masterdata/channel/list' + window.suffix,
                    /*url:"http://192.168.99.24:6004/customer/list",*/
                    data: {
                        "conditions":[],
                        "pageRequest":{
                            'pageNum':0,
                            'pageSize':1000
                        },
                        "sortBy":{
                            /*'direction':this.direction,
                            'field':this.field*/
                        }
                    }
                }).then(res => {
                    for(var i=0;i<res.data.content[0].totalElements;i++){
                        if(res.data.content[0].content[i].channelName){
                            channelNameArray.push(res.data.content[0].content[i].channelName);
                        }
                    }
                    _this.channelNameArray=channelNameArray;
                    console.log(_this.channelNameArray);
                })

            },


            saveProjectsInfo: function () {
                var _this = this;
                this.$refs['ruleFormOne'].validate((valid) => {
                    if (valid) {
                        this.ruleFormOne = true;
                    }

                   /* this.$refs['ruleFormTwo'].validate((valid) => {
                        if (valid) {
                            this.ruleFormTwo = true
                        }
                    });
                    this.$refs['ruleFormThree'].validate((valid) => {
                        if (valid) {
                            this.ruleFormThree = true
                        }
                    });
                    this.$refs['ruleFormFour'].validate((valid) => {
                        if (valid) {
                            this.ruleFormFour = true
                        }
                    });*/
                    if(_this.form.status==true){
                        _this.form.status="Y"
                    }else if(_this.form.status==false){
                        _this.form.status="N"
                    }
                    console.log("所属行业：",_this.form.customerIndustry);
                    console.log("所属业务部门：",_this.form.salesDepartment);
                    console.log("销售负责人：",_this.form.salesLeader);
                    console.log("状态：",_this.form.status);

                    if (this.ruleFormOne) {
                        if(_this.form.channelName==_this.currentChannelName){
                            /* console.log("提交的数据",_this.form.customerName)
                             console.log('对比的数据',_this.customerNameArray)*/
                            _this.flag="true";
                        }else if(_this.form.channelName!=_this.currentChannelName){
                            _this.flag="true";
                            for(var y=0;y<_this.channelNameArray.length;y++){
                                if(_this.form.channelName==_this.channelNameArray[y]){
                                    _this.flag="false";
                                }
                            }
                        }
                        if(_this.flag=="true"){
                            this.$myHttp({
                                method: 'POST',
                                url: this.prefix + 'masterdata/channel/save'+ window.suffix,
                                /*url:'http://192.168.99.24:6004/channel/save',*/
                                data:{
                                    "entity":{
                                        'id': _this.$route.params.id,
                                        "channelName":_this.form.channelName,
                                        "channelType":_this.form.channelType,
                                        "primaryContact":_this.form.primaryContact,
                                        "channelIndustry":_this.form.channelIndustry,
                                        "telephone":_this.form.telephone,
                                        "email":_this.form.email,
                                        "fax":_this.form.fax,
                                        "site": _this.form.site,
                                        "salesLeader": _this.form.salesLeader,
                                        "salesDepartment": _this.form.salesDepartment,
                                        "status": _this.form.status,
                                        "memo": _this.form.memo,
                                    }
                                }
                            }).then(res => {
                                //成功回调方法
                                _this.$notify({
                                    title: '成功',
                                    message: '保存成功',
                                    type: 'success'
                                });
                                _this.$router.push({
                                    name: 'customerInforManage/channelInfor',
                                    params:{
                                        currentPage:_this.currentPage
                                    }
                                });
                            })
                        }else {
                            this.$notify({
                                title: '警告',
                                message: '所填名称与其他名称重复，请修改！',
                                type: 'warning'
                            });
                        }
                    }
                });
            },
            close: function () {
                var _this = this;
                this.$router.push({
                    name: 'customerInforManage/channelInfor',
                    params:{
                        currentPage:_this.currentPage
                    }
                });
            },
            makeDataDictionary:function () {
                //行业
                var _this=this;
                this.$myHttp({
                    method: 'POST',
                    url: this.prefix + 'sysConfig/getSysDictionaryList'+ window.suffix,
                    data: {
                        "entity": {
                            "dtype": "行业"
                        },
                        "orderBy": "idx asc"
                    }
                }).then(res => {
                    //成功回调方法
                    var resArry = [];
                    for(var items in res.data.rows){
                        resArry.push({
                            label:res.data.rows[items].dvalue,
                            value:res.data.rows[items].dkey
                        })
                    }
                    _this.channelIndustryData = resArry;
                });
                //渠道类型
                this.$myHttp({
                    method: 'POST',
                    url: this.prefix + 'sysConfig/getSysDictionaryList'+ window.suffix,
                    data: {
                        "entity": {
                            "dtype": "渠道类型"
                        },
                        "orderBy": "idx asc"
                    }
                }).then(res => {
                    //成功回调方法
                    var resArry = [];
                    for(var items in res.data.rows){
                        resArry.push({
                            label:res.data.rows[items].dvalue,
                            value:res.data.rows[items].dkey
                        })
                    }
                    _this.channelTypeData = resArry;
                });
                //所属业务部门
                this.$myHttp({
                    method: 'POST',
                    url: this.prefix + 'masterdata/customer/getSaleOrgs'+ window.suffix,
                    /*url:'http://192.168.99.24:6004/customer/getSaleOrgs',*/
                    data: {
                        "entity": {
                            "dtype": "所属业务部门"
                        },
                        "orderBy": "idx asc"
                    }
                }).then(res => {
                    //成功回调方法
                    var resArry = [];
                    for(var items in res.data.content){
                        resArry.push({
                            label:res.data.content[items].orgName,
                            value:res.data.content[items].id
                        })
                    }
                    _this.salesDepartmentData = resArry;
                });
                //销售负责人
                this.$myHttp({
                    method: 'POST',
                    url: this.prefix + 'masterdata/customer/getSalePersons'+ window.suffix,
                   /* url:'http://192.168.99.24:6004/customer/getSalePersons',*/
                    data: {
                        "entity": {
                            "dtype": "销售负责人"
                        },
                        "orderBy": "idx asc"
                    }
                }).then(res => {
                    //成功回调方法
                    var resArry = [];
                    for(var items in res.data.content){
                        resArry.push({
                            label:res.data.content[items].userName,
                            value:res.data.content[items].id
                        })
                    }
                    _this.salesLeaderData = resArry;
                    console.log(_this.salesLeaderData)
                });
                //渠道类型
               /* this.$myHttp({
                    method: 'POST',
                    /!*url: this.prefix + 'sysConfig/getSysDictionaryList'+ window.suffix,*!/
                    url:this.prefix + 'sysConfig/getSysDictionaryList'+ window.suffix,
                    data: {
                        "entity": {
                            "dtype": "渠道类型"
                        },
                        "orderBy": "idx asc"
                    }
                }).then(res => {
                    //成功回调方法
                    console.log('渠道类型',res);
                    var resArry = [];
                    for(var items in res.data.content){
                        resArry.push({
                            label:res.data.content[items].userName,
                            value:res.data.content[items].id
                        })
                    }
                    _this.channelTypeData = resArry;
                });*/
            }
        },
        created() {
            //跨router传参方式
            var _this = this;
            this.currentPage = this.$route.params.currentPage;
            //生成当前页面需要的数据字典内容
            this.makeDataDictionary();
            /* console.log('litan',this.$route.params);*/
            if (this.$route.params.id) {
                this.isShow = true;
                this.$myHttp({
                    method: 'POST',
                    url: this.prefix + 'masterdata/channel/view'+ window.suffix,
                    /*url: "http://192.168.99.24:6004/channel/view",*/
                    data: {
                        "entity": _this.$route.params.id
                    }
                }).then(res => {
                    //成功回调方法
                    console.log(res.data.content);
                    _this.form= res.data.content;
                    _this.currentChannelName=res.data.content.channelName;
                    _this.salesDepartmentData.forEach((subitem,index) => {
                        /* console.log(subitem.value);*/
                        if(_this.form.salesDepartment){
                            if(_this.form.salesDepartment == subitem.value){
                                _this.form.salesDepartment = subitem.value;
                            }
                        }
                    });
                    _this.salesLeaderData.forEach((subitem,index) => {
                        /*  console.log(subitem.value);*/
                        if(_this.form.salesLeader){
                            if(_this.form.salesLeader == subitem.value){
                                _this.form.salesLeader = subitem.value;
                            }
                        }
                    });
                    if(res.data.content.status=="Y"||res.data.content.status=="true"){
                        _this.form.status=true
                    }else if(res.data.content.status=="N"||res.data.content.status=="false"){
                        _this.form.status=false
                    }
                })
            }

            //马总需求
            /* this.form.salesLeader = '进行中';*/

        },
        mounted(){
            /*this.initOrg()*/
        },
        computed: {
            contractAmount: {
                set: function (val) {

                    this.form.contractAmount = val * 1000
                },
                get: function (val) {

                    return this.form.contractAmount / 1000
                }
            }
        }
    };
</script>

<style>

</style>
