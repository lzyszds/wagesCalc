<template>
  <button class="noselect" @click="onOut">
    <span class="text">退出登陆</span>
    <span class="icon">
      <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
        <path
          d="M24 20.188l-8.315-8.209 8.2-8.282-3.697-3.697-8.212 8.318-8.31-8.203-3.666 3.666 8.321 8.24-8.206 8.313 3.666 3.666 8.237-8.318 8.285 8.203z">
        </path>
      </svg>
    </span>
  </button>
  <div class="home">
    <div class="standard">
      <el-divider content-position="left">工资标准设定</el-divider>
      <el-form :inline="true" :model="form" class="demo-form-inline">
        <el-form-item label="基本工资">
          <el-input class="forminput" v-model.number="form.basic" />
        </el-form-item>
        <el-form-item label="岗位工资">
          <el-select v-model="form.jobsName" @change="jobsNameFn">
            <el-option :label="form.jobsTop.label" value="jobsTop" />
            <el-option :label="form.jobsDirector.label" value="jobsDirector" />
            <el-option :label="form.jobsDirectorOf.label" value="jobsDirectorOf" />
            <el-option :label="form.jobsOrdinary.label" value="jobsOrdinary" />
          </el-select>
          <el-input class="jobs" @change="jobsFn" v-model.number="form.jobs" />
        </el-form-item>
        <el-form-item label="岗位增长工资">
          <el-select v-model="form.growthName" @change="growthNameFn">
            <el-option :label="form.jobsTop.label" value="jobsTop" />
            <el-option :label="form.jobsDirector.label" value="jobsDirector" />
            <el-option :label="form.jobsDirectorOf.label" value="jobsDirectorOf" />
            <el-option :label="form.jobsOrdinary.label" value="jobsOrdinary" />
          </el-select>
          <el-input class="jobs" @change="growthFn" v-model.number="form.growth" />
        </el-form-item>
        <el-form-item label="月绩效工资">
          <el-input class="forminput" v-model.number="form.pre" />
        </el-form-item>
        <el-form-item label="月生活津贴">
          <el-input class="forminput" v-model.number="form.allowance" />
        </el-form-item>
        <el-form-item label="全勤天数">
          <el-input class="forminput" v-model.number="form.fullTime" />
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="onSubmit">是否保存</el-button>
        </el-form-item>
      </el-form>
    </div>
    <el-divider content-position="left">员工信息表</el-divider>
    <div class="empInfo">
      <el-table :data="infoState" style="width: 100%" max-height="250">
        <el-table-column label="员工号" width="120">
          <template #default="scope">
            <input class="update" type="text" v-model="infoState[scope.$index].id">
          </template>
        </el-table-column>
        <el-table-column label="名字" width="120">
          <template #default="scope">
            <input class="update" type="text" v-model="infoState[scope.$index].name">
          </template>
        </el-table-column>
        <el-table-column label="岗位">
          <template #default="scope">
            <el-select v-model="infoState[scope.$index].position">
              <el-option :label="form.jobsTop.label" :value="form.jobsTop.label" />
              <el-option :label="form.jobsDirector.label" :value="form.jobsDirector.label" />
              <el-option :label="form.jobsDirectorOf.label" :value="form.jobsDirectorOf.label" />
              <el-option :label="form.jobsOrdinary.label" :value="form.jobsOrdinary.label" />
            </el-select>
          </template>
        </el-table-column>
        <el-table-column label="级别" width="120">
          <template #default="scope">
            <el-select v-model="infoState[scope.$index].lengthOfService">
              <el-option v-for="(item, index) in ['1级', '2级', '3级', '4级', '5级', '6级', '7级', '8级', '9级', '10级']"
                :key="index" :label="item" :value="item" />
            </el-select>
          </template>
        </el-table-column>
        <el-table-column label="出勤天数" width="120">
          <template #default="scope">
            <input class="update" type="text" v-model="infoState[scope.$index].workingDays">
          </template>
        </el-table-column>
        <el-table-column label="绩效分" width="120">
          <template #default="scope">
            <input class="update" type="text" v-model="infoState[scope.$index].performPoints">
          </template>
        </el-table-column>
        <el-table-column label="加班天数" width="120">
          <template #default="scope">
            <input class="update" type="text" v-model="infoState[scope.$index].daysWorkOvertime">
          </template>
        </el-table-column>
        <el-table-column label="计件数" width="120">
          <template #default="scope">
            <input class="update" type="text" v-model="infoState[scope.$index].piece">
          </template>
        </el-table-column>



        <el-table-column label="Operations">
          <template #default="scope">
            <el-button link type="danger" size="small" @click.prevent="deleteRow(scope.$index)">
              删除
            </el-button>
          </template>
        </el-table-column>
      </el-table>
      <el-button class="mt-4" style="width: 100%" @click="onAddItem">添加员工</el-button>
      <el-button class="mt-4" style="width: 100%" @click="onCalc">计算工资</el-button>
    </div>
    <el-divider content-position="left">员工工资表</el-divider>
    <div class="table">
      <el-table :data="tableData" table-layout="auto" max-height="250">
        <el-table-column prop="id" label="员工号" />
        <el-table-column prop="name" label="名字" />
        <el-table-column prop="position" label="岗位" width="200px" />
        <el-table-column prop="basic" label="基本工资" />
        <el-table-column prop="workingDays" label="岗位工资" />
        <el-table-column prop="lengthOfService" label="岗位级别工资" />
        <el-table-column prop="allowance" label="生活津贴" />
        <el-table-column prop="pre" label="绩效工资" />
        <el-table-column prop="daysWorkOvertime" label="加班工资" />
        <el-table-column prop="piece" label="计件工资" />
        <el-table-column prop="sums" label="总工资" />
      </el-table>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { reactive, ref } from 'vue'
