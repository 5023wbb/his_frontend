<template>
  <div>
    <el-row :gutter="30">
      <el-col :span="6"><div class="grid-content" style="background: #acfed0;"><el-link icon="el-icon-s-order" @click="temporarysto">暂存</el-link></div></el-col>
      <el-col :span="6"><div class="grid-content " style="background: #bafed5;"><el-link icon="el-icon-success" @click="submit">提交</el-link></div></el-col>
      <el-col :span="6"><div class="grid-content " style="background: #aafecc;"><el-link icon="el-icon-delete" @click="refreshMsg">清空所有</el-link></div></el-col>
      <el-col :span="6"><div class="grid-content " style="background: #a5fec2;"><el-link icon="el-icon-circle-plus" @click="refreshMsg" >刷新</el-link></div></el-col>
    </el-row>
    <el-row :gutter="20" style="margin-top: 5px;width: 30%;">
      <el-col :span="10"><div class="grid-content" style="background:#a6b6fe;">病史信息</div></el-col>
    </el-row>

    <el-container>
      <el-container>
        <el-main style="padding: 4px;">
          <el-row :gutter="5">
            <el-col :span="4"><div class="msg " style="text-align: left;">主诉</div></el-col>
            <el-col :span="20"><div class="msg ">
              <el-input :disabled="cansee3"
                type="textarea"
                :autosize="{ minRows: 2, maxRows: 3}"
                placeholder="请输入内容"
                v-model="medicalmainpage.chiefcomplaint">
              </el-input>
            </div></el-col>
          </el-row>

          <el-row :gutter="5">
            <el-col :span="4"><div class="msg " style="text-align: left;">现病史</div></el-col>
            <el-col :span="20"><div class="msg ">
              <el-input :disabled="cansee3"
                type="textarea"
                :autosize="{ minRows: 2, maxRows: 3}"
                placeholder="请输入内容"
                v-model="medicalmainpage.histofpresill">
              </el-input>
            </div></el-col>
          </el-row>

          <el-row :gutter="5">
            <el-col :span="4"><div class="msg " style="text-align: left;">现病治疗情况</div></el-col>
            <el-col :span="20"><div class="msg ">
              <el-input :disabled="cansee3"
                type="textarea"
                :autosize="{ minRows: 2, maxRows: 3}"
                placeholder="请输入内容"
                v-model="medicalmainpage.treatofpresill">
              </el-input>
            </div></el-col>
          </el-row>

          <el-row :gutter="5">
            <el-col :span="4"><div class="msg " style="text-align: left;">检查建议</div></el-col>
            <el-col :span="20"><div class="msg ">
              <el-input :disabled="cansee3"
                type="textarea"
                :autosize="{ minRows: 2, maxRows: 3}"
                placeholder="请输入内容"
                v-model="medicalmainpage.examsuggestion">
              </el-input>
            </div></el-col>
          </el-row>

        </el-main>
      </el-container>
      <el-container>
        <el-main style="padding: 4px;">
          <el-row :gutter="5">
            <el-col :span="4"><div class="msg " style="text-align: left;">既往史</div></el-col>
            <el-col :span="20"><div class="msg ">
              <el-input :disabled="cansee3"
                type="textarea"
                :autosize="{ minRows: 2, maxRows: 3}"
                placeholder="请输入内容"
                v-model="medicalmainpage.pasthist">
              </el-input>
            </div></el-col>
          </el-row>

          <el-row :gutter="5">
            <el-col :span="4"><div class="msg " style="text-align: left;">过敏史</div></el-col>
            <el-col :span="20"><div class="msg ">
              <el-input :disabled="cansee3"
                type="textarea"
                :autosize="{ minRows: 2, maxRows: 3}"
                placeholder="请输入内容"
                v-model="medicalmainpage.allergichist">
              </el-input>
            </div></el-col>
          </el-row>

          <el-row :gutter="5">
            <el-col :span="4"><div class="msg " style="text-align: left;">体格检查</div></el-col>
            <el-col :span="20"><div class="msg ">
              <el-input :disabled="cansee3"
                type="textarea"
                :autosize="{ minRows: 2, maxRows: 3}"
                placeholder="请输入内容"
                v-model="medicalmainpage.physicalexamination">
              </el-input>
            </div></el-col>
          </el-row>

          <el-row :gutter="5">
            <el-col :span="4"><div class="msg " style="text-align: left;">注意事项</div></el-col>
            <el-col :span="20"><div class="msg ">
              <el-input :disabled="cansee3"
                type="textarea"
                :autosize="{ minRows: 2, maxRows: 3}"
                placeholder="请输入内容"
                v-model="medicalmainpage.attention">
              </el-input>
            </div></el-col>
          </el-row>

        </el-main>
      </el-container>
    </el-container>
    <illness-diagnose :diagnoseill="diagnoseill"></illness-diagnose>
  </div>
</template>

