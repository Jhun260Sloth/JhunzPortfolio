<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'

const colorMode = useColorMode()
const isDark = computed({
  get () {
    return colorMode.value === 'dark'
  },
  set () {
    colorMode.preference = colorMode.value === 'dark' ? 'light' : 'dark'
  }
})

const quote = ref({ id: null, text: '', author: '' })
const ranpass = ref({ random_pass: '' })
const timer = ref(20)
const progress = ref(0)
const intervalId = ref(null)

const fetchQuote = async () => {
  try {
    const response = await fetch('https://dummyjson.com/quotes/random')
    const data = await response.json()
    quote.value = { id: data.id, text: data.quote, author: data.author }
    timer.value = 20
    progress.value = 0
  } catch (error) {
    console.error('Error fetching quote:', error)
  }
}

const githubUsername = ref('Jhun260Sloth')
const repoCount = ref(0)

const fetchRepoCount = async () => {
  try {
    const response = await fetch(`https://api.github.com/users/${githubUsername.value}`)
    const data = await response.json()
    repoCount.value = data.public_repos
  } catch (error) {
    console.error('Error fetching GitHub repo count:', error)
  }
}

const startTimer = () => {
  intervalId.value = setInterval(() => {
    if (timer.value > 0) {
      timer.value--
      progress.value = ((20 - timer.value) / 20) * 100
    } else {
      fetchQuote()
    }
  }, 1000)
}

onMounted(() => {
  fetchQuote()
  startTimer()
  fetchRepoCount()
})

onUnmounted(() => {
  if (intervalId.value) clearInterval(intervalId.value)
})

const circumference = 2 * Math.PI * 14 // 14 is the radius of our circle
const strokeDashoffset = computed(() => {
  return circumference * (1 - progress.value / 100)
})



</script>



<template>
  <div class="font-sans antialiased leading-normal tracking-wider bg-cover min-h-screen flex items-center" 
       :class="{ 'text-gray-900 bg-white': !isDark, 'text-gray-100 bg-gray-900': isDark }"
       style="background-image:url('https://source.unsplash.com/1L71sPT5XKc');">
    <header>
      <ClientOnly>
        <UButton
          :icon="isDark ? 'i-heroicons-moon-20-solid' : 'i-heroicons-sun-20-solid'"
          color="gray"
          variant="ghost"
          aria-label="Theme"
          @click="isDark = !isDark"
          class="absolute top-4 right-4"
        />
        <template #fallback>
          <div class="w-8 h-8" />
        </template>
      </ClientOnly>
    </header>

    <div class="max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 flex flex-col md:flex-row space-y-8 md:space-y-0 md:space-x-8">
      <!-- Profile Card (Left Side) -->
      <div id="profile" class="w-full md:w-1/2 rounded-lg shadow-2xl opacity-75"
     :class="{ 'bg-white': !isDark, 'bg-gray-800': isDark }">
  <div class="p-4 md:p-8">
    <div class="rounded-full shadow-xl mx-auto mb-4 h-32 w-32 bg-cover bg-center" 
         style="background-image: url('https://avatar.iran.liara.run/public/boy')"></div>
    
    <h1 class="text-3xl font-bold text-center">Jhunnel O. De Jesus</h1>
    <div class="mx-auto w-4/5 pt-3 border-b-2 border-green-500 opacity-25"></div>

    <p class="pt-4 text-base font-bold flex items-center justify-center">
      <svg class="h-4 fill-current text-green-700 pr-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
        <path d="M9 12H1v6a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-6h-8v2H9v-2zm0-1H0V5c0-1.1.9-2 2-2h4V2a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v1h4a2 2 0 0 1 2 2v6h-9V9H9v2zm3-8V2H8v1h4z"/>
      </svg> Student / Learner
    </p>
    
    <p class="pt-2 text-gray-600 text-xs lg:text-sm flex items-center justify-center">
  <span class="bg-gray-500 text-white mt-2 px-2 py-1 rounded-md text-xs">Github Repositories: {{ repoCount }}</span>
