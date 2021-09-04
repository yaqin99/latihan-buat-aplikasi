

<template>
<div class="container">
  <div class="row">
    <div class="col">
      <h3 class="text-center mt-5">Todo List</h3>
      <hr>
    
      <table class="table table-striped">
        <tr v-for="(todo, i) in todoList" :key="todo.id">
          <td>{{ i + 1 }}</td>
          <td>{{ todo.nama }}</td>
          <td>
            <input type="checkbox" v-model="todo.status">
          </td>
          <td class="text-end">
            <button type="button" class="btn-sm btn-outline-info" @click="editTodo(i)">Edit</button>
            <button type="button" class="btn-sm btn-outline-danger" @click="deleteTodo(i)">Hapus</button>
          </td>
           
        </tr>
      </table>

      <hr>

      <div class="d-flex">
        <input type="text" v-model="todo" class="form-control">

        
       
          <button type="button" class="btn btn-primary mr-2" @click="addTodo()">Simpan</button>
        

        
      </div>

      {{todoList}}
      <br>
      {{todo}}

    </div>
  </div>
</div>

</template>
<script setup lang="ts">
  import { reactive, ref } from 'vue'
  type todoType = {
    id: number;
    nama: string;
    status: boolean;
  }
 let todoList = reactive<todoType[]>([])
  
  const todo = ref('');
  const idTodo = ref();
  // let cekEdit:any ; 

  let isEdit = ref(); 
  console.log(isEdit)
  


  const editTodo = (i: number) => {
    //  cekEdit = true ;
    const row = todoList[i];
    todo.value = row.nama; 
    idTodo.value = row.id;
    
    isEdit.value = true ; 
    console.log(isEdit.value)
  }

  const simpanEdit = () => {
     
     todoList.splice(idTodo.value - 1,1,{
     id:idTodo.value,nama:todo.value,status:false
    })
  }
    const addTodo = () => {
      todoList.push({
        

      })
    // // cekEdit = false ;
    //   if(isEdit.value == true){
    //     simpanEdit()
    //     isEdit.value = false;
    //     todo.value = '' ; 
    //   }
    //   else {
    //   const arrLength = todoList.length + 1;
    //   todoList.push({
    //     id: arrLength, nama: todo.value, status: false
    //   })
      }
    todo.value = '';
    
  }
  const deleteTodo = (i: number) =>{
    todoList.splice(i, 1)
  }
  onMounted(async() =>{
    const response = await fetch ('https://localhost:8181');
    const data = await response.json();
    // todoList = [];
    if(data.lenght > 0 ){
      data.forEach((d: any) => {
        todoList.push({
          id: d.id_task,
          nama: d.nama_task,
          status: d.status == 1 ? true : false
        })
      });
    }
  }
</script>

