<template>
  <div class="wrapper">

    <div class="wrapper-content">
      
      <section>
        <div class="container">

          <message v-if="message" :message="message"/>

          <newNote :note="note" :priorities="priorities" @addNote="addNote"/>

          <div class="note-header" style="margin: 36px 0;">
            <h1> {{ title }} </h1>

            <search :search="search"
            placeholder="Find a note"
            @search="search = $event"/>

            <div class="icons">
              <svg :class="{ active: grid }" @click="grid = true" style="cursor: pointer;" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" ><rect x="3" y="3" width="7" height="7"></rect><rect x="14" y="3" width="7" height="7"></rect><rect x="14" y="14" width="7" height="7"></rect><rect x="3" y="14" width="7" height="7"></rect></svg>
              <svg :class="{ active: !grid }" @click="grid = false" style="cursor: pointer;" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="8" y1="6" x2="21" y2="6"></line><line x1="8" y1="12" x2="21" y2="12"></line><line x1="8" y1="18" x2="21" y2="18"></line><line x1="3" y1="6" x2="3" y2="6"></line><line x1="3" y1="12" x2="3" y2="12"></line><line x1="3" y1="18" x2="3" y2="18"></line></svg>
            </div>
          </div>

          <notes :notes="notesFilter" :grid="grid" :isPriorityHigh="isPriorityHigh" :isPriorityVeryHigh="isPriorityVeryHigh" @remove="removeNote"/>

        </div>
      </section>

    </div>

  </div>
</template>

<script>
import message from '@/components/Message.vue'
import newNote from '@/components/NewNote.vue'
import notes from '@/components/Notes.vue'
import search from '@/components/Search.vue'
export default {
  components: {
    message, newNote, notes, search
  },
  data () {
    return {
      title: 'Notes App',
      search: '',
      message: null,
      grid: true,
      note: {
        	title: '',
          description: '',
          priority: 'Standard',
          isPriorityHigh: Boolean(this.priority === 'High'),
          isPriorityVeryHigh: Boolean(this.priority === 'Very High'),
      },
      notes: [
          {
              title: 'The First Note',
              description: 'The First Note Description',
              priority: 'High',
              date: new Date(Date.now()).toLocaleString()
          },
          {
              title: 'The Second Note',
              description: 'The Second Note Description',
              priority: 'Very High',
              date: new Date(Date.now()).toLocaleString()
          },
          {
              title: 'The Third Note',
              description: 'The Third Note Description',
              priority: 'Standard',
              date: new Date(Date.now()).toLocaleString()
          }
      ],
      priorities: ['Standard', 'High', 'Very High']
    }
  },
  computed: {
    notesFilter() {
      let array = this.notes,
          search = this.search
      if (!search) return array
      search = search.trim().toLowerCase()

      // filter
      array = array.filter(function (item) {
        if (item.title.toLowerCase().indexOf(search) !== -1) {
          return item
        }
      })
      
      // Error
      return array
    }
  },
  methods: {
     addNote() {
       let {title, description, priority} = this.note
       if (title === '') {
           this.message = 'Input a title'
           return false
       }
       if (priority === '') {
           this.message = 'Choose a priority'
           return false
       }
       this.notes.push({
           title,
           description,
           priority,
           date: new Date(Date.now()).toLocaleString(),
       })
       this.note.title = ''
       this.note.description = ''
       this.message = null
       this.priority = ''
    },
    removeNote(index) {
      this.notes.splice(index, 1)
    }
  }
}
</script>

<style>
</style>
