<template>
  <div class="mt-16">
    <template v-if="error.statusCode === 404">
      <div class="container m-auto">
        <div class="align-center justify-center">
          <div class="mr-3">
            <img src="@/static/error/404.svg" alt="">
          </div>
          <div class="text-center">
            <h1>404</h1>
            <h2 class="my-3">Página não encontrada</h2>
            <div>
              <el-button type="primary" @click="goHome">Voltar ao inicío</el-button>
            </div>
          </div>
        </div>
      </div>
    </template>
    <template v-if="error.statusCode === 403">
      <div class="container mx-auto">
        <div class="align-center justify-center">
          <div class="mr-3">
            <img src="@/static/error/403.svg" alt="">
          </div>
          <div class="text-center">
            <h1>403</h1>
            <h2 class="my-3">Acesso negado.</h2>
            <div>
              <el-button type="primary" @click="goHome">Voltar ao inicío</el-button>
            </div>
          </div>
        </div>
      </div>
    </template>
    <template v-if="error.statusCode === 500">
      <div class="container mx-auto">
        <div class="align-center justify-center" row>
          <div class="mr-3">
            <img src="@/static/error/500.svg" alt="">
          </div>
          <div class="text-center">
            <h1>500</h1>
            <h2 class="my-3">Estamos com problemas.</h2>
            <div>
              <el-button type="primary" @click="goHome">Voltar ao inicío</el-button>
            </div>
          </div>
        </div>
      </div>
    </template>
  </div>
</template>

<script>
export default {
  layout: 'empty',
  props: ['error'],
  methods: {
    goHome() {
      this.$router.push({ path: '/' });
    }
  },
}
</script>

<style scoped lang="css">
h1 {
  font-size: 150px;
  line-height: 150px;
  font-weight: 700;
  color: #252932;
  text-shadow: rgba(61, 61, 61, 0.3) 1px 1px, rgba(61, 61, 61, 0.2) 2px 2px, rgba(61, 61, 61, 0.3) 3px 3px;
}
</style>