import { useRouter } from 'vue-router'
//定义路由方法
const router = useRouter()

//退出登陆
const onOut = () => {
  localStorage.removeItem('_token')
  router.push('/')
}

const form = reactive({
  basic: 800, //基本工资
  jobs: 8000, //基本岗位工资
  jobsName: 'jobsTop', //岗位工资名称
  growth: 500,  //岗位工资增长
  growthName: 'jobsTop',  //岗位工资增长名称
  jobsTop: {
    label: '厂长及以上岗位',
    value: 8000,
    growth: 500
  },
  jobsDirector: {
    label: '部门主管及以上岗位',
    value: 6000,
    growth: 300
  },
  jobsDirectorOf: {
    label: '车间主任、工段长、班组长',
    value: 4500,
    growth: 150
  },
  jobsOrdinary: {
    label: '普通员工',
    value: 3000,
    growth: 100
  },
  pre: 300, //基本绩效工资
  allowance: 100, //基本生活津贴
  fullTime: 27  //基本全勤天数
})

//判断是否有默认设置，没有则设置默认设置
if (!localStorage.getItem('form')) {
  localStorage.setItem('form', JSON.stringify(form))
} else {
  Object.assign(form, JSON.parse(localStorage.getItem('form') || ''))
}

//是否确定将默认设置保存在浏览器中
const onSubmit = () => {
  localStorage.setItem('form', JSON.stringify(form))
}

