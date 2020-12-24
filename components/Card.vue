<template>
  <div class="shadow-md rounded">
    <img
      class="rounded object-cover w-full"
      alt="picture of cookie"
      :src="cookie.picture.url"
    />
    <div class="px-2 py-1 text-lg font-medium text-blue-800">
      {{ cookie.name }}
    </div>
    <div class="px-2 py-1 text-gray-800">
      {{ cookie.description }}
    </div>
    <div class="px-2 py-1">
      <template v-if="cookie.scores">
        <h4 class="font-medium">Scoring</h4>
        <ul>
          <li class="py-1">Looks: {{ cookie.scores.looks }}</li>
          <li class="py-1">Originality: {{ cookie.scores.originality }}</li>
          <li class="py-1">Taste: {{ cookie.scores.taste }}</li>
          <li class="pt-2 font-medium">Total: {{ total }}</li>
        </ul>
      </template>
      <template v-else>
        <span class="font-medium"> waiting to be scored </span>
      </template>
    </div>
    <!-- <div class="px-2 py-1 flex">
      <div class="flex-grow"></div>
      <button class="tracking-wide font-medium text-blue-700 px-3 py-2 rounded">
        MORE
      </button>
    </div> -->
  </div>
</template>

<script>
export default {
  props: ['cookie'],
  computed: {
    total() {
      if (this.cookie.scores) {
        const {
          scores: { looks, originality, taste },
        } = this.cookie
        return looks + originality + taste
      }
    },
  },
}
</script>