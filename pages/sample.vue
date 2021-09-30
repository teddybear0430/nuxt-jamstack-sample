<template>
  <div class="post">
    <h1>{{ data.title }}</h1>
    <time :datetime="data.createdAt">{{ data.createdAt }}</time>
    <div class="container" v-html="data.body"></div>
  </div>
</template>

<script lang="ts">
import {
  defineComponent,
  ref,
  useContext,
  useFetch,
} from '@nuxtjs/composition-api'

export default defineComponent({
  setup() {
    // 取得してきたコンテンツを格納するためのステート
    const data = ref({})

    // @nuxt/httpモジュールを呼び出す
    const { $http } = useContext()

    const args = {
      headers: {
        'X-API-KEY': process.env.API_KEY || '',
      },
    }

    // APIをコールしてデータの取得処理を行う
    useFetch(async () => {
      const result: {} = await $http.$get(
        `${process.env.ENDPOINT}/blogs/wp-query-get-posts-difference`,
        args
      )

      console.log(result)

      data.value = result
    })

    return { data }
  },
})
</script>

<style>
h1 {
  font-size: 32px;
}

img {
  width: 100%;
  max-width: 620px;
  height: auto;
}

.post {
  width: 100%;
  max-width: 820px;
  margin: 30px auto 50px;
}

.container img {
  width: 100%;
  height: auto;
  margin: 30px 0px;
  box-shadow: rgb(0 0 0 / 20%) 0px 0px 10px;
}
</style>
