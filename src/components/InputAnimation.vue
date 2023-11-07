<script>
export default {
  data() {
    return {
      text: '',
      maxLength: 10,
    }
  },
  methods: {
    inputChange() {
        const length = this.text.length
        if(length > this.maxLength) {
            this.text = this.text.substring(0, this.maxLength)
        }
    }
  }
}
</script>

<template>
  <div class="flex justify-center h-screen">
    <div class="mt-[40vh]">
      <div class="flex justify-between">
        <label for="username" class="block text-md font-medium text-gray-900">Username</label>
        <p class="text-sm font-medium text-slate-500">
          {{ this.text.length }}/{{ this.maxLength }}
        </p>
      </div>
      <div class="mt-2">
        <input
          type="text"
          id="username"
          v-model="text"
          @input="inputChange"
          class="border-2 border-gray-300 bg-white h-8 p-3 rounded-md shadow-sm w-64 focus:outline-none focus:w-72 focus:h-10 text-md tracking-wider"
        />
      </div>
    </div>
  </div>
</template>
