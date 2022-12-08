<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input 
        v-model="newTask"
        class="col" 
        square 
        filled 
        bg-color="white" 
        placeholder="Přidat úkol" 
        dense
        @keyup.enter="addTask"
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click.stop="addTask" />
        </template>
      </q-input>
    </div> 
    <q-list class="bg-white" separator bordered>
      <q-item 
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{ 'done bg-blue-1' : task.done }"
        clickable
        v-ripple>
        <q-item-section avatar>
          <q-checkbox v-model="task.done" color="primary" class="no-pointer-events" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{task.title}}</q-item-label>
        </q-item-section>
        <q-item-section
          v-if="task.done"
          side
        >
          <q-btn @click.stop="deleteTask(index)" flat round dense color="primary" icon="delete" />
        </q-item-section>
      </q-item>
    </q-list>

    <div 
      v-if="!tasks.length"
      class="no-tasks absolute-center column items-center">
      <q-icon
        name="check"
        size="100px"
        color="primary"
      ></q-icon>
      <div class="text-h5 text-primary text-center">
        Nemáte žádné úkoly
      </div>
    </div>
  </q-page>
</template>

<script>
//import { useQuasar } from 'quasar'
import { defineComponent } from 'vue'

export default defineComponent({
  /*setup () {
    const $q = useQuasar()
  },*/
  data() {
    return {
      newTask: '',
      tasks: [
        {
          title: "Get bananas",
          done: false
        },
        {
          title: "Eat bananas",
          done: true
        },
        {
          title: "Poo bananas",
          done: false
        }
      ]
    }
  },
  methods: {
    deleteTask(index){
      this.$q.dialog({
        title: 'Potvrzení',
        message: 'Opravdu odstranit?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index, 1)  
        this.$q.notify('Úkol smazán')
      })
    },
    addTask(){
      this.tasks = [...this.tasks, { title: this.newTask, done: false }]
      this.newTask = ''
    }
  }
})
</script>

<style lang="scss">
  .done {
    .q-item__label {
      text-decoration: line-through;
      color: #bbbbbb;
    }
  }

  .no-tasks {
    opacity: .5;
  }
</style>