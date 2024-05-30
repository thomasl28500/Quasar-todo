<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        @keyup.enter="addTask"
        class="col"
        filled
        square
        bg-color="white"
        v-model="newTask"
        placeholder="Ajouter une tâche"
        dense
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{ 'done bg-pink-1': task.done }"
        clickable=""
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
        <q-item-section v-if="task.done" side>
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
    <div
      v-if="!tasks.length"
      class="no-tasks absolute-center"
      style="text-align: center"
    >
      <q-icon name="check" size="100px" color="primary" />
      <div class="text-h5 text-primary text-center">Aucune Tâche</div>
    </div>
  </q-page>
</template>

<script setup>
defineOptions({
  name: "IndexPage",
});
</script>

<script>
export default {
  data() {
    return {
      newTask: "",
      tasks: [
        // {
        //   title: "Manger des croquettes",
        //   done: false,
        // },
        // {
        //   title: "Faire le beau",
        //   done: false,
        // },
        // {
        //   title: "Jouer à la baballe",
        //   done: true,
        // },
        // {
        //   title: "Se rendre au salon de Toilettage",
        //   done: false,
        // },
      ],
    };
  },
  methods: {
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Confirmer",
          message: "Voulez-vous vraiment supprimer ceci ?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify("Tâche supprimée");
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
    color: #bbb;
  }
}

.no-tasks {
  opacity: 0.5;
}
</style>
