<template></template>
  <div>
    <h1>分类列表</h1>
    <el-table
      :tree-props="{children: 'children', hasChildren: 'hasChildren'}"
      :data="list"
      style="width: 100%"
    >
      <el-table-column prop="name" label="名称"></el-table-column>
      <el-table-column fixed="right" label="操作" width="100">
        <template slot-scope="scope">
  <el-button type="text" size="small" @click="edit(scope.row._id)">编辑</el-button>
  <el-button type="text" size="small" @click="del(scope.row._id)">删除</el-button>
</template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  name: "categoryList",
  data() {
    return {
      list: []
    };
  },
  created() {
    this.getCategoryList();
  },
  methods: {
    //获取分类列表
    getCategoryList() {
      let url = "rest/category";
      this.$.get(url).then(res => {
        let { code, data } = res.data;
        if (code === 1) {
          this.list = data;
        }
      });
    },
    //编辑
    edit(id) {
      this.$router.push(`/category/edit/${id}`);
    },
    //删除
    del(id) {
      this.$confirm("确认删除该分类吗？", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      }).then(async () => {
        let url = `rest/category/${id}`;
        this.$.delete(url).then(res => {
          res.data.code === 1 && this.getCategoryList();
        });
      });
    }
  }
};
</script>