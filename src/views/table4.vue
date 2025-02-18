<template>
	<div>
		<div class="container">
			<div class="search-box">
				<el-input v-model="query.name" placeholder="微服务" class="search-input mr10" clearable></el-input>
				<el-button type="primary" :icon="Search" @click="handleSearch">搜索</el-button>
				<el-button type="warning" :icon="CirclePlusFilled" @click="visible = true">新增</el-button>
			</div>
			<el-table :data="tableData" border class="table" ref="multipleTable" header-cell-class-name="table-header">
				<el-table-column prop="id" label="ID" width="55" align="center"></el-table-column>
				<el-table-column prop="name" label="策略名称" align="center"></el-table-column>
				<el-table-column prop="type" label="策略类型" align="center"></el-table-column>
				<el-table-column prop="create_time" label="创建时间" align="center"></el-table-column>
				<el-table-column prop="update_time" label="更新时间" align="center"></el-table-column>
				<el-table-column label="工作状态" align="center">
					<template #default="scope">
						<el-tag :type="scope.row.state ? 'success' : 'danger'">
							{{ scope.row.state ? '正常' : '异常' }}
						</el-tag>
					</template>
				</el-table-column>

				<el-table-column label="操作" width="280" align="center">
					<template #default="scope">
						<el-button type="warning" size="small" :icon="View" @click="handleView(scope.row)">
							查看
						</el-button>

            <el-button
                type="danger"
                size="small"
                :icon="Edit"
                @click="handleEdit(scope.$index, scope.row)"
                v-permiss="15"
            >
              删除
            </el-button>

						<el-button
							type="primary"
							size="small"
							:icon="Edit"
							@click="handleEdit(scope.$index, scope.row)"
							v-permiss="15"
						>
							编辑
						</el-button>
					</template>
				</el-table-column>
			</el-table>
			<div class="pagination">
				<el-pagination
					background
					layout="total, prev, pager, next"
					:current-page="query.pageIndex"
					:page-size="query.pageSize"
					:total="pageTotal"
					@current-change="handlePageChange"
				></el-pagination>
			</div>
		</div>
		<el-dialog
			:title="idEdit ? '编辑用户' : '新增用户'"
			v-model="visible"
			width="500px"
			destroy-on-close
			:close-on-click-modal="false"
			@close="closeDialog"
		>
			<TableEdit :data="rowData" :edit="idEdit" :update="updateData" />
		</el-dialog>
		<el-dialog title="查看用户详情" v-model="visible1" width="700px" destroy-on-close>
			<TableDetail :data="rowData" />
		</el-dialog>
	</div>
</template>

<script setup lang="ts" name="basetable">
import { ref, reactive } from 'vue';
import { ElMessage, ElMessageBox } from 'element-plus';
import { Delete, Edit, Search, CirclePlusFilled, View } from '@element-plus/icons-vue';
import { fetchData } from '../api/index';
import TableEdit from '../components/table-edit.vue';
import TableDetail from '../components/table-detail.vue';

interface TableItem {
	id: number;
	name: string;
	label: string;
	type: string;
	create_time: string;
	update_time: string;
	state: string;
}

const query = reactive({
	address: '',
	name: '',
	pageIndex: 1,
	pageSize: 10
});
const tableData = ref<TableItem[]>([]);
const pageTotal = ref(0);
// 获取表格数据
const getData = async () => {
	tableData.value = [
    {
      id: 1,
      name: "改进蚁群算法",
      label: "groupon_service",
      type: "负载均衡",
      create_time: "2024-02-06 14:25:09",
      update_time: "2024-02-06 14:25:09",
      state: "正常",
    },
    {
      id: 2,
      name: "经典蚁群算法",
      label: "groupon_service",
      type: "负载均衡",
      create_time: "2024-02-06 14:34:11",
      update_time: "2024-02-06 14:34:11",
      state: "正常",
    },
    {
      id: 3,
      name: "一致性哈希算法",
      label: "groupon_service",
      type: "负载均衡",
      create_time: "2024-02-06 14:34:34",
      update_time: "2024-02-06 14:34:34",
      state: "正常",
    },
    {
      id: 4,
      name: "改进BBR限流算法",
      label: "groupon_service",
      type: "服务限流",
      create_time: "2024-02-06 16:47:34",
      update_time: "2024-02-06 16:47:34",
      state: "正常",
    },
    {
      id: 4,
      name: "滑动窗口限流算法",
      label: "groupon_service",
      type: "服务限流",
      create_time: "2024-02-06 16:47:42",
      update_time: "2024-02-06 16:47:42",
      state: "正常",
    },
    {
      id: 4,
      name: "令牌桶限流算法",
      label: "groupon_service",
      type: "服务限流",
      create_time: "2024-02-06 16:47:56",
      update_time: "2024-02-06 16:47:56",
      state: "正常",
    },
  ];
	pageTotal.value = 1;
};
getData();

// 查询操作
const handleSearch = () => {
	query.pageIndex = 1;
	getData();
};
// 分页导航
const handlePageChange = (val: number) => {
	query.pageIndex = val;
	getData();
};

// 删除操作
const handleDelete = (index: number) => {
	// 二次确认删除
	ElMessageBox.confirm('确定要删除吗？', '提示', {
		type: 'warning'
	})
		.then(() => {
			ElMessage.success('删除成功');
			tableData.value.splice(index, 1);
		})
		.catch(() => {});
};

const visible = ref(false);
let idx: number = -1;
const idEdit = ref(false);
const rowData = ref({});
const handleEdit = (index: number, row: TableItem) => {
	idx = index;
	rowData.value = row;
	idEdit.value = true;
	visible.value = true;
};
const updateData = (row: TableItem) => {
	idEdit.value ? (tableData.value[idx] = row) : tableData.value.unshift(row);
	console.log(tableData.value);
	closeDialog();
};

const closeDialog = () => {
	visible.value = false;
	idEdit.value = false;
};

const visible1 = ref(false);
const handleView = (row: TableItem) => {
	rowData.value = row;
	visible1.value = true;
};
</script>

<style scoped>
.search-box {
	margin-bottom: 20px;
}

.search-input {
	width: 200px;
}

.mr10 {
	margin-right: 10px;
}
.table-td-thumb {
	display: block;
	margin: auto;
	width: 40px;
	height: 40px;
}
</style>