//定义员工工资条 类型接口
interface tableDataType {
  name: string, //名字
  id: string, //员工号
  position: string, //岗位
  workingDays: number, //出勤天数
  basic: number, //基本工资
  positionPie: number, //岗位工资
  allowance: number, //生活津贴
  pre: number, //绩效工资
  lengthOfService: number, //岗位级别工资
  daysWorkOvertime: number, //加班工资
  piece: number, //计件数
  sums: number, //总工资
}
//定义员工信息状态 类型接口
interface infoStateType {
  name: string, //名字
  id: string, //员工号
  workingDays: number,  //出勤天数
  performPoints: number,  //绩效分
  daysWorkOvertime: number, //加班天数
  position: string, //岗位
  lengthOfService: string,  //岗位级别
  piece: number,  //计件数
}
//定义员工工资条
const tableData = ref<tableDataType[]>([])
//定义员工信息状态
const infoState = ref<infoStateType[]>([])
//删除不计算的员工信息
const deleteRow = (index: number) => {
  infoState.value.splice(index, 1)
}
//添加员工信息
const onAddItem = () => {
  infoState.value.push({
    name: 'Tom',
    id: '001',
    workingDays: 28,
    performPoints: 100,
    daysWorkOvertime: 0,
    position: '普通员工',
    lengthOfService: '1级',
    piece: 0,
  })
  let info = infoState.value[infoState.value.length - 1]
  info.daysWorkOvertime = info.workingDays - form.fullTime < 0 ? 0 : info.workingDays - form.fullTime
  // console.log(infoState.value);
}
onAddItem()
//计算取两位小数
const toFixed = (num: number) => {
  return Math.round(num * 100) / 100
}
// 切换岗位工资
const jobsNameFn = () => {
  form.jobs = form[form.jobsName].value
}
//更改岗位工资
const jobsFn = () => {
  form[form.jobsName].value = form.jobs
}
// 切换增长率
const growthNameFn = () => {
  form.growth = form[form.growthName].growth
}
//更改增长率
const growthFn = () => {
  form[form.growthName].growth = form.growth
}

