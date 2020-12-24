<template>
  <form class="container" ref="form">
    <transition
      enter-class="opacity-0"
      enter-active-class=""
      enter-to-class="opacity-100"
      leave-class="opacity-100"
      leave-active-class=""
      leave-to-class="opacity-0"
    >
      <div
        v-if="loading"
        class="absolute z-10 bg-blue-800 text-white p-4 rounded bg-opacity-75"
        style="bottom: 25px"
      >
        Saving cookie...
      </div>
    </transition>
    <div class="flex flex-col px-2">
      <div class="py-2">
        <label class="block font-medium" for="name">Name*</label>
        <input
          required
          v-model="name"
          ref="name"
          class="w-full rounded p-2 border-2 outline-none focus:border-blue-600 transition-all duration-100 ease-in-out"
          id="name"
          name="name"
          placeholder="Name..."
          type="text"
        />
      </div>
      <div class="py-2">
        <label class="block font-medium" for="description">Description</label>
        <textarea
          v-model="description"
          ref="description"
          name="description"
          class="w-full rounded p-2 border-2 outline-none focus:border-blue-600 transition-all duration-100 ease-in-out"
          id="description"
          placeholder="Description..."
          type="text"
        />
      </div>
      <div class="py-2">
        <label class="block font-medium" for="image">Image*</label>
        <input
          required
          class="w-full rounded p-2 border-2 outline-none focus:border-blue-600 transition-all duration-100 ease-in-out"
          name="picture"
          ref="file"
          id="image"
          type="file"
          accept="image/*"
        />
      </div>
      <div class="py-4">
        <button
          @click.prevent="submit"
          class="font-medium tracking-wide bg-blue-700 text-white px-3 py-2 rounded"
        >
          SUBMIT COOKIE
        </button>
      </div>
      <transition
        enter-class="opacity-0"
        enter-active-class=""
        enter-to-class="opacity-100"
        leave-class="opacity-100"
        leave-active-class=""
        leave-to-class="opacity-0"
      >
        <div
          v-if="showErrors"
          class="py-2 px-2 rounded bg-red-200 transition-all duration-100 ease-in-out"
        >
          <ul>
            <li class="text-red-800 py-1" v-for="error in errors" :key="error">
              - {{ error }}
            </li>
          </ul>
        </div>
      </transition>
    </div>
  </form>
</template>

<script>
export default {
  data() {
    return {
      name: '',
      description: '',
      showErrors: false,
      errors: [],
      loading: false,
    }
  },
  methods: {
    validate() {
      this.errors = []
      let errors = false
      if (this.$refs.file.files.length != 1) {
        this.errors.push('An image of your cookie is required')
        this.showErrors = true
        errors = true
      }
      if (!this.name) {
        this.errors.push('A name is required')
        this.showErrors = true
        errors = true
      }
      return errors
    },
    async submit() {
      if (this.validate()) {
        return
      }
      this.loading = true
      this.showErrors = false
      const formData = new FormData()
      const data = {}
      this.name && (data.name = this.name)
      this.description && (data.description = this.description)
      const file = this.$refs.file.files[0]
      formData.append('files.picture', file, file.name)
      formData.append('data', JSON.stringify(data))
      const cookie = await this.$http.post(
        'https://strapi.kaleberc.com/cookies',
        formData
      )
      this.loading = false
      this.$router.push('/')
    },
  },
}
</script>

<style scoped>
.container {
  @apply min-h-screen flex flex-col mx-auto px-2;
}
</style>
