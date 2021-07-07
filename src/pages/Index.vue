<template>
  <div class="container flex flex-center ">
    <div class="text-h1">
      {{status.label}}
    </div>
    <q-circular-progress
      :value="percentage"
      size="75vh"
      color="primary"
      class="q-pt-xl q-ml-xl"
      label="percentage"
      show-value
    >
    <p v-if="!loading" class="text-h3">
      {{ status.totalTime - minutes + status.baseTime }} : {{ String(59 - seconds).padStart(2,'0') }}
    </p>
    </q-circular-progress>
   <!-- {{ currentTime }} -->
  </div>
</template>

<script>
import { defineComponent, ref, computed } from 'vue'

export default defineComponent({
  name: 'PageIndex',

  setup () {
    let today = new Date()
    const minutes = ref(0)
    const seconds = ref(0)
    const loading = ref(true)
    const status = computed(() => {
      //
      let data = {}
      if (minutes.value < 45) {
        data = {
          label: 'In focus mode',
          color: 'primary',
          totalTime: 45,
          baseTime: 0
        }
      } else {
        data = {
          label: 'On a break!',
          color: 'secondary',
          totalTime: 15,
          baseTime: 45
        }
      }
      return data
    })

    function getNow () {
      // console.log('get time', percentage.value)
      loading.value = false
      today = new Date()
      minutes.value = today.getMinutes()
      seconds.value = today.getSeconds()
    }

    const percentage = computed(() =>
      minutes.value < 45 ? minutes.value / 45 * 100 : minutes.value / 15 * 100
    )

    setInterval(getNow, 1000)

    return {
      minutes,
      seconds,
      percentage,
      status,
      loading
    }
  }
})
</script>
