<script setup>
  import axios from 'axios'
  import {reactive, ref, onMounted} from 'vue'
  import { ElMessage } from 'element-plus';

  const books = reactive([])
  const getStudent = () => {
    axios.get('http://127.0.0.1:5000/books',).then(
      (res => {
        books.splice(0,books.length)
        books.push(...res.data.results)
        console.log("update!")
      })
    )


  }

  onMounted(() => {
    getStudent()
  })

  const handleDelete = (index, scope) => {
    console.log(index,scope)

    // fetch('http://127.0.0.1:5000/books/1',{method:"DELETE"}).then(
    //   ()=>{
    //     getStudent()
    //   }
    // )
    axios.delete(`http://127.0.0.1:5000/books/${scope.id}`).then(() => {
      getStudent()
    })
  }
  

</script>

<template>

  



<el-space direction="vertical"  :size="15">
  <h1>Book management System</h1>
  <el-table :data="books" style="width: 100%">
    <el-table-column label = "ID" prop='book_number'/>
    <el-table-column label = "Name" prop='book_name'/>
    <el-table-column label = "Type" prop='book_type'/>
    <el-table-column label = "Price" prop='book_prize'/>
    <el-table-column label = "Author" prop='author'/>
    <el-table-column label = "publisher" prop='book_publisher' width="120"/>


    <el-table-column label="Operations" width="190">
      <template #default="scope">
        <el-button size="small" @click="handleEdit(scope.$index, scope.row)"
          >Edit</el-button
        >
        <el-button
          size="small"
          type="danger"
          @click="handleDelete(scope.$index, scope.row)"
          >Delete</el-button
        >
      </template>
    </el-table-column>
    

  </el-table>


  <el-button 
      type="primary" 
      round
      >Add Books
    </el-button>



</el-space>


</template>

<style scoped>

</style>
