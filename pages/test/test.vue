<template>
	<!-- 使用 useVirtual 属性开启虚拟滚动 使用虚拟滚动时，必须要固定表格高度和行高 -->
	<div style="height: 100%;width: 100%;padding: 0 30px">
		<div class="table_toolbar">
			<button class="mini-btn" style="margin: 0 10rpx" type="default" size="mini" @click="setData(3)">变化数据为3条</button>
			<button class="mini-btn" type="default" size="mini" @click="setData(200)">变化数据为200条</button>
		</div>
		<!--我是加入分页的表格-->
		<view class="tableWrap">
			<pl-table
				:data="data.tableData"
				big-data-checkbox
				:max-height="height"
				header-drag-style
				fixedColumnsRoll
				use-virtual
				:row-height="rowHeight"
				:pagination-show="true"
				:total="pageForm.total"
				:page-size="pageForm.pageSize"
				:current-page="pageForm.currentPage"
				@handlePageSize="handlePageSize"
			>
				<template slot="empty">
					没有查询到符合条件的记录
				</template>
				<pl-table-column type="selection" width="55" />
				<pl-table-column type="index" width="100" fixed />
				<!--show-overflow-tooltip属性代表超出则内容部分给出提示框-->
				<pl-table-column
					v-for="item in columns"
					:key="item.id"
					:resizable="item.resizable"
					:show-overflow-tooltip="item.showOverflowTooltip"
					:prop="item.prop"
					:label="item.label"
					:fixed="item.fixed"
					:width="item.width"
				/>
			</pl-table>
		</view>
	</div>
</template>

<script>
var dataList = Array.from({ length: 500 }, (_, idx) => ({
	id: idx + '_' + 1,
	date: '2016-05-03',
	name: 1,
	ab: '欢迎使用pl-table',
	address: idx,
	children: Array.from({ length: 3 }, (_, idx2) => ({
		id: idx + '_' + idx2 + '_' + 1,
		date: '2016-05-03',
		name: 1,
		ab: '欢迎使用pl-table',
		address: idx + '_' + idx2,
		children: Array.from({ length: 1 }, (_, idx3) => ({
			id: idx + '_' + idx2 + '_' + idx3 + '_' + 1,
			date: '2016-05-03',
			name: 1,
			ab: '欢迎使用pl-table',
			address: idx + '_' + idx2 + '_' + idx3
		}))
	}))
}));

export default {
	name: 'home',
	data() {
		return {
			rowHeight: 50,
			columns: [
				{ prop: 'address', label: '日期', width: 120, treeNode: true, showOverflowTooltip: true },
				{ prop: 'address', label: '地址', width: 100, showOverflowTooltip: true },
				{ prop: 'address', label: '噜噜噜', width: 230, showOverflowTooltip: true },
				{ prop: 'address', label: '娃哈哈', width: 100, showOverflowTooltip: true },
				{ prop: 'address', label: '地址', width: 100, showOverflowTooltip: true },
				{ prop: 'address', label: '娃哈哈', width: 100, showOverflowTooltip: true },
				{ prop: 'address', label: '娃哈哈', width: 100, showOverflowTooltip: true },
				{ prop: 'address', label: '地址', width: 100, showOverflowTooltip: true },
				{ prop: 'address', label: '娃哈哈', width: 100, showOverflowTooltip: true },
				{ prop: 'address', label: '娃哈哈', width: 100, showOverflowTooltip: true },
				{ prop: 'address', label: '噜噜噜', showOverflowTooltip: true },
				{ prop: 'address', label: '娃哈哈', width: 100, showOverflowTooltip: true, fixed: 'right' }
			],
			columns2: Array.from({ length: 20 }, (_, idx) => ({
				prop: 'address',
				label: '地址' + idx,
				width: 200,
				showOverflow: true,
				sortable: true,
				fixed: ''
			})),
			data: {
				tableData: Array.from({ length: 20 }, (_, idx) => ({
					id: idx + 1,
					date: '2016-05-03',
					name: 1,
					ab: '欢迎使用pl-table',
					address: 1 + idx
				}))
			},
			top: 0,
			height: 500,
			pageForm: {
				total: 1000,
				pageSize: 10,
				currentPage: 1
			},
			treeData: dataList
		};
	},
	methods: {
		selectAll(val) {
			console.log(val);
		},
		selectable(row, index) {
			if (index === 1) {
				return false;
			} else {
				return true;
			}
		},
		setHei(val) {
			this.height = val;
		},
		tableBodyScroll({ scrollTop }, e) {
			this.top = scrollTop;
		},
		allSelection() {
			this.$refs.plTable.toggleAllSelection();
		},
		clearSelection() {
			this.$refs.plTable.clearSelection();
			this.$refs.plTable2.clearSelection();
		},
		setData(num) {
			this.data.tableData = Array.from({ length: num }, (_, idx) => ({
				id: idx + 1,
				date: '2016-05-03',
				name: 1,
				ab: '欢迎使用pl-table',
				address: 1 + idx
			}));
		},
		scrollBottom() {
			this.$refs.plTable.scrollBottom();
		},
		pagingScrollTopLeft(val) {
			this.$refs.plTable.pagingScrollTopLeft(val, 0);
		},
		// 分页事件
		handlePageSize({ page, size }) {
			console.log(page, size);
		},
		// 获取已经展开的节点
		getTreeExpansionEvent() {
			console.log(this.$refs.plTreeTable.getTreeExpandRecords());
		}
	}
};
</script>
<style>
body,
html {
	margin: 0;
	box-sizing: border-box;
	width: 100%;
	height: 100%;
}
body ::-webkit-scrollbar-thumb {
	-webkit-border-radius: 5px;
	border-radius: 5px;
	background-color: rgba(144, 147, 153, 0.1);
}
.selectTr td {
	background: #f5f5f5 !important;
	color: red !important;
}
</style>
