<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-grey-3">
      <q-input class="col" bottom-slots square v-model="newTaskText" label="Add task" counter dense @keyup.enter="addTask">
        <template v-slot:before>
          <q-icon name="event" />
        </template>

        <template v-slot:hint>
          New task text
        </template>

        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask" :disable="!newTaskText.length" />
        </template>
      </q-input>
    </div>

    <q-list separator border>
      <q-item @click="task.done = !task.done"
              clickable
              :class="{'done bg-green-1': task.done}"
              v-for="(task, index) in tasks"
              :key="task.id"
              v-ripple
      >
        <q-item-section avatar top>
          <q-checkbox class="no-pointer-events" v-model="task.done" val="cyan" color="cyan" />
        </q-item-section>

        <q-item-section>
          <q-item-label v-if="false">{{  task.title }}</q-item-label>
          <q-item-label caption>{{ task.content }}</q-item-label>
        </q-item-section>

        <q-item-section v-if="task.done" side>
          <q-btn square color="deep-orange" icon="delete" @click.stop="deleteTask(index)" />
        </q-item-section>
      </q-item>
    </q-list>

    <div class="q-pa-md q-gutter-sm absolute-center" v-if="!tasks.length">
      <q-banner inline-actions rounded class="bg-grey-10 text-white text-center">
        <h2>
          <q-icon name="check" size="100px" color="white" />
          
          No Tasks!</h2>
      </q-banner>
    </div>

  </q-page>
</template>

<script>
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'IndexPage',

  data() {
    return {
      tasks : [
        // {
        //   id      : 't1',
        //   title   : 'Title 1',
        //   content : 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. ',
        //   done    : false,
        //   date    : Date.now(),
        // },
        // {
        //   id      : 't2',
        //   title   : 'Title 2',
        //   content : 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. ',
        //   done    : true,
        //   date    : Date.now(),
        // },
        // {
        //   id      : 't3',
        //   title   : 'Title 3',
        //   content : 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. ',
        //   done    : false,
        //   date    : Date.now(),
        // },
        // {
        //   id      : 't4',
        //   title   : 'Title 4',
        //   content : 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. ',
        //   done    : true,
        //   date    : Date.now(),
        // },
      ],
      newTaskText: '',
    }
  },

  methods: {
    addTask: function () {
      // if (!this.newTaskText.length) {
      //   return
      // }

      this.tasks.unshift({
        id      : 't1',
        title   : 'Title 1',
        content : this.newTaskText,
        done    : false,
        date    : Date.now(),
      });
      this.newTaskText = '';
    },

    deleteTask: function (index) {
      this.$q.dialog({
        dark        : true,
        title       : 'Удаление',
        message     : 'Would you like to delete task?',
        cancel      : true,
        persistent  : true
      }).onOk(() => {
        this.tasks.splice(index, 1);
        this.showNotif('Task deleted');
      })
    },

    showNotif (msg) {
      this.$q.notify({
        message: msg,
        icon: 'announcement'
      })
    }
  }
})
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: green;
  }
}
</style>
