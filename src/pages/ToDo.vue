<template>
  <q-page class="bg-grey-1 column">
    <div class="row q-pa-sm bg-white">
      <q-input
        @keyup.enter="addTask"
        filled
        class="col"
        square
        bg-color="white"
        v-model="newTask"
        placeholder="add task"
        dense
      >
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>

    <q-list separator bordered>
      <q-item
        @click="task.done = !task.done"
        :class="{ 'done bg-purple-1': task.done }"
        clickable
        v-ripple
        v-for="(task, index) in tasks"
        :key="task.title"
      >
        <q-item-section avatar>
          <q-checkbox
            class="no-pointer-events"
            v-model="task.done"
            val="teal"
            color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            @click.stop="deleteTask(index)"
            flat
            round
            color="primary"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      tasks: [
        {
          title: "hello1",
          done: true,
        },
        {
          title: "hello2",
          done: false,
        },
        {
          title: "hello3",
          done: false,
        },
      ],
    };
  },
  methods: {
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Confirm",
          message: "Are you sure?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify("Task deleted!");
        });
    },

    addTask() {
      this.tasks.push({
        title: this.newTask,
        done: false,
      });
      this.newTask = "";
    },
  },
};
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: $primary;
  }
}
</style>
