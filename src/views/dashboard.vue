<template>
	<div>
		<el-row :gutter="20">
			<el-col :span="8">


				<el-card shadow="hover" class="mgb20" style="height: 352px">
					<div class="user-info">
						<el-avatar :size="120" :src="imgurl" />
						<div class="user-info-cont">
							<div class="user-info-name">{{ name }}</div>
							<div>{{ role }}</div>
						</div>
					</div>

					<div class="user-info-list">
						上次登录时间：
						<span>2024-02-06</span>
					</div>
					<div class="user-info-list">
						上次登录地点：
						<span>南京</span>
					</div>
				</el-card>

				<el-card shadow="hover" style="height: 372px">
					<template #header>
						<div class="clearfix">
							<span>当前部署</span>
						</div>
					</template>
					fulfilment_004
					<el-progress :percentage="79.4" color="#42b983"></el-progress>
					trade_011
					<el-progress :percentage="34" color="#f1e05a"></el-progress>
					settlement_008
					<el-progress :percentage="15.6"></el-progress>
					goods_021
					<el-progress :percentage="13" color="#f56c6c"></el-progress>
				</el-card>
			</el-col>


			<el-col :span="16">
				<el-row :gutter="20" class="mgb20">


					<el-col :span="8">
						<el-card shadow="hover" :body-style="{ padding: '10px' }">
							<div class="grid-content grid-con-1">
								<el-icon class="grid-con-icon"><Cpu /></el-icon>
								<div class="grid-cont-right">
									<div class="grid-num">4</div>
									<div>微服务数量</div>
								</div>
							</div>
						</el-card>
					</el-col>


					<el-col :span="8">
						<el-card shadow="hover" :body-style="{ padding: '10px' }">
							<div class="grid-content grid-con-2">
								<el-icon class="grid-con-icon"><Cpu /></el-icon>
								<div class="grid-cont-right">
									<div class="grid-num">52</div>
									<div>节点数量</div>
								</div>
							</div>
						</el-card>
					</el-col>
					<el-col :span="8">
						<el-card shadow="hover" :body-style="{ padding: '10px' }">
							<div class="grid-content grid-con-3">
								<el-icon class="grid-con-icon"><List /></el-icon>
								<div class="grid-cont-right">
									<div class="grid-num">6</div>
									<div>策略数量</div>
								</div>
							</div>
						</el-card>
					</el-col>
				</el-row>


<!--        <el-col :span="12">-->
<!--          <el-card shadow="hover">-->
<!--            <schart ref="bar" class="schart" canvasId="bar" :options="options"></schart>-->
<!--          </el-card>-->
<!--        </el-col>-->

        <el-card shadow="hover">
          <schart ref="line" class="schart" canvasId="line" :options="options2"></schart>
        </el-card>

<!--				<el-card shadow="hover" style="height: 603px">-->
<!--					<template #header>-->
<!--						<div class="clearfix">-->
<!--							<span>待运行任务</span>-->
<!--							<el-button style="float: right; padding: 3px 0" text>添加</el-button>-->
<!--						</div>-->
<!--					</template>-->

<!--					<el-table :show-header="false" :data="todoList" style="width: 100%">-->
<!--						<el-table-column width="40">-->
<!--							<template #default="scope">-->
<!--								<el-checkbox v-model="scope.row.status"></el-checkbox>-->
<!--							</template>-->
<!--						</el-table-column>-->
<!--						<el-table-column>-->
<!--							<template #default="scope">-->
<!--								<div-->
<!--									class="todo-item"-->
<!--									:class="{-->
<!--										'todo-item-del': scope.row.status-->
<!--									}"-->
<!--								>-->
<!--									{{ scope.row.title }}-->
<!--								</div>-->
<!--							</template>-->
<!--						</el-table-column>-->
<!--					</el-table>-->
<!--				</el-card>-->


			</el-col>
		</el-row>
		<el-row :gutter="20">
