<!--
 * @Author: 陈巧龙
 * @Date: 2023-12-08 09:44:43
 * @LastEditors: Please set LastEditors
 * @LastEditTime: 2023-12-08 16:19:41
 * @FilePath: \DW-Systems\src\components\yzt\LeftView.vue
 * @Description: 一张图左区域
-->
<script setup>
import bus from 'vue3-eventbus'
import { ref, onMounted } from 'vue'
import BarChart from '@/components/common/charts/BarChart.vue'
import PieChart from '@/components/common/charts/PieChart.vue'

//控制上沿距离
const top = '13%'
//x坐标数据
const xData = ['市辖区', '夷陵区', '远安县', '兴山县', '秭归县', '长阳县', '五峰县', '宜都县', '当阳市', '枝江市']
//柱状体颜色
const color1 = 'rgb(0,157,230)'
const color2 = 'rgb(118,131,246)'
//表格数据
const series1 = [
    {
        name: '雨量(mm)',
        data: [1.20, 2.48, 2.04, 4.90, 1.74, 1.20, 2.48, 2.04, 4.90, 1.74],
    },
]
const series2 = [
    {
        name: '监测点数(个)',
        data: [120, 248, 204, 490, 174, 120, 248, 204, 490, 175],
    },
]
//饼图颜色
let color3 = [
    "#4fc5ea",
    "#6c6fbf",
    "#5ed8a9",
    "#8f55e7",
    "#605ad8",
];

//饼图数据
const series3 = [
    {
        name: '监测点数量',
        data: [
            { value: 1240, name: '滑坡' },
            { value: 158, name: '不稳定斜坡' },
            { value: 85, name: '崩塌' },
            { value: 5, name: '地面塌陷' },
            { value: 4, name: '泥石流' }
        ],
    },
]
const parentMethod = (data) => {
    console.log('data', data)
}
//默认选择第三个选项
const selectValue = ref('168h')

const options = [
    {
        value: '1h',
        label: '近一小时',
    },
    {
        value: '24h',
        label: '近24小时',
    },
    {
        value: '168h',
        label: '近7天',
    },
]

onMounted(() => {

})

//初始化显示第一个icon
const currentIcon = ref(true);

function changeValue() {
    console.log(selectValue.value)
}
//发送页面
function handleIconClick() {
    bus.emit('windowVisible', currentIcon.value)
    currentIcon.value = !currentIcon.value
}

</script>

<template>
    <div class="main-container">
        <div class="main-page">
            <div class="echart-container">
                <div class="container1">
                    <div class="container-icon">
                        <el-icon color="#1979C4">
                            <DataAnalysis />
                        </el-icon>
                        <span>累计降雨量</span>
                    </div>
                    <el-select v-model="selectValue" class="rain-select" placeholder="近7天" size="small"
                        @change="changeValue">
                        <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value" />
                    </el-select>
                </div>
                <div class="charts1">
                    <bar-chart :xData="xData" :series="series1" :color="color1" :top="top" :id="'charts1'"
                        @parentMethod="parentMethod"></bar-chart>
                </div>
            </div>
            <div class="echart-container">
                <div class="container-icon">
                    <el-icon color="#1979C4">
                        <Tickets />
                    </el-icon>
                    <span>监测点分布</span>
                </div>
                <div class="charts2">
                    <bar-chart :xData="xData" :series="series2" :color="color2" :top="top" :id="'charts2'"
                        @parentMethod="parentMethod"></bar-chart>
                </div>
            </div>
            <div class="echart-container">
                <div class="container-icon">
                    <el-icon color="#1979C4">
                        <Compass />
                    </el-icon>
                    <span>灾害类型</span>
                </div>
                <div class="charts3">
                    <pie-chart :series="series3" :color="color3" :top="top" :id="'charts3'"
                        @parentMethod="parentMethod"></pie-chart>
                </div>
            </div>
        </div>
        <div class="icon" @click="handleIconClick">
            <el-icon color="white" v-if="currentIcon">
                <CaretLeft />
            </el-icon>

            <el-icon color="white" v-else>
                <CaretRight />
            </el-icon>
        </div>
    </div>
</template>

<style scoped lang="scss">
.main-container {
    margin: 0;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    padding-left: 10px;

    .main-page {
        width: calc(100% - 14px);
        height: 97%;
        display: flex;
        flex-direction: column;
        justify-content: space-between;

        .echart-container {
            height: 32%;
            background-color: white;

            span {
                display: flex;
                align-items: center;
                font-size: 14px;
                font-weight: 600;
                color: #666666;
                height: 35px;
                margin-left: 10px;
            }

            .container1 {
                display: flex;
                align-items: center;
                justify-content: space-between;
                margin-right: 10px;

                .rain-select {
                    width: 30%;
                }

            }

            .charts1,
            .charts2,
            .charts3 {
                height: calc(100% - 35px);
            }

            .container-icon {
                display: flex;
                align-items: center;
                margin-left: 10px;
            }
        }
    }

    .icon {
        top: 50%;
        background-color: rgba(51, 122, 179, 0.9);
        height: 32px;
        width: 14px;
        display: flex;
        align-items: center;
        cursor: pointer;
    }
}
</style>