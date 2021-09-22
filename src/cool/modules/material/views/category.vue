<template>
	<cl-crud :ref="setRefs('crud')" @load="onLoad">
		<el-row type="flex" align="middle">
			<!-- 刷新按钮 -->
			<cl-refresh-btn />
			<!-- 新增按钮 -->
			<cl-add-btn />
			<!-- 删除按钮 -->
			<cl-multi-delete-btn />
			<cl-flex1 />
			<!-- 关键字搜索 -->
			<cl-search-key />
		</el-row>

		<el-row>
			<!-- 数据表格 -->
			<cl-table :ref="setRefs('table')" v-bind="table" />
		</el-row>

		<el-row type="flex">
			<cl-flex1 />
			<!-- 分页控件 -->
			<cl-pagination />
		</el-row>

		<!-- 新增、编辑 -->
		<cl-upsert :ref="setRefs('upsert')" v-bind="upsert" />
	</cl-crud>
</template>

<script lang="ts">
import { defineComponent, inject, reactive } from "vue";
import { CrudLoad, Upsert, Table } from "cl-admin-crud-vue3/types";
import { useRefs } from "/@/core";

export default defineComponent({
	setup() {
		const { refs, setRefs } = useRefs();
		// 请求服务
		const service = inject<any>("service");

		// 新增、编辑配置
		const upsert = reactive<Upsert>({
			items: []
		});

		// 表格配置
		const table = reactive<Table>({
			columns: [
				{
					prop: "name",
					label: "分类名称"
				},
				{
					prop: "type",
					label: "类型",
					dict: [
						{
							label: "图片",
							value: 0
						},
						{
							label: "视频",
							value: 1
						},
						{
							label: "文档",
							value: 2
						},
						{
							label: "其他",
							value: 3
						}
					]
				},
				{
					prop: "px",
					label: "尺寸参数"
				},
				{
					prop: "kb",
					label: "大小限制"
				},
				{
					prop: "article_count",
					label: "素材数量"
				},
				{
					prop: "sort",
					label: "排序"
				},
				{
					prop: "remark",
					label: "描述"
				},
				{
					prop: "updateTime",
					label: "更新时间",
					sortable: "custom",
					width: 150
				},
				{
					prop: "status",
					label: "状态",
					dict: [
						{
							label: "禁用",
							value: 0
						},
						{
							label: "启用",
							value: 1
						}
					]
				},
				{
					label: "操作",
					type: "op",
					buttons: ["slot-add", "edit", "delete"]
				}
			]
		});

		// crud 加载
		function onLoad({ ctx, app }: CrudLoad) {
			// 绑定 service
			ctx.service(service.material.category).done();
			app.refresh();
		}

		return {
			refs,
			setRefs,
			upsert,
			table,
			onLoad
		};
	}
});
</script>
