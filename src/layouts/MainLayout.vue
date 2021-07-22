<template>
  <q-layout view="hHh lpR fFf">

    <q-header elevated :class="headerStyle">
      <q-toolbar>
        <router-link to="/">
          <q-toolbar-title class="">
              <img v-if="!$q.dark.isActive" class="q-pt-sm q-ml-lg" src="../../public/Sideway.svg">
              <img v-else class="q-pt-sm q-ml-lg" src="../../public/GreenWhite.svg">
          </q-toolbar-title >
        </router-link>
        <q-space/>
        <q-btn flat round :color="iconColor" class="q-mr-md" :icon="darkIcon" @click="toggleDarkState" />
        <!-- <p class="q-mr-xl q-my-auto"> {{ username }} | Signout</p> -->
        <q-btn flat round :color="iconColor" class="q-mr-md" :icon="soundIcon" @click="toggleSoundState" />
        <q-btn flat round :color="iconColor" class="q-mr-md" icon="person" @click="togglePersonState" >
          <q-menu anchor="bottom left" self="top right" class="q-mt-lg">
            <div class="row q-ma-lg">
              <div class="column">
                <div v-for="(person,index) in people" class="q-mt-sm" :key="index" >
                  <div class="row align-center ">
                    <div class="q-mr-lg">
                      <p class="person--name text-h6 q-mb-none">{{person.name}}</p>
                      <p class="person--location text-subtitle2 q-mb-none text-grey-13">{{ person.location }}</p>
                    </div>
                    <q-space/>
                    <p class="person--time q-mb-none text-h6 text-grey-13"> {{ String((new Date().getUTCHours()  + person.offset) % 24).padStart(2,'0') }}:{{ String(new Date().getUTCMinutes()).padStart(2,'0') }}</p>
                    <q-icon :name="workIcon(person.offset)" size="20px" class="q-mt-xs q-ml-sm" color="grey-13 "/>
                  </div>
                </div>
              </div>
              <q-separator vertical class="q-mx-md"/>
              <div class="column">
                <div v-for="(person,index) in people2" class="q-mt-sm" :key="index" >
                  <div class="row align-center ">
                    <div class="q-mr-lg">
                      <p class="person--name text-h6 q-mb-none">{{person.name}}</p>
                      <p class="person--location text-subtitle2 q-mb-none text-grey-13">{{ person.location }}</p>
                    </div>
                    <q-space/>
                    <p class="person--time q-mb-none text-h6 text-grey-13">  {{ String((new Date().getUTCHours()  + person.offset) % 24).padStart(2,'0') }}:{{ String(new Date().getUTCMinutes()).padStart(2,'0') }}</p>
                    <q-icon :name="workIcon(person.offset)" size="20px" class="q-mt-xs q-ml-sm" color="grey-13 "/>
                  </div>
                </div>
              </div>
            </div>
          </q-menu>
        </q-btn>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view  :soundStatus="soundState"/>
    </q-page-container>

  </q-layout>
</template>
<script>

import { defineComponent, ref, computed } from 'vue'
import { useQuasar } from 'quasar'

const people = [
  {
    name: 'Alan',
    location: 'Mexico',
    offset: -5
  },
  {
    name: 'Ana',
    location: 'Zurich',
    offset: 2
  },
  {
    name: 'Celine',
    location: 'Zurich',
    offset: 2
  },
  {
    name: 'Dan',
    location: 'Vancouver',
    offset: -7
  },
  {
    name: 'Daniel',
    location: 'Zurich',
    offset: 2
  },
  {
    name: 'Diane',
    location: 'Zurich',
    offset: 2
  },
  {
    name: 'Gabriela',
    location: 'Calgary',
    offset: -6
  }
]

const people2 = [
  {
    name: 'Hannah',
    location: 'Zurich',
    offset: 2
  },
  {
    name: 'Kiko',
    location: 'Zurich',
    offset: 2
  },
  {
    name: 'Lotta',
    location: 'Zurich',
    offset: 2
  },
  {
    name: 'Miranda',
    location: 'Zurich',
    offset: 2
  },
  {
    name: 'Pari',
    location: 'Zurich',
    offset: 2
  },
  {
    name: 'Rafael',
    location: 'São Paolo',
    offset: -3
  }
]

export default defineComponent({
  name: 'MainLayout',
  setup () {
    const $q = useQuasar()
    function toggleDarkState () {
      $q.dark.toggle()
    }
    const soundState = ref(false)
    function toggleSoundState () {
      soundState.value = !soundState.value
    }
    const soundIcon = computed(() => {
      return soundState.value ? 'volume_up' : 'volume_off'
    })

    const personState = ref(false)
    function togglePersonState () {
      personState.value = !personState.value
    }
    const leftDrawerOpen = ref(false)

    const workIcon = (timeOffset) => {
      const hour = (new Date().getUTCHours() + timeOffset) % 24

      return hour < 8 || hour > 17 ? 'night_shelter' : 'work'
    }

    return {
      toggleDarkState,
      toggleLeftDrawer () {
        leftDrawerOpen.value = !leftDrawerOpen.value
      },
      toggleSoundState,
      soundIcon,
      soundState,
      togglePersonState,
      people,
      people2,
      workIcon
    }
  },
  computed: {
    darkIcon: () => {
      const $q = useQuasar()
      // console.log($q.dark.isActive)
      return $q.dark.isActive ? 'dark_mode' : 'light_mode'
    },
    headerStyle: () => {
      const $q = useQuasar()
      return $q.dark.isActive ? 'bg-grey-10 text-white' : 'bg-grey-1 text-black'
    },
    iconColor: () => {
      const $q = useQuasar()
      // console.log($q.dark.isActive)
      return $q.dark.isActive ? 'secondary' : 'primary'
    }
  }
})
</script>

<style>

.person--location{
  margin-top: -5px;
}

</style>
