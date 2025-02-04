<template>
  <v-app>
    <v-app-bar :elevation="3">
      <template v-slot>
        <v-app-bar-title style="flex: auto">
          <RouterLink to="/">
            <v-icon icon="mdi-alpha-w-box" />
            Wordle
          </RouterLink>
        </v-app-bar-title>
        <v-spacer />

        <v-btn icon="mdi-brightness-7" @click="switchTheme" />

        <ActiveUser />

        <v-menu>
          <template v-slot:activator="{ props }">
            <v-btn icon="mdi-menu" v-bind="props" />
          </template>

          <v-list>
            <v-list-item>
              <v-list-item-title>
                <RouterLink to="/wordle">Play Wordle</RouterLink>
              </v-list-item-title>
            </v-list-item>
            <v-list-item>
              <v-list-item-title>
                <RouterLink to="/wordoftheday">Daily Wordle</RouterLink>
              </v-list-item-title>
            </v-list-item>
            <v-list-item>
              <v-list-item-title>
                <RouterLink to="/last-ten-words">Last 10 Words</RouterLink>
              </v-list-item-title>
            </v-list-item>
            <v-list-item>
              <v-list-item-title>
                <RouterLink to="/leaderboard">Leaderboard</RouterLink>
              </v-list-item-title>
            </v-list-item>
            <v-list-item>
              <v-list-item-title><RouterLink to="/about">About</RouterLink></v-list-item-title>
            </v-list-item>
            <v-list-item>
              <v-list-item-title
                ><RouterLink to="/instructions">Instructions</RouterLink></v-list-item-title
              >
            </v-list-item>
          </v-list>
        </v-menu>
      </template>
    </v-app-bar>

    <v-main class="background">
      <RouterView />
    </v-main>
  </v-app>
</template>

<script setup lang="ts">
import { useTheme } from 'vuetify/lib/framework.mjs'
import { reactive } from 'vue'
import { useDisplay } from 'vuetify'
import { provide } from 'vue'
import { PlayerService } from './scripts/playerService'
import { Services } from './scripts/services'
import ActiveUser from './components/ActiveUser.vue'

// Provide the useDisplay to other components so that it can be used in testing.
const display = reactive(useDisplay())
provide(Services.Display, display)
const playerService = new PlayerService()
playerService.setupPlayerAsync()
provide(Services.PlayerService, playerService)

const theme = useTheme()

function switchTheme() {
  if (theme.global.name.value === 'light') {
    setDarkTheme()
  } else {
    setLightTheme()
  }
}

function setLightTheme() {
  theme.global.name.value = 'light'
}

function setDarkTheme() {
  theme.global.name.value = 'dark'
}
</script>
