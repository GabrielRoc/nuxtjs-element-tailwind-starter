<template>
  <div class="bg-gray-100 h-screen overflow-hidden relative lg:p-4">
    <div class="container m-auto">
      <div class="align-center justify-center">
        <div class="shadow-lg rounded-2xl p-4 bg-white dark:bg-gray-700 w-full">
          <h1>LOGIN</h1>
          <el-form ref="loginForm" label-position="right" label-width="100px" :model="login" :rules="rules">
            <el-form-item label="Email" prop="email">
              <el-input v-model="login.email" type="email" placeholder="Insira seu email" />
            </el-form-item>
            <el-form-item label="Senha" prop="password">
              <el-input v-model="login.password" placeholder="Insira sua senha" show-password />
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="submitForm()">Login</el-button>
            </el-form-item>
          </el-form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  layout: 'empty',
  data() {
    return {
      login: {
        email: '',
        password: ''
      },

      rules: {
        password: [
          { required: true, message: 'Campo obrigatório', trigger: 'blur' },
          { min: 8, message: 'Minímo de 8 caracteres', trigger: 'blur' }
        ],
        email: [
          { required: true, message: 'Campo obrigatório', trigger: 'blur' },
        ],
      }
    }
  },

  methods: {
    error() {
      this.$notify.error({
        title: 'Erro',
        message: 'Email ou senha inválidos'
      });
    },

    async doLogin() {
      try {
        const { data } = await this.$axios.post("/auth/login", this.login);
        localStorage.setItem("token", data);
        this.$router.push("/");
      } catch (e) {
        this.error();
      }
    },

    submitForm() {
      this.$refs.loginForm.validate((valid) => {
        if (valid) {
          this.doLogin();
        } else {
          this.error();
        }
      });
    },
  },
}
</script>

<style>

</style>
