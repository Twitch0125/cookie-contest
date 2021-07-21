<template>
  <div class="container">
    <section class="flex justify-center py-6 pb-12">
      <nuxt-link
        class="bg-blue-700 rounded px-3 py-2 text-white font-medium tracking-wide"
        to="/add-cookie"
      >
        ADD YOUR COOKIE
      </nuxt-link>
    </section>
    <section>
      <h1 class="font-bold text-xl">COOKIES</h1>
      <div class="grid lg:grid-cols-2">
        <Card
          class="m-2"
          v-for="cookie in cookies"
          :key="cookie.id"
          :cookie="cookie"
        />
      </div>
    </section>
  </div>
</template>

<script>
export default {
  async asyncData({ $http, $config }) {
    console.log({$config});
    const cookies = await $http.$get(`${$config.apiUrl}/cookies`)
    cookies.forEach(
      (cookie) =>
        (cookie.picture.url = `${$config.apiUrl}${cookie.picture.url}`)
    )
    return { cookies }
  },
}
</script>

<style>
.container {
  @apply min-h-screen flex flex-col mx-auto px-2;
}
.card {
  @screen lg {
    width: 600px;
  }
}
</style>
