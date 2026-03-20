<script setup>
import {ref} from "vue";

import Divider from 'primevue/divider';
import OrderList from 'primevue/orderlist';
import Button from 'primevue/button';
import InputText from "primevue/inputtext";
import Select from 'primevue/select';

let taskToEdit = ref(null);
let requirementToEdit = ref(null);

let currentId = ref(0);
let currentRequirementId = ref(0);

let activeTasks = ref([]);

let taskTypes = ref([
  { name: "Normal" },
  { name: "Daily" }
]);

function removeTask(task) {
  activeTasks.value = activeTasks.value.filter(t => t.id !== task.id)
}

function removeRequirement(requirement) {
  taskToEdit.value.requirements = taskToEdit.value.requirements.filter(t => t.id !== requirement.id)
}

function addTask() {
  activeTasks.value.push(
      {
        id: currentId.value,
        task: "New Task",
        type: "Normal",
        date: "3/20",
        requirements: []
      }
  );
  currentId.value++;
  taskToEdit.value = activeTasks.value[activeTasks.value.length - 1];
}

function addRequirement() {
  taskToEdit.value.requirements.push(
      {
        id: currentRequirementId.value,
        name: "New Requirement",
        complete: false
      }
  )
  currentRequirementId.value++;
  requirementToEdit.value = taskToEdit.value.requirements[taskToEdit.value.requirements.length - 1];
}

function clearEdit() {
  taskToEdit.value = null;
}

function clearRequirement() {
  requirementToEdit.value = null;
}

</script>

<template>
  <div class="app">
    <main>
      Task Tracker
      <div v-if="requirementToEdit != null">
        <div style="display: flex; flex-direction: row; align-items: center; justify-content: center;">
          <Button label="Back" @click="clearRequirement"/>
          &nbsp;
          <h2>Edit Requirement</h2>
        </div>
        <div style="display: flex; flex-direction: column; align-items: center; justify-content: center;">
          Requirement Name: <InputText type="Requirement Name" v-model="requirementToEdit.name" />
        </div>
      </div>
      <div v-if="requirementToEdit == null">
        <div v-if="taskToEdit != null">
          <div style="display: flex; flex-direction: row; align-items: center; justify-content: center;">
            <Button label="Back" @click="clearEdit"/>
            &nbsp;
            <h2>Edit Task</h2>
          </div>
          <div style="display: flex; flex-direction: column; align-items: center; justify-content: center;">
            Task Name: <InputText type="Task Name" v-model="taskToEdit.task" />
            Task Type: <Select v-model="taskToEdit.type" :options="taskTypes" optionLabel="name" placeholder="Select a Type" class="w-full md:w-80" />
            Due Date: <InputText type="Due Date" v-model="taskToEdit.date" />
          </div>
          <OrderList v-model="taskToEdit.requirements" dataKey="name" breakpoint="575px" scrollHeight="575px">
            <template #option="{ option }">
              <span v-if="!option.complete">{{ option.name }}</span>
              <s v-if="option.complete">{{ option.name }}</s>
              <img @click="option.complete = !option.complete; $forceUpdate();" :src="option.complete ? '/x.svg' : '/check.svg'" alt="Edit Requirement" class="complete_img">
              <img @click="requirementToEdit = option; $forceUpdate();" src="/pencil.svg" alt="Edit Requirement" class="pencil_img">
              <img @click="removeRequirement(option); $forceUpdate();" src="/trash_can.svg" alt="Delete Task" class="trash_can_img">
            </template>
          </OrderList>
          <Divider/>
          <Button>
            <span @click="addRequirement(); $forceUpdate();" class="add_task_text">Add Requirement</span>
          </Button>
        </div>
        <div v-if="taskToEdit == null">
          <h2>Tasks</h2>
          <Divider/>
          <OrderList v-model="activeTasks" dataKey="task" breakpoint="575px" scrollHeight="575px">
            <template #option="{ option }">
              {{ option.task }}
              <img @click="taskToEdit = option; $forceUpdate();" src="/pencil.svg" alt="Edit Task" class="pencil_img">
              <img @click="removeTask(option); $forceUpdate();" src="/trash_can.svg" alt="Delete Task" class="trash_can_img">
            </template>
          </OrderList>
          <Divider/>
          <Button>
            <span @click="addTask(); $forceUpdate();" class="add_task_text">Add Task</span>
          </Button>
        </div>
      </div>
    </main>
  </div>
</template>

<style scoped>
.app {
  display: inline-block;
  overflow: hidden;
  border: solid 1px #000000;
  border-radius: 16px;
  width: 400px;
  height: 800px;
}
.complete_img {
  position: absolute;
  right: 60px;
  width: 25px;
  height: 25px;
}
.pencil_img {
  position: absolute;
  right: 30px;
  width: 25px;
  height: 25px;
}
.trash_can_img {
  position: absolute;
  right: 0;
  width: 25px;
  height: 25px;
}
.add_task_text {
  font-size: 30px;
}
</style>