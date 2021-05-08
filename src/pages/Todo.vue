<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
        <q-input
            v-model="newTask"
            @keyup.enter="addTask"
            class="col"
            square
            filled
            bg-color="white"
            placeholder="Add task"
            dense
        >
            <template v-slot:append>
            <q-btn
                @click="addTask"
                round
                dense 
                flat 
                icon="add" />
            </template>
        </q-input>
    </div>
    <q-list
        class="bg-white"
        separator
        bordered
    >
        <q-item
            v-for="(task, index) in tasks"
            :key="task.title"
            @click="task.done = !task.done"
            :class="{ 'done bg-blue-1' : task.done }"
            clickable
            v-ripple
        >
            <q-item-section avatar>
                <q-checkbox 
                    v-model="task.done"
                    class="no-pointer-events"
                    color="primary" 
                />
            </q-item-section>
            <q-item-section>
                <q-item-label>{{ task.title }}</q-item-label>
            </q-item-section>
            <q-item-section
                v-if="task.done"
                side
            >
            <q-btn
                @click.stop="deleteTask(index)"
                flat 
                round
                dense
                color="primary" 
                icon="delete"
            />
            </q-item-section>
        </q-item>
    </q-list>
    <div class="no-tasks absolute-center">
        <div 
            v-if="!tasks.length"
            class="text-h5 text-primary text-center">
            <q-icon
                name="check"
                size="50px"
                color="primary"
            />
            No tasks
        </div>
    </div>
  </q-page>
</template>

<script>
export default {
  data(){
      return{
          newTask: '',
          tasks: [
            //   {
            //       title: 'Get bananas',
            //       done: false
            //   },
            //   {
            //       title: 'Eat bananas',
            //       done: true
            //   },
            //   {
            //       title: 'Poo bananas',
            //       done: false
            //   }
          ]
      }
  },
    methods: {
      deleteTask(index){
        this.$q.dialog({
            title: 'Confirm',
            message: 'Would you like to delete the task?',
            cancel: true,
            persistent: true
        }).onOk(() => {
            this.tasks.splice(index, 1)
            this.$q.notify('Task Sucessfully deleted..')
        })
      },
      addTask(){
          this.tasks.push({
              title: this.newTask,
              done: false
          })
          this.$q.notify('Task Sucessfully added..')
          this.newTask = ''
      }
    }
}
</script>

<style lang="scss">
    .done{
        .q-item__label{
            text-decoration: line-through;
            color: #bbb;
        }
    }
    .no-tasks{
        opacity: 0.7;
    }
</style>
