<template>
    <div>
        <el-form :model="ruleForm" ref="ruleForm" label-width="100px" class="demo-ruleForm">
            <el-collapse  v-model="activeNames">
                <el-collapse-item style="padding:0 20px;background: #fff;" name="1">
                    <template slot="title">
                        <div class="moduleTit">
                            <h3>
                                <span class="infoTit">扩展字段信息</span>
                            </h3>
                        </div>
                    </template>
                    <div class="moduleWhite" style="margin-top:20px;">
                        <el-row>
                            <el-col :span="8">
                                <el-form-item label="扩展对象" prop="cfObjCode">
                                    <el-select v-model="ruleForm.cfObjCode" style="width:100%;" size="small" disabled>
                                        <el-option
                                                v-for="item in objects"
                                                :key="item.value"
                                                :label="item.label"
                                                :value="item.value">
                                        </el-option>
                                    </el-select>
                                </el-form-item>
                            </el-col>
                            <el-col :span="8">
                                <el-form-item label="数据库字段" prop="cfCfieldColumn">
                                    <el-select v-model="ruleForm.cfCfieldColumn" style="width:100%;" size="small" disabled>
                                        <el-option
                                                v-for="item in columns"
                                                :key="item.value"
                                                :label="item.label"
                                                :value="item.value">
                                        </el-option>
                                    </el-select>
                                </el-form-item>
                            </el-col>
                            <el-col :span="8">
                                <el-form-item label="排序号" prop="sort">
                                    <el-input-number  v-model="ruleForm.sort"  controls-position="right" size="small" :min="1" style="width: 100%;" disabled></el-input-number>
                                </el-form-item>
                            </el-col>
                        </el-row>
                        <el-row>
                            <el-col :span="8">
                                <el-form-item label="字段显示类型" prop="cfCfieldType">
                                    <el-select size="small" v-model="ruleForm.cfCfieldType"   disabled style="width:100%;">
                                        <el-option label="内部" value="1"></el-option>
                                        <el-option label="外部" value="2"></el-option>
                                    </el-select>
                                </el-form-item>
                            </el-col>
                            <el-col :span="8">
                                <el-form-item label="字段显示名" prop="cfCfieldLabel">
                                    <el-input v-model="ruleForm.cfCfieldLabel" size="small" disabled></el-input>
                                </el-form-item>
                            </el-col>
                        </el-row>
                    </div>
                </el-collapse-item>
                <hr/>
            </el-collapse>
            <div style="text-align:center;position: fixed;width:calc(100% - 235px);;bottom:20px;">
                <el-button class="el-icon-back" size="medium" round @click="goBack">返回</el-button>
            </div>

        </el-form>


    </div>
</template>
<script>
    export default {
        data () {
            return {
                ruleForm: {
                    id: '',
                    cfCfieldLabel: '',
                    cfCfieldColumn: '',
                    cfObjCode: '',
                    cfCfieldType: '',
                    cfCfieldName: '',
                    sort: ''
                },
                tableData: [],
                columns: [],
                objects: [],
                activeNames: ['1']
            };
        },
        mounted () {
        },
        created () {
            this.ruleForm.id = this.$route.params.id;
            this.backfilleditData();
            this.findObject();
            this.getColumns();
        },
        methods: {
            backfilleditData () {
                var getDataId = this.ruleForm.id;
                if (getDataId) {
                    this.$myHttp({
                        method: 'post',
                        url: this.permissionPrefix + '/microarch/sys/cfields/view',
                        data: {
                            'entity': {
                                'id': getDataId
                            }
                        }
                    }).then(res => {
                        var backData = res.data.entity;
                        this.ruleForm.cfCfieldLabel = backData.cfCfieldLabel;
                        this.ruleForm.cfCfieldColumn = backData.cfCfieldColumn;
                        this.ruleForm.cfObjCode = backData.cfObjCode;
                        this.ruleForm.cfCfieldType = backData.cfCfieldType;
                        this.ruleForm.cfCfieldName = backData.cfCfieldName;
                        this.ruleForm.sort = backData.sort;
                    });
                }
            },

            goBack () {
                this.$router.push({
                    name: 'sys/cfields/cfieldsList'
                });
            },
            handleChange (val) {
                console.log('val---', val);
            },
            getColumns () {
                var _this = this;
                var cfObjCode = _this.ruleForm.cfObjCode;
                if (cfObjCode) {
                    this.$myHttp({
                        method: 'get',
                        url: _this.prefix + '/microarch/sys/cfields/columns/' + cfObjCode + window.suffix,
                        data: {}
                    }).then(res => {
                        // 成功回调
                        var resArry = [];
                        for (var d = 0, len = res.data.length; d < len; d++) {
                            resArry.push({
                                value: res.data[d],
                                lable: res.data[d]
                            });
                        }
                        _this.columns = resArry;
                    });
                }
            },
            findObject () {
                var that = this;
                this.$myHttp({
                    method: 'post',
                    url: this.permissionPrefix + '/microarch/sys/sysDictHead/findByDcode',
                    data: {
                        'entity': {
                            'dtype': 'ExtendedTable'
                        }
                    }
                }).then(resp => {
                    var resArry = [];
                    for (var d = 0, len = resp.data.entity.sysDicts.length; d < len; d++) {
                        resArry.push({
                            value: resp.data.entity.sysDicts[d].dkey,
                            label: resp.data.entity.sysDicts[d].dvalue
                        });
                    }
                    that.objects = resArry;
                    console.log(resArry);
                });
            }

        }
    };
</script>
<style scoped>
    hr{
        height:0px;
        border:0px solid #ccc;
        margin:10px 0;
    }
    .roleUl{
        width:500px;
        height:auto;
        border:1px solid #ccc;
        border-bottom: 0;
        margin-top:5px;
        min-width:370px;
        margin-bottom:10px;
        overflow:hidden;
    }
    .roleUl li{
        border-bottom:1px solid #ccc;
        font-size:14px;
        text-align:center;
        min-width:370px;
        height:44px;
    }
    .roleUl li span{
        display:inline-block;
        padding:5px 0 7px 0;
        height:100%;
        box-sizing: border-box;
        float:left;
    }
    .roleUl li span.checkselect{
        width:50px;
        padding-left:10px;
    }
    .roleUl li span.selectName{
        width:200px;
        line-height: 33px;
        border-right:1px solid #ccc;
    }
    .roleCodeSpan{
        width:200px;
        line-height: 33px;
        border-right:1px solid #ccc;
        margin-left: 15px;
        top: -5px;
    }
    span.btnAction{
        margin-left:24px;
        top:-3px;
        position: relative;
    }
    .tree{
        position:absolute;
        top:0
    }
    .transFace{
        position:absolute;
        top:0;
        left:0;
        width:100%;
        height:100%;
        z-index:2;
        cursor:pointer;
    }
    /**优化界面***/
    .moduleWhite2{
        background: #fff;
        /*padding:10px;*/
    }
    .moduleTit h3{
        border-bottom: 1px solid #ccc;
        font-weight:normal;
        font-size:16px;
        color:#333;
        line-height: 25px;
        margin-bottom: 20px;
        padding:10px 0;
    }
    .moduleTit h3 span.infoTit{
        font-size:14px;
        padding-left:15px;
        border-left: 5px solid #2d8cf0;
    }
    .moduleTit h3 span.infoIcon{
        float:right;
        font-size:20px;
        padding-right: 20px;
        cursor: pointer;
    }
    .sliceLine{
        border-top:1px dashed #ddd;
        margin-bottom:22px;
    }

</style>