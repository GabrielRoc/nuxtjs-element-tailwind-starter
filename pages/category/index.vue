<template>
  <div class="shadow-lg rounded-2xl p-4 bg-white dark:bg-gray-700 w-full">
    <el-table v-loading="loading" :data="tableData">
      <el-table-column label="Nome" prop="name" />
      <el-table-column align="right">
        <template slot-scope="scope">
          <el-button size="mini" @click="handleEdit(scope.$index, scope.row)">Editar</el-button>
          <el-button size="mini" type="danger" @click="handleDelete(scope.$index, scope.row)">Deletar</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>

export default {
  data() {
    return {
      tableData: [],
      loading: false
    }
  },

  mounted() {
    this.fetchData();
  },

  methods: {
    async fetchData() {
      this.loading = true;
      try {
        const { data } = await this.$axios.get("/category/get");
        this.tableData = data;
      } catch (e) {
        this.$notify.error({
          title: 'Erro',
          message: 'Não foi possível carregar'
        });
      } finally {
        this.loading = false;
      }
    },

    handleEdit(index, row) {

    },

    handleDelete(index, row) {

    }
  },
}
</script>
