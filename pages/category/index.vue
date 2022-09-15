<template>
  <div class="shadow-lg rounded-2xl p-4 bg-white dark:bg-gray-700 w-full">
    <el-table v-loading="loading" :data="tableData">
      <el-table-column label="Nome" prop="name" />
      <el-table-column align="right">
        <template slot="header">
          <el-button size="mini" type="primary" @click="addNewEntity()">
            Adicionar
          </el-button>
        </template>
        <template slot-scope="scope">
          <el-button size="mini" @click="getEntity(scope.row)">Editar</el-button>
          <el-popconfirm title="Tem certeza de que deseja excluir este item?" confirm-button-text='OK'
            cancel-button-text='Cancelar' @confirm="handleDelete(scope.$index, scope.row)">
            <el-button slot="reference" size="mini" type="danger">
              Deletar
            </el-button>
          </el-popconfirm>
        </template>
      </el-table-column>
    </el-table>

    <el-dialog :title="`${form.id ? 'Editar' : 'Adicionar nova'} categoria`" :visible.sync="dialogFormVisible">
      <el-form ref="ruleForm" :model="form" :rules="rules">
        <el-form-item label="Nome da categoria" prop="name">
          <el-input v-model="form.name" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">Cancelar</el-button>
        <el-button type="primary" @click="handleConfirm()">Confirmar</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>

export default {
  data() {
    return {
      tableData: [],
      loading: false,
      dialogFormVisible: false,
      form: {
        name: '',
      },
      rules: {
        name: [
          { required: true, message: 'O campo nome é obrigatório!', trigger: 'blur' },
          { min: 3, max: 70, message: 'O nome deve ser de no mínimo 3 carácteres e no máximo 70', trigger: 'blur' }
        ],
      },
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

    getEntity(row) {
      this.form = { ...row };
      this.dialogFormVisible = true
    },

    addNewEntity() {
      this.resetForm();
      this.dialogFormVisible = true
    },

    handleConfirm() {
      if (this.form.id) {
        this.handleEdit();
      } else {
        this.handleCreate();
      }
    },

    handleCreate() {
      this.$refs.ruleForm.validate(async (valid) => {
        if (valid) {
          try {
            await this.$axios.post("/category/create", this.form);
            this.$notify.success({
              title: 'Sucesso',
              message: 'Categoria criada com sucesso!'
            });
            this.dialogFormVisible = false;
            this.fetchData();
          } catch (e) {
            this.$notify.error({
              title: 'Erro',
              message: 'Não foi possível criar a categoria'
            });
          }
        }
        else {
          this.$notify.error({
            title: 'Erro',
            message: 'Preencha os campos corretamente'
          });
        }
      });
    },

    handleEdit() {
      this.$refs.ruleForm.validate(async (valid) => {
        if (valid) {
          try {
            await this.$axios.patch(`/category/update/${this.form.id}`, this.form);
            this.$notify.success({
              title: 'Sucesso',
              message: 'Categoria atualizada com sucesso!'
            });
            this.resetForm();
            this.dialogFormVisible = false;
            this.fetchData();
          } catch (e) {
            this.$notify.error({
              title: 'Erro',
              message: 'Não foi possível atualizar'
            });
          }
        }
        else {
          this.$notify.error({
            title: 'Erro',
            message: 'Preencha os campos corretamente'
          });
        }
      });
    },

    async handleDelete(index, row) {
      try {
        await this.$axios.delete(`/category/delete/${row.id}`);
        this.$notify.success({
          title: 'Sucesso',
          message: 'Deletado com sucesso'
        });
        this.tableData.splice(index, 1);
      } catch (e) {
        this.$notify.error({
          title: 'Erro',
          message: 'Não foi possível deletar'
        });
      }
    },

    resetForm() {
      this.form = {
        name: '',
      }
    }
  },
}
</script>
