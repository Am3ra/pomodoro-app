<template>
  <q-pa>

    <div class="container flex justify-evenly items-center q-mt-xl">
      <div class="text-h1 text-center">
        {{status.label}}
      </div>
      <q-circular-progress
        :value="percentage"
        size="75vh"
        :color="status.color"
        class=" "
        label="percentage"
        show-value
      >
      <p v-if="!loading" class="text-h3 q-ma-none">
        {{ status.totalTime - 1 - minutes + status.baseTime }} : {{ String(59 - seconds).padStart(2,'0') }}
      </p>
      </q-circular-progress>
    <!-- {{ currentTime }} -->
    </div>
  </q-pa>
</template>

<script>
import { defineComponent, ref, computed } from 'vue'

export default defineComponent({
  name: 'PageIndex',

  setup () {
    let today = new Date()
    const minutes = ref(-1)
    const seconds = ref(0)
    const loading = ref(true)
    const status = computed(() => {
      //
      let data = {}
      if (minutes.value < 25) {
        data = {
          label: 'In focus mode',
          color: 'primary',
          totalTime: 25,
          baseTime: 0
        }
      } else {
        data = {
          label: 'On a break!',
          color: 'negative',
          totalTime: 5,
          baseTime: 25
        }
      }
      return data
    })

    function getNow () {
      // console.log('get time', percentage.value)
      loading.value = false
      today = new Date()
      minutes.value = today.getMinutes() % 30
      seconds.value = today.getSeconds()

      // minutes.value += 1
      // console.log(minutes.value)
      if (minutes.value === 0 && seconds.value === 0) {
        // console.log(minutes.value)
        notifyMe('Time to work!')
      }
      if (minutes.value === 25 && seconds.value === 0) {
        // console.log(minutes.value)
        notifyMe('Time to relax!')
      }
    }

    Notification.requestPermission()

    function notifyMe (text) {
      if (!window.Notification) {
        console.log('Browser does not support notifications.')
      } else {
        // check if permission is already granted
        if (Notification.permission === 'granted') {
          // show notification here
          new Notification('Pomodoro App', {
            body: text
          }).show()
        } else {
          // request permission from user
          Notification.requestPermission().then(function (p) {
            if (p === 'granted') {
              // show notification here
              new Notification('Hi there!', {
                body: text
              }).show()
            } else {
              console.log('User blocked notifications.')
            }
          }).catch(function (err) {
            console.error(err)
          })
        }
      }
    }

    const percentage = computed(() =>
      minutes.value < 25 ? (minutes.value + (seconds.value / 60)) / 25 * 100 : 100 - (minutes.value - 25 + (seconds.value / 60)) / 5 * 100
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

<style lang="scss">

//$

.text-h1, p {
  font-family: 'Interstate';
}

.container{
  height:100%;
}

</style>
