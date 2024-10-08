<template>
  <div class="container py-3 ">
    <div class="row ">
      <div class="col-sm-12 ">
        <h2 class="text-success text-center fw-bold ">Tasklist</h2>
        <hr class="border-success border-2">
      </div>
      <div class="col-sm-6 mt-3 ">
        <div class=" p-4">
          <div class="card mx-auto border-2 border-dark rounded-4 shadow-lg pb-3" style="width:300px; ">
            <img src="../../public/todo.jpg" height="150" class="card-img-top rounded-4 border" alt="Basic Task list">
            <div class="card-body ">
              <h5 class="card-title text-dark">Basic version</h5>
              <p class="card-text">Features</p>
              <ul>
                <li>Create, Delete Task</li>
              </ul>
              <hr class="my-4">
              <input type="text" class="form-control border-success" v-model.trim="taskName" @keyup.enter="runTask"
                placeholder="Enter your task">
              <button class="btn btn-success form-control mt-2 btn-sm " type="submit" @click="runTask">Add to Task
                list</button>
            </div>
            <ul v-if="arraylist.length > 0"
              class="list-group list-group-flush  mt-3 border-bottom border-top border-dark  customGreen  ">
              <h6 class="px-3 py-2 mb-0 fw-bold bg-success-subtle text-success-emphasis"><span
                  class="badge text-bg-warning">{{ arraylist.length }}</span>
                Pending Task
              </h6>
              <li v-for="(data, index) in arraylist" :key="index"
                class="list-group-item d-flex justify-content-between ">
                <div>
                  <span class="">{{ data.taskName }}</span>
                </div>
                <div>
                  <button class="btn btn-sm btn-danger custButton" @click="deleteTask(index)"><i
                      class="bi bi-trash"></i></button>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>

      <div class="col-sm-6 mt-3 ">
        <div class=" p-4">
          <div class="card mx-auto border-2 border-dark rounded-4 shadow-lg pb-3" style="width:300px;">
            <img src="../../public/todo1.jpg" height="150" class="card-img-top rounded-4 border"
              alt="Advance Task List">
            <div class="card-body ">
              <h5 class="card-title text-dark">Advance Version</h5>
              <p class="card-text">Features</p>
              <ul>
                <li>Create, Delete Task</li>
                <li>Edit Task</li>
                <li>Time stamp</li>
                <li>Undo Deleted Task</li>
                <li>Completed Task List</li>
              </ul>
              <hr class="my-4 border border-dark">
              <input type="text" class="form-control border-success" v-model.trim="newtaskName"
                @keyup.enter="advrunTask" placeholder="Enter your task">
              <button class="btn btn-success form-control mt-2 btn-sm " type="submit" @click="advrunTask">Add to
                Task list</button>
            </div>
            <ul v-if="newarraylist.length > 0"
              class="list-group list-group-flush  border-bottom border-top border-dark mt-3  customGreen  ">
              <h6 class="px-3 py-2 mb-0 fw-bold bold bg-success-subtle text-success-emphasis"><span
                  class="badge text-bg-warning">{{
                    newarraylist.length }}</span>
                Pending
                Task
              </h6>
              <li v-for="(data, index) in newarraylist" :key="index"
                class="list-group-item d-flex justify-content-between  ">
                <div class="col1">
                  <span class="">{{ data.newtaskName }}</span>
                  <br><span class="smallFont"><i class="bi bi-clock"></i> {{ data.dateTime }}</span>
                </div>
                <div class="col2">
                  <button class="btn btn-sm btn-danger mx-1 custButton" @click="adveditTask(index, data)"
                    title="Edit"><i class="bi bi-pencil-square"></i></button>
                  <button class="btn btn-sm btn-danger custButton" @click="advdeleteTask(index, data)" title="Delete"><i
                      class="bi bi-trash"></i></button>
                </div>
              </li>
            </ul>
            <ul v-if="newarraylistDeleted.length > 0"
              class="list-group list-group-flush border-bottom border-top border-dark   mt-3  customRed   ">
              <h6 class="px-3 py-2 mb-0 fw-bold bold bg-danger-subtle text-danger-emphasis"><span
                  class="badge text-bg-warning">{{
                    newarraylistDeleted.length
                  }}</span>
                Completed
                Task
              </h6>
              <li v-for="(data, index) in newarraylistDeleted" :key="index"
                class="list-group-item d-flex justify-content-between  ">
                <div>
                  <span class="">{{ data.newtaskName }}</span>
                  <br><span class="smallFont"><i class="bi bi-clock"></i> {{ data.dateTime }}</span>
                </div>
                <div>
                  <button class="btn btn-sm btn-danger mx-1 custButton" @click="backToPending(index, data)"
                    title="Back to Pending TAsk"><i class="bi  bi-arrow-counterclockwise"></i></button>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<script setup>
import { ref } from 'vue'
const taskName = ref(null)
const arraylist = ref([])

const newtaskName = ref(null)
const dateTime = ref(null)
const newarraylist = ref([])
const newarraylistDeleted = ref([])




const runTask = () => {
  if (!taskName.value) {
    return
  } else {

    arraylist.value.unshift({ "taskName": taskName.value });
    taskName.value = ""
    console.log(arraylist.value)
  }

}

const deleteTask = (index) => {
  arraylist.value.splice(index, 1)
}

const advrunTask = () => {
  if (!newtaskName.value) {
    return
  } else {
    const date = new Date();
    const options = { weekday: 'short', month: 'short', day: '2-digit', year: 'numeric', hour: '2-digit', minute: '2-digit', second: '2-digit', hour12: false };
    dateTime.value = date.toLocaleString('en-US', options).replace(',', '');
    newarraylist.value.unshift({ "newtaskName": newtaskName.value, "dateTime": dateTime.value });
    newtaskName.value = ""
  }
}
const advdeleteTask = (index, data) => {
  console.log(data, index)
  newarraylist.value.splice(index, 1)
  newarraylistDeleted.value.unshift(data)
  console.log(newarraylistDeleted.value)
}

const adveditTask = (index, data) => {
  newtaskName.value = data.newtaskName
  newarraylist.value.splice(index, 1)
  console.log(newtaskName.value)
}

const backToPending = (index, data) => {
  newarraylistDeleted.value.splice(index, 1)
  newarraylist.value.unshift(data)
}


</script>

<style scoped>
.smallFont {
  font-size: 9px;
  color: #666
}

.customGreen {
  background: rgb(208, 245, 208);
}

.customRed {
  background: rgb(247, 224, 224);
}

.custButton {
  font-size: 12px;
  padding: 2px 5px;
}

.list-group-item {
  border-bottom: 1px dashed #000;
  background: none;
}


.hrSet {
  border-top: 1px solid #999;
  margin: 30px 0px;
  opacity: 1;
}
</style>