</p>




    <div class="mt-6 w-4/5 mx-auto">
  <ul class="space-y-4">
    <li>
      <a href="https://github.com/Jhun260Sloth" class="flex items-center px-4 py-2 rounded-md text-xs text-white bg-gray-900 hover:bg-gray-800">
        <svg class="h-6 w-6 mr-2" fill="currentColor" viewBox="0 0 24 24">
          <path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/>
        </svg>
        GitHub
      </a>
    </li>
    <li>
      <a href="https://web.facebook.com/jhunneldejesus/" class="flex items-center px-4 py-2 rounded-md text-xs text-white bg-blue-600 hover:bg-blue-700">
        <svg class="h-6 w-6 mr-2" fill="currentColor" viewBox="0 0 24 24">
          <path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/>
        </svg>
        Facebook
      </a>
    </li>
    <li>
      <a href="https://web.sketchub.in/u/Frost-J2" class="flex items-center px-4 py-2 rounded-md text-xs text-white bg-blue-400 hover:bg-blue-500">
        <svg class="h-6 w-6 mr-2" fill="currentColor" viewBox="0 0 24 24">
          <path d="M12 0C5.376 0 0 5.376 0 12s5.376 12 12 12 12-5.376 12-12S18.624 0 12 0zm5.568 8.16c-.18 1.896-.96 8.76-1.344 11.592-.168 1.272-.504 1.704-.816 1.752-.696.096-1.224-.456-1.896-.888-.96-.624-1.488-1.008-2.472-1.632-1.128-.72-.396-1.116.24-1.764.168-.168 3.072-2.808 3.12-3.048.007-.034.011-.067.011-.101 0-.084-.044-.162-.12-.204-.076-.042-.168-.032-.235.024-.936.744-2.688 1.8-4.056 2.712-.264.192-.569.321-.888.384-.912.168-1.736-.096-2.448-.432-.84-.408-1.296-.624-1.261-.96.024-.192.6-.432 1.152-.624 1.536-.516 3.131-.851 4.752-1.008 2.16-.216 2.424-.168 3.264 0 .696.144 1.632.72 1.944.96.024 0 .072.024.072.024l.024-.168z"/>
        </svg>
        Sketchub
      </a>
    </li>
  </ul>
</div>



  <!-- New Quote Container with Timer -->
  <div class="mt-8 p-4 rounded-lg shadow-md relative"
       :class="{ 'bg-gray-100': !isDark, 'bg-gray-700': isDark }">
    <div class="absolute top-2 right-2 w-8 h-8">
      <svg class="w-8 h-8 transform -rotate-90">
        <circle
          cx="16"
          cy="16"
          r="14"
          stroke-width="4"
          fill="transparent"
          :class="isDark ? 'text-green-500' : 'text-green-400'"
          class="stroke-current"
        />
        <circle
          cx="16"
          cy="16"
          r="14"
          stroke-width="4"
          fill="transparent"
          :class="isDark ? 'text-gray-600' : 'text-gray-200'"
          class="stroke-current transition-all duration-1000 ease-linear"
          :stroke-dasharray="circumference"
          :stroke-dashoffset="strokeDashoffset"
        />
      </svg>
      <div class="absolute inset-0 flex items-center justify-center">
        <span class="text-xs font-bold">{{ timer }}</span>
      </div>
    </div>
    <h3 class="text-lg font-semibold mb-2">Quote of the Day</h3>
    <p class="italic mb-2">"{{ quote.text }}"</p>
    <p class="text-sm text-right">- {{ quote.author }}</p>
    <!-- <p class="text-xs text-gray-500 mt-2">Quote ID: {{ quote.id }}</p> -->
  </div>

  </div>
      </div>
      
     <!-- Projects List (Right Side) -->
     <div class="w-full md:w-1/2 rounded-lg shadow-2xl opacity-75"
           :class="{ 'bg-white': !isDark, 'bg-gray-800': isDark }">
        <div class="p-4 md:p-8">
          <h2 class="text-2xl font-bold mb-6">Latest Projects</h2>
          <div class="space-y-6">
            <div class="border border-gray-200 rounded-lg overflow-hidden shadow-sm hover:shadow-md transition-shadow duration-300"
                 :class="{ 'border-gray-700': isDark }">
              <div class="p-4">
                <h3 class="font-bold text-lg mb-2">Project 1</h3>
                <p class="text-sm mb-3">Brief description of Project 1. Technologies used: HTML, CSS, JavaScript</p>
                <a href="#" class="text-blue-500 hover:text-blue-600 text-sm">Read more →</a>
              </div>
            </div>

            <div class="border border-gray-200 rounded-lg overflow-hidden shadow-sm hover:shadow-md transition-shadow duration-300"
                 :class="{ 'border-gray-700': isDark }">
              <div class="p-4">
                <h3 class="font-bold text-lg mb-2">Project 2</h3>
                <p class="text-sm mb-3">Brief description of Project 2. Technologies used: React, Node.js, MongoDB</p>
                <a href="#" class="text-blue-500 hover:text-blue-600 text-sm">Read more →</a>
              </div>
            </div>

            <div class="border border-gray-200 rounded-lg overflow-hidden shadow-sm hover:shadow-md transition-shadow duration-300"
                 :class="{ 'border-gray-700': isDark }">
              <div class="p-4">
                <h3 class="font-bold text-lg mb-2">Project 3</h3>
                <p class="text-sm mb-3">Brief description of Project 3. Technologies used: Vue.js, Firebase</p>
                <a href="#" class="text-blue-500 hover:text-blue-600 text-sm">Read more →</a>
              </div>
            </div>

            <div class="border border-gray-200 rounded-lg overflow-hidden shadow-sm hover:shadow-md transition-shadow duration-300"
                 :class="{ 'border-gray-700': isDark }">
              <div class="p-4">
                <h3 class="font-bold text-lg mb-2">Project 4</h3>
                <p class="text-sm mb-3">Brief description of Project 4. Technologies used: Python, Django, PostgreSQL</p>
                <a href="#" class="text-blue-500 hover:text-blue-600 text-sm">Read more →</a>
              </div>
            </div>
          </div>
        </div>
      </div>


      
    </div>
  </div>
</template>


