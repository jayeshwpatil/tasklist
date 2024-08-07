<template>
  <div class="container mt-4 py-5 ">
    <div class="row ">

      <div class="col-sm-6">
        <div class="card mx-auto" style="width:300px; ">
          <img src="../../public/todo.jpg" class="card-img-top" alt="Todo">
          <div class="card-body">
            <h4 class="card-title text-primary">Basic Task List</h4>
            <p class="card-text">Create, delete your task list effortlessly.</p>
            <hr class="hrSet">
            <input type="text" class="form-control" v-model.trim="taskName" @keyup.enter="runTask"
              placeholder="Enter your task">
            <button class="btn btn-success form-control mt-2 btn-sm " type="submit" @click="runTask">Add to
              list</button>
          </div>
          <ul class="list-group list-group-flush">
            <li v-for="(data, index) in arraylist" :key="index" class="list-group-item d-flex justify-content-between ">
              <div>
                <span class="">{{ data.taskName }}</span>
              </div>
              <div>
                <button class="btn btn-sm btn-danger" @click="deleteTask(index)"><i class="bi bi-trash"></i></button>
              </div>
            </li>
          </ul>
        </div>
      </div>

      <div class="col-sm-6">
        <div class="card mx-auto" style="width:300px;">
          <img src="../../public/todo1.jpg" class="card-img-top" alt="Todo">
          <div class="card-body">
            <h4 class="card-title text-primary">Advance Task List</h4>
            <p class="card-text">Create, manage and delete your task list effortlessly.</p>
            <hr class="hrSet">
            <input type="text" class="form-control" v-model.trim="newtaskName" @keyup.enter="advrunTask"
              placeholder="Enter your task">
            <button class="btn btn-success form-control mt-2 btn-sm " type="submit" @click="advrunTask">Add to
              list</button>
          </div>
          <ul v-if="newarraylist.length > 0" class="list-group list-group-flush">
            <h6 class="p-3"><span class="badge text-bg-warning">{{ newarraylist.length }}</span> Pending Task </h6>
            <li v-for="(data, index) in newarraylist" :key="index"
              class="list-group-item d-flex justify-content-between customGreen ">
              <div class="col1">
                <span class="fw-bold">{{ data.newtaskName }}</span>
                <br><span class="smallFont"><i class="bi bi-clock"></i> {{ data.dateTime }}</span>
              </div>
              <div class="col2">
                <button class="btn btn-sm btn-danger mx-1" @click="adveditTask(index, data)" title="Edit"><i
                    class="bi bi-pencil-square"></i></button>
                <button class="btn btn-sm btn-danger" @click="advdeleteTask(index, data)" title="Delete"><i
                    class="bi bi-trash"></i></button>
              </div>
            </li>
          </ul>
          <ul v-if="newarraylistDeleted.length > 0" class="list-group list-group-flush">
            <h6 class="p-3"><span class="badge text-bg-warning">{{ newarraylistDeleted.length }}</span> Completed Task
            </h6>
            <li v-for="(data, index) in newarraylistDeleted" :key="index"
              class="list-group-item d-flex justify-content-between customRed ">
              <div>
                <span class="fw-bold">{{ data.newtaskName }}</span>
                <br><span class="smallFont"><i class="bi bi-clock"></i> {{ data.dateTime }}</span>
              </div>
              <div>
                <button class="btn btn-sm btn-danger mx-1" @click="backToPending(index, data)"
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
.container {
  background: #f4f4f4;
}

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

.col1 {
  width: 72%;
}

.col2 {
  width: 28%;
}

.hrSet {
  border-top: 1px solid #999;
  margin: 30px 0px;
  opacity: 1;
}
</style>