<!--			<el-col :span="12">-->
<!--				<el-card shadow="hover">-->
<!--					<schart ref="bar" class="schart" canvasId="bar" :options="options"></schart>-->
<!--				</el-card>-->
<!--			</el-col>-->
			<el-col :span="12">
<!--				<el-card shadow="hover">-->
<!--					<schart ref="line" class="schart" canvasId="line" :options="options2"></schart>-->
<!--				</el-card>-->
			</el-col>
		</el-row>
	</div>
</template>

<script setup lang="ts" name="dashboard">
import Schart from 'vue-schart';
import { reactive } from 'vue';
import imgurl from '../assets/img/img.jpg';
import {Search} from "@element-plus/icons-vue";

const name = localStorage.getItem('ms_username');
const role: string = name === 'admin' ? '超级管理员' : '普通用户';

const options = {
	type: 'bar',
	title: {
		text: '最近一周各品类销售图'
	},
	xRorate: 25,
	labels: ['周一', '周二', '周三', '周四', '周五'],
	datasets: [
		{
			label: '家电',
			data: [234, 278, 270, 190, 230]
		},
		{
			label: '百货',
			data: [164, 178, 190, 135, 160]
		},
		{
			label: '食品',
			data: [144, 198, 150, 235, 120]
		}
	]
};
const options2 = {
	type: 'line',
	title: {
		text: '最近5分钟各服务QPS'
	},
	labels: ['11:23:49', '11:24:49', '11:25:49', '11:26:49', '11:27:49'],
	datasets: [
		{
			label: 'goods',
			data: [234, 278, 270, 190, 230]
		},
		{
			label: 'trade',
			data: [164, 178, 150, 135, 160]
		},
		{
			label: 'fulfilment',
			data: [74, 118, 200, 235, 90]
		},
    {
      label: 'settlement',
      data: [174, 218, 300, 335, 190]
    }
	]
};
const todoList = reactive([
	{
		title: '今天要修复100个bug',
		status: false
	},
	{
		title: '今天要修复100个bug',
		status: false
	},
	{
		title: '今天要写100行代码加几个bug吧',
		status: false
	},
	{
		title: '今天要修复100个bug',
		status: false
	},
	{
		title: '今天要修复100个bug',
		status: true
	},
	{
		title: '今天要写100行代码加几个bug吧',
		status: true
	}
]);
</script>

<style scoped>
.el-row {
	margin-bottom: 20px;
}

.grid-content {
	display: flex;
	align-items: center;
	height: 100px;
}

.grid-cont-right {
	flex: 1;
	text-align: center;
	font-size: 14px;
	color: #999;
}

.grid-num {
	font-size: 30px;
	font-weight: bold;
}

.grid-con-icon {
	font-size: 50px;
	width: 100px;
	height: 100px;
	text-align: center;
	line-height: 100px;
	color: #fff;
}

.grid-con-1 .grid-con-icon {
	background: rgb(45, 140, 240);
}

.grid-con-1 .grid-num {
	color: rgb(45, 140, 240);
}

.grid-con-2 .grid-con-icon {
	background: rgb(100, 213, 114);
}

.grid-con-2 .grid-num {
	color: rgb(100, 213, 114);
}

.grid-con-3 .grid-con-icon {
	background: rgb(242, 94, 67);
}

.grid-con-3 .grid-num {
	color: rgb(242, 94, 67);
}

.user-info {
	display: flex;
	align-items: center;
	padding-bottom: 20px;
	border-bottom: 2px solid #ccc;
	margin-bottom: 20px;
}

.user-info-cont {
	padding-left: 50px;
	flex: 1;
	font-size: 14px;
	color: #999;
}

.user-info-cont div:first-child {
	font-size: 30px;
	color: #222;
}

.user-info-list {
	font-size: 14px;
	color: #999;
	line-height: 25px;
}

.user-info-list span {
	margin-left: 70px;
}

.mgb20 {
	margin-bottom: 20px;
}

.todo-item {
	font-size: 14px;
}

.todo-item-del {
	text-decoration: line-through;
	color: #999;
}

.schart {
	width: 100%;
	height: 563px;
}
</style>
