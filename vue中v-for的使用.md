---
title: vue中v-for与v-if的结合使用
date: 2018-03-06 13:42:03
tags: [vue]
---

项目中需要封装一个steps步骤条，使用v-for与v-if结合来动态控制应该显示的阶段，mark一下便于以后查找记忆

    <template>
      <el-steps :active="activeValue" align-center>
        <el-step  v-for="item in caseProcess" v-if="item.key < activeValue"  :key="item.key" :title="stepsName+item.key" :description="item.value"></el-step>
      </el-steps>
    </template>  

    export default {
        name: "buildTable",
        directives: {
        waves
        },
        data() {
            return {
              activeValue: undefined,
              stepsName: '阶段',
            }
        }
    }
---   
>值得注意的是v-for使用的时候一定要添加`:key='item.key'`，否则会报异常warning，`component lists rendered with v-for should have explicit keys. `  