<script>
import gsap from 'gsap'

export default {
  data() {
    return {
      text: '',
      maxLength: 10
    }
  },
  methods: {
    getTextWidth() {
      const testSpan = document.createElement('span')
      testSpan.style.visibility = 'hidden'
      testSpan.style.position = 'absolute'
      testSpan.style.whiteSpace = 'nowrap'
      testSpan.style.font = window.getComputedStyle(this.$refs.myInput).font
      testSpan.textContent = this.text
      document.body.appendChild(testSpan)

      const width = testSpan.offsetWidth
      document.body.removeChild(testSpan)

      return width
    },
    inputChange() {
      const inputText = this.$refs.myInput
      const textWidth = this.getTextWidth(inputText.value)

      gsap.to(document.querySelector('.checkmark'), {
        scale: 0,
        opacity: 0,
        duration: 0.3,
        ease: 'none'
      })
      if (this.text.length >= this.maxLength) {
        this.text = this.text.substring(0, this.maxLength)
        const timeline = gsap.timeline()
        timeline.to(document.querySelector('.cursor'), {
          width: '30px',
          left: 'calc(100% - 3.5rem)',
          background: '#fff',
          //background: '#cbd5e2',
          ease: 'none',
          rotate: 360,
          duration: 0.3,
          onUpdate: () => {
            document.querySelector('.progress').style.background = '#126612'
          }
        })
        timeline.to(document.querySelector('.checkmark'), {
          scale: 1,
          opacity: 1,
          duration: 0.3,
          ease: 'none'
        })
        timeline.to(document.querySelector('.progress'), {
          width: '0',
        })
      } else {
        gsap.to(document.querySelector('.cursor'), {
          width: '10px',
          left: textWidth + 15 + 'px',
          background: '#cbd5e2',
          ease: 'none',
          duration: 0.3,
          onUpdate: () => {
            document.querySelector('.progress').style.background = '#126612'
          }
        })
        gsap.to(document.querySelector('.progress'), {
          width: '10px'
        })
      }
      const progressValue = this.text.length * 10
      document.querySelector('.progress').style.height = progressValue + '%'
    }
  }
}
</script>

<template>
  <div class="flex justify-center h-screen bg-slate-100">
    <div class="mt-[40vh]">
      <div class="flex justify-between">
        <label for="username" class="block text-xl font-semibold text-gray-900 ml-2"
          >Username</label
        >
        <span class="text-sm font-medium text-slate-500 mr-2">
          {{ this.text.length }}/{{ this.maxLength }}
        </span>
      </div>
      <div class="mt-2 relative">
        <input
          type="text"
          id="username"
          v-model="text"
          ref="myInput"
          @input="inputChange"
          class="border-2 border-gray-300 bg-white h-12 rounded-2xl shadow-md w-72 focus:outline-none text-lg tracking-wide p-4 caret-transparent"
        />
        <div
          class="absolute flex justify-items-center items-center bg-[#cbd5e1] w-2 rounded-2xl translate-y-[-50%] top-0 ml-4 mt-6 cursor"
        >
          <div
            class="progress rounded-2xl absolute left-2/4 bottom-0 translate-x-[-50%] bg-[#126612]"
          ></div>
          <svg
            class="w-[30px] h-[30px] text-gray-800 scale-0 shrink-0 checkmark"
            aria-hidden="true"
            xmlns="http://www.w3.org/2000/svg"
            fill="currentColor"
            viewBox="0 0 20 20"
          >
            <path
              fill="rgb(18, 102, 18)"
              d="m18.774 8.245-.892-.893a1.5 1.5 0 0 1-.437-1.052V5.036a2.484 2.484 0 0 0-2.48-2.48H13.7a1.5 1.5 0 0 1-1.052-.438l-.893-.892a2.484 2.484 0 0 0-3.51 0l-.893.892a1.5 1.5 0 0 1-1.052.437H5.036a2.484 2.484 0 0 0-2.48 2.481V6.3a1.5 1.5 0 0 1-.438 1.052l-.892.893a2.484 2.484 0 0 0 0 3.51l.892.893a1.5 1.5 0 0 1 .437 1.052v1.264a2.484 2.484 0 0 0 2.481 2.481H6.3a1.5 1.5 0 0 1 1.052.437l.893.892a2.484 2.484 0 0 0 3.51 0l.893-.892a1.5 1.5 0 0 1 1.052-.437h1.264a2.484 2.484 0 0 0 2.481-2.48V13.7a1.5 1.5 0 0 1 .437-1.052l.892-.893a2.484 2.484 0 0 0 0-3.51Z"
            />
            <path
              fill="#fff"
              d="M8 13a1 1 0 0 1-.707-.293l-2-2a1 1 0 1 1 1.414-1.414l1.42 1.42 5.318-3.545a1 1 0 0 1 1.11 1.664l-6 4A1 1 0 0 1 8 13Z"
            />
          </svg>
        </div>
      </div>
    </div>
  </div>
</template>
