<template>
  <q-layout view="hHh lpR fFf">

    <q-header elevated :class="headerStyle">
      <q-toolbar>
        <router-link to="/">
          <q-toolbar-title class="">
              <img class="q-pt-sm q-ml-lg" src="../assets/Sideway.svg">
          </q-toolbar-title >
        </router-link>
        <q-space/>
        <q-btn flat round color="primary" class="q-mr-md" :icon="darkIcon" @click="toggleDarkState" />
        <!-- <p class="q-mr-xl q-my-auto"> {{ username }} | Signout</p> -->
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>

    <!-- <q-footer reveal elevated class="bg-grey-10 text-white">
      <q-toolbar class="mx-auto row justify-evenly" >
        <p>© 2020 Sparkademy AG</p>
        <p> Terms & Conditions</p>
        <p>Privacy Policy</p>
        <p>FAQ</p>
        <p>Contact</p>
        <q-icon icon="twitter"/>
      </q-toolbar>
    </q-footer> -->

  </q-layout>
</template>
<script>

import { defineComponent, ref } from 'vue'
import { useQuasar } from 'quasar'

export default defineComponent({
  name: 'MainLayout',
  setup () {
    const $q = useQuasar()
    function toggleDarkState () {
      $q.dark.toggle()
    }

    const leftDrawerOpen = ref(false)
    return {
      toggleDarkState,
      toggleLeftDrawer () {
        leftDrawerOpen.value = !leftDrawerOpen.value
      }
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
    }
  }
})
</script>