//计算工资
const onCalc = () => {
  tableData.value = []
  infoState.value.forEach((res, index) => {
    let posipie = null // 定义岗位工资增长率变量
    //将员工信息添加到工资条中
    tableData.value.push({
      name: res.name,
      id: res.id,
      position: res.position,
      basic: form.basic,
      positionPie: 0,
      allowance: form.allowance,
      pre: form.pre,
      lengthOfService: 0,
      workingDays: 0,
      daysWorkOvertime: 0,
      piece: res.piece,
      sums: 0
    })
    //计算岗位工资 并获得岗位工资增长率
    switch (res.position) {
      case '厂长及以上岗位':
        // 获取岗位级别工资
        tableData.value[index].positionPie = form.jobsTop.value
        // 获取岗位级别工资增长率
        posipie = form.jobsTop.growth
        break;
      case '部门主管及以上岗位':
        // 获取岗位级别工资
        tableData.value[index].positionPie = form.jobsDirector.value
        // 获取岗位级别工资增长率
        posipie = form.jobsDirector.growth
        break;
      case '车间主任、工段长、班组长':
        // 获取岗位级别工资
        tableData.value[index].positionPie = form.jobsDirectorOf.value
        // 获取岗位级别工资增长率
        posipie = form.jobsDirectorOf.growth
        break;
      case '普通员工':
        // 获取岗位级别工资
        tableData.value[index].positionPie = form.jobsOrdinary.value
        // 获取岗位级别工资增长率
        posipie = form.jobsOrdinary.growth
        break;
    }
    //计算岗位工资  月岗位工资标准÷全勤天数×出勤天数=实发工资  出勤天数大于全勤天数时，按全勤天数计发
    tableData.value[index].workingDays = toFixed(
      (tableData.value[index].positionPie / form.fullTime) *
      (res.workingDays > form.fullTime ? form.fullTime : res.workingDays)
    )
    //计算岗位级别工资
    tableData.value[index].lengthOfService = (Number(res.lengthOfService.split('级')[0]) - 1) * posipie!
    //生活补贴工资 月生活津贴标准÷全勤天数×出勤天数=实发工资  出勤天数大于全勤天数时，按全勤天数计发
    tableData.value[index].allowance = toFixed(
      (form.allowance / form.fullTime) *
      (res.workingDays > form.fullTime ? form.fullTime : res.workingDays)
    )
    /* 计算绩效工资 
      实际得分90分以上（含90分）全额计发绩效工资
      实际得分90分以下，按实际得分数的%计发绩效工资。
    */
    tableData.value[index].pre = res.performPoints >= 90 ? form.pre : toFixed((res.performPoints / 100) * form.pre)
    //计算加班工资 月基本工资标准÷全勤天数×加班天数=实发工资
    tableData.value[index].daysWorkOvertime = toFixed((form.basic / form.fullTime) * res.daysWorkOvertime)
    //计算总工资 基本工资+岗位工资+生活补贴工资+绩效工资+岗位级别工资+加班工资+计件工资
    tableData.value[index].sums = toFixed(
      tableData.value[index].basic +
      tableData.value[index].workingDays +
      tableData.value[index].allowance +
      tableData.value[index].pre +
      tableData.value[index].lengthOfService +
      tableData.value[index].daysWorkOvertime +
      tableData.value[index].piece
    )
  })
}
</script>
<style scoped lang="less">
.home {
  width: 1300px;
  height: 100vh;
  margin: 0 auto;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;

  &>.standard {
    width: 100% !important;
    // box-shadow: 0px 0px 8px 2px rgba(0, 0, 0, 0.1);
    padding: 10px;
    padding-bottom: 0;

    // border-radius: 10px !important;
    .jobs {
      width: 100px;
      margin-left: 10px;
    }

    .forminput {
      width: 150px;
      text-align: left;
    }
  }

  &>.empInfo {
    width: 100% !important;
    height: 300px;
    box-shadow: 0px 0px 8px 2px rgba(0, 0, 0, 0.1);
    padding: 10px;
    border-radius: 10px !important;

    &>.el-table {
      width: 100% !important;
      height: 100% !important;
      border-radius: 10px !important;

      .el-select {
        width: 90% !important;
      }
    }

    &>button {
      width: 40% !important;
      margin-top: 10px !important;
      margin-left: 5% !important;
    }

    .update {
      width: 90%;
      text-align: center;
      border: none;
      border-bottom: 1px solid #ccc;
      padding-bottom: 8px;
      text-align: left;
      outline: none;
      background-color: transparent;

    }
  }

  &>.table {
    width: 100% !important;
    height: 300px;
    box-shadow: 0px 0px 8px 2px rgba(222, 189, 189, 0.1);
    padding: 10px;
    border-radius: 10px !important;
  }
}

.el-table .warning-row {
  --el-table-tr-bg-color: var(--el-color-warning-light-9);
}

.el-table .success-row {
  --el-table-tr-bg-color: var(--el-color-success-light-9);
}

.demo-form-inline {
  padding: 10px 20px;

  .el-form-item {
    margin-right: 30px;

    :deep(.el-form-item__label) {
      width: 100px;
    }
  }
}

button.noselect {
  position: absolute;
  width: 150px;
  height: 50px;
  cursor: pointer;
  display: flex;
  align-items: center;
  background: red;
  border: none;
  border-radius: 5px;
  box-shadow: 1px 1px 3px rgba(0, 0, 0, 0.15);
  background: #e62222;

  &,
  & span {
    transition: 200ms;
  }

  & .text {
    transform: translateX(35px);
    color: white;
    font-weight: bold;
  }

  & .icon {
    position: absolute;
    border-left: 1px solid #c41b1b;
    transform: translateX(110px);
    height: 40px;
    width: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  & svg {
    width: 15px;
    fill: #eee;
  }

  &:hover {
    background: #ff3636;
  }

  &:hover .text {
    color: transparent;
  }

  &:hover .icon {
    width: 150px;
    border-left: none;
    transform: translateX(0);
  }

  &:focus {
    outline: none;
  }

  &:active .icon svg {
    transform: scale(0.8);
  }
}
</style>