<script>
  import IllnessDiagnose from './IllnessDiagnose'
  import {submitmedicalmainpage,changestatus,addFirstDiagnose,coverMedicalMainPage} from "@/api/diagnose"
    export default {
        name: "MedicalMainPage",
      components:{
        IllnessDiagnose
      },
      data(){
          return{
            medicalmainpage:{
              registid:"",
              medicalrecordid:"",
              chiefcomplaint:"无",
              histofpresill:"无",
              treatofpresill:"无",
              pasthist:"无",
              allergichist:"无",
              physicalexamination:"无",
              attention:"无",
              examsuggestion:"无"
            },
            //用来进行分类
            illlist:[],
            illcata:[],
            //疾病
            illindex:-1,
            //
            diagnoseill:[],

            tableData: [{
              date: '2016-05-03',
              name: '王小虎',
              address: '上海市普陀区金沙江路 1518 弄'
            }, {
              date: '2016-05-02',
              name: '王小虎',
              address: '上海市普陀区金沙江路 1518 弄'
            }]
          }
      },
      computed:{
      },
        props:{
            patient:{
              type:Object,
              default:()=>{
                return{
                  name:"",
                  age:"",
                  gender:"",
                  medicalrecordid:"",
                  registid:""
                }
              }
            },
          cansee3:{
              type:Boolean,
              default:true
          }
        },
      methods:{
          temporarysto(){
            this.$message({
              message:'该功能尚未实现',
              type:'success',
              showClose:true,
              duration:1000
            })
            /*
            将数据保存到数据库，每次在选到这个病人的时候自动显示上次暂存的内容。
            1，在双击病人表单的时候向后端发送请求，接受完之后传到这个组件。
            2，这个组件只需简单的判定，然后再进行绑定。
             */
          },
          submit(){
            if(this.patient.medicalrecordid===""){
              this.$message({
                message: '请先选择病人',
                type: 'failed'
              });

            }else{

              this.medicalmainpage.registid= this.patient.registid;
              this.medicalmainpage.medicalrecordid=this.patient.medicalrecordid;
              for(var a in this.medicalmainpage){
                if(this.medicalmainpage[a]===""){
                  this.$message.error("病历内容不能为空")
                  return;
                }
              }
                this.$confirm("是否提交病历？","提示",{
                  confirmButtonText:"确认",
                  cancelButtonText:"取消",
                  type:"warning"
                }).then(()=>{
                    //检查病历填写情况
                        //修改挂号信息上的诊断状态为已诊断
                    changestatus(this.medicalmainpage.registid).then(response=>{
                      console.log("就诊信息已经修改");
                    }).catch(error=>{
                      return
                      this.$message({
                        message:'就诊信息修改失败',
                        type:'failed',
                        duration:1000
                      })
                    })
                    submitmedicalmainpage(this.medicalmainpage).then(response=>{
                        if(response.data==1){
                          //提交初诊信息
                          for(var a in this.diagnoseill){
                            addFirstDiagnose(this.diagnoseill[a],this.patient.medicalrecordid,this.patient.registid).then(response=>{
                              console.log("初诊信息提交成功")
                            }).catch(error=>{
                              console.log("初诊信息提交失败")
                            })
                          }
                          this.medicalmainpage={};
                          this.patient.medicalrecordid="";
                          this.patient.registid="";
                          this.patient.name="";
                          this.patient.gender="";
                          this.patient.age="";
                          this.$message({
                            message: '病历提交成功',
                            type: 'success'
                          });
                        }else if(response.data==2){
                            this.$confirm("病历信息存在，是否要进行覆盖?","提示",{
                            confirmButtonText:"确认",
                            cancelButtonText:"取消",
                            type:"warning"
                          }).then(()=>{
                            coverMedicalMainPage(this.medicalmainpage).then(response=>{
                              //提交初诊信息
                              for(var a in this.diagnoseill){
                                addFirstDiagnose(this.diagnoseill[a],this.patient.medicalrecordid,this.patient.registid).then(response=>{
                                  console.log("初诊信息提交成功")
                                }).catch(error=>{
                                  console.log("初诊信息提交失败")
                                })
                              }
                              this.medicalmainpage={};
                              this.patient.medicalrecordid="";
                              this.patient.registid="";
                              this.patient.name="";
                              this.patient.gender="";
                              this.patient.age="";
                              this.$message({
                                message: '病历提交成功',
                                type: 'success',
                                duration:1000
                              });
                            }).catch(error=>{
                              this.$message({
                                message: '病历提交失败',
                                type: 'error',
                                duration:1000
                              });
                            })
                          })
                        }
                    }).catch(error=>{
                      console.log("请求失败")
                    })
                }).catch(error=>{
                  console.log("请求错误")
                })
            }
          },
        refreshMsg(){
            for(var key in this.patient){
              this.patient[key]="";
            }
            this.diagnoseill =[]
          this.medicalmainpage={}
        }
      }
    }
</script>


<style scoped>
  .bg-purple {
    background: #b8e6ce;
  }
  .msg{
    min-height: 50px;
    margin-top: 8px;
  }
  .grid-content {
    border-radius: 4px;

    padding-top: 2%;
  }
</style>
