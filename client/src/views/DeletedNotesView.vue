<template>
  <v-container
    ><v-card
      ><v-card-title style="display: flex; justify-content: space-between"
        ><div>Notes:</div>
        <div style="display: flex; gap: 10">
          <v-text-field
            style="width: 200px"
            density="compact"
            variant="solo"
            label="search note..."
            append-inner-icon="mdi-magnify"
            single-line
            hide-details
          ></v-text-field
          ><v-btn to="/note-editor">Create Note</v-btn>
        </div></v-card-title
      ><v-list v-for="note in notes" :key="note.id"
        ><v-list-item rounded="xl" class="mx-auto" :to="'/note-editor/' + note.id">
          <div style="display: flex; justify-content: space-between; align-items: center">
            {{ note.title }}
            <div style="display: flex; align-items: center; gap: 20px">
              <div>Date Deleted: {{ note.deletedDate?.split('T', 1).toString() }}</div>
            </div>
          </div>
        </v-list-item></v-list
      ></v-card
    ></v-container
  >
</template>

<script setup lang="ts">
import Axios from 'axios'
import { ref } from 'vue'

const notes = ref<INote[]>([])

export interface INote {
  id?: number
  title: string
  content: string
  date: string
  deletedDate?: string
}

Axios.get('/Note/GetDeletedNotes').then((result) => {
  notes.value = result.data as INote[]
})
</script>
