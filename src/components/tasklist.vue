<template>
  <div class="container py-5 ">
    <div class="row ">

      <div class="col-sm-6 mt-5 ">
        <div class="card mx-auto border-0" style="width:300px; ">
          <img src="../../public/todo.jpg" height="150" class="card-img-top border border-dark" alt="Basic Task list">
          <div class="card-body border border-dark">
            <h5 class="card-title text-dark">Basic Task List</h5>
            <p class="card-text">Features</p>
            <ul>
              <li>Create, Delete Task</li>
            </ul>
            <input type="text" class="form-control" v-model.trim="taskName" @keyup.enter="runTask"
              placeholder="Enter your task">
            <button class="btn btn-success form-control mt-2 btn-sm " type="submit" @click="runTask">Add to Task
              list</button>
          </div>
          <ul v-if="arraylist.length > 0" class="list-group list-group-flush border mt-3  border-success customGreen">
            <h6 class="px-3 py-2"><span class="badge text-bg-warning">{{ arraylist.length }}</span> Pending Task
            </h6>
            <li v-for="(data, index) in arraylist" :key="index" class="list-group-item d-flex justify-content-between ">
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

      <div class="col-sm-6 mt-5 ">
        <div class="card mx-auto border-0 " style="width:300px;">
          <img src="../../public/todo1.jpg" height="150" class="card-img-top border border-dark"
            alt="Advance Task List">
          <div class="card-body border border-dark">
            <h5 class="card-title text-dark">Advance Task List</h5>
            <p class="card-text">Features</p>
            <ul>
              <li>Create, Delete Task</li>
              <li>Edit Task</li>
              <li>Time stamp</li>
              <li>Blank task not Allowed</li>
              <li>Undo Deleted Task</li>
            </ul>
            <input type="text" class="form-control" v-model.trim="newtaskName" @keyup.enter="advrunTask"
              placeholder="Enter your task">
            <button class="btn btn-success form-control mt-2 btn-sm " type="submit" @click="advrunTask">Add to
              Task list</button>
          </div>
          <ul v-if="newarraylist.length > 0" class="list-group list-group-flush border mt-3 border-success customGreen">
            <h6 class="px-3 py-2"><span class="badge text-bg-warning">{{ newarraylist.length }}</span> Pending Task
            </h6>
            <li v-for="(data, index) in newarraylist" :key="index"
              class="list-group-item d-flex justify-content-between  ">
              <div class="col1">
                <span class="fw-bold">{{ data.newtaskName }}</span>
                <br><span class="smallFont"><i class="bi bi-clock"></i> {{ data.dateTime }}</span>
              </div>
              <div class="col2">
                <button class="btn btn-sm btn-danger mx-1 custButton" @click="adveditTask(index, data)" title="Edit"><i
                    class="bi bi-pencil-square"></i></button>
                <button class="btn btn-sm btn-danger custButton" @click="advdeleteTask(index, data)" title="Delete"><i
                    class="bi bi-trash"></i></button>
              </div>
            </li>
          </ul>
          <ul v-if="newarraylistDeleted.length > 0"
            class="list-group list-group-flush border mt-3 border-danger customRed">
            <h6 class="px-3 py-2"><span class="badge text-bg-warning">{{ newarraylistDeleted.length }}</span> Completed
              Task
            </h6>
            <li v-for="(data, index) in newarraylistDeleted" :key="index"
              class="list-group-item d-flex justify-content-between  ">
              <div>
                <span class="fw-bold">{{ data.newtaskName }}</span>
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

  arraylist.value.unshift({ "taskName": taskName.value });
  taskName.value = ""
  console.log(arraylist.value)

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
  border: 1px dashed #ccc;
}


.hrSet {
  border-top: 1px solid #999;
  margin: 30px 0px;
  opacity: 1;
}
</style>
