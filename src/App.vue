

<template>
<div class="container">
  <div class="row">
    <div class="col">
      <h3 class="text-center mt-5">Todo List</h3>
      <hr>
    
      <table class="table table-striped">
        <tr v-for="(todo, i) in todoList" :key="todo.id">
          <td>{{ i + 1 }}</td>
          <td><span :class="todo.status == true ? 'selesai' : 'gak-selesai'">{{ todo.nama }}</span></td>
          <td>
            <input type="checkbox" v-model="todo.status">
          </td>
          <td class="text-end">
            <button type="button" class="btn-sm btn-outline-info" @click="editTodo(i)">Edit</button>
            <button type="button" class="btn-sm btn-outline-danger" @click="deleteTodo(i)">Hapus</button>
            <button type="button" class="btn btn-warning mr-2" @click="simpanEdit(i)">Edit</button>

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
  import { onMounted, reactive, ref } from 'vue'
  import Api from './services/api'
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
    return row ; 
  }
  
  const simpanEdit = async (i:number) => {
     
     const todoBody = {
        id:'',
        nama_kegiatan:todo.value,
        status:2 ,
      }
      
     
     const y = todoList[i]; 
    
     const path = '/task' + '/' + y.id; 
     const data = await Api.putResource(path,todoBody,'PUT')
     todoList.splice(0,todoList.length); 
     const mintaData = await Api.getResource('/',todoList)
     todo.value = '';
     isEdit.value = false ; 
     }
   
     
  const addTodo = async () => {
      
      
       const todoBody = {
        id:'',
        nama_kegiatan:todo.value,
        status:2 ,
      }
      
    try{
      const data = await Api.postResource('/task',todoBody,'POST')
      todo.value = '';
      
            const response = await fetch('http://localhost:8181');
            const sample = await response.json();
            const lastArr =  sample.slice(-1).pop();
            console.log(lastArr)

                todoList.push({
                  id: lastArr.id,
                  nama: lastArr.nama_kegiatan,
                  status: lastArr.status == 1 ? true : false
                })
      }
    catch(err){
    console.log(err)
    }

    
    }


  const deleteTodo = async (i: number) =>{
    const index = todoList[i]; 
    
    const path = '/task' + '/' + index.id;
    todoList.splice(0,todoList.length)
    const data = await Api.deleteResource(path,'DELETE');
    const get = await Api.getResource('/',todoList)
  }

  onMounted(async() =>{
    console.log(todoList)
    try {
      const data = await Api.getResource('/',todoList)
    }
    catch(err){
      console.log(err)
    }
    
  })

</script>

<style scoped>
.selesai {
  text-decoration: line-through;
}
</style>