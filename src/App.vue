<script setup>
  import axios from 'axios'
  import {reactive, ref, onMounted} from 'vue'
  import { ElMessage } from 'element-plus';

  const books = reactive([])
  const getStudent = () => {
    axios.get('http://127.0.0.1:5000/books/',).then(
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

  const ruleFormRef = ref()

  const resetInput = () =>{
    console.log("Run resetInput")
    book_form.author = ''
    book_form.book_name = ''
    book_form.book_prize = ''
    book_form.book_publisher = ''
    book_form.book_type = ''
    book_form.book_number = ''
    book_form.book_type = ''
  }
  
  const submitBook = () =>{
    axios.post('http://127.0.0.1:5000/books/',book_form).then(() => {
      console.log(JSON.stringify(book_form))
      resetInput()
      dialogFormVisible.value = false
      getStudent()
      }
    )
  }


const dialogTableVisible = ref(false)
const dialogFormVisible = ref(false)
const formLabelWidth = '140px'



const book_form = reactive({
  book_number:'',
  book_name:'',
  book_type:'',
  book_prize:'',
  author:'',
  book_publisher:'',
  id:"",
})




</script>

<template>

  



<el-space direction="vertical"  :size="15">
  <h1>Book management System</h1>
  <el-table :data="books" style="width: 100%">
    <el-table-column label = "Number" prop='book_number'/>
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
      @click="dialogFormVisible = true"
      type="primary" 
      round
      >Add Books
  </el-button>
  


  <el-dialog v-model="dialogFormVisible" title="Add a new Book">
    <el-form 
            ref="ruleFormRef" 
            :model="book_form">
      <el-form-item label="Number" :label-width="formLabelWidth">
        <el-input v-model="book_form.book_number" autocomplete="off" />
      </el-form-item>
      <el-form-item label="Name" :label-width="formLabelWidth">
        <el-input v-model="book_form.book_name" autocomplete="off" />
      </el-form-item>

      <el-form-item label="Type" :label-width="formLabelWidth">
        <el-select v-model="book_form.book_type" placeholder="Please select a zone">
          <el-option label="Type No.1" value="shanghai" />
          <el-option label="Type No.2" value="beijing" />
        </el-select>
      </el-form-item>

      <el-form-item label="Price" :label-width="formLabelWidth">
        <el-input v-model="book_form.book_prize" autocomplete="off" />
      </el-form-item>

      <el-form-item label="Author" :label-width="formLabelWidth">
        <el-input v-model="book_form.author" autocomplete="off" />
      </el-form-item>

      <el-form-item label="Publisher" :label-width="formLabelWidth">
        <el-input v-model="book_form.book_publisher" autocomplete="off" />
      </el-form-item>


      <el-form-item>
      <el-button @click="resetInput">Reset</el-button>
      <el-button type="primary" @click="submitBook(ruleFormRef)"
          >Confirm</el-button>
      </el-form-item>
    </el-form>
    <!-- <template #footer>
      <span class="dialog-footer">
        <el-button @click="resetInput(ruleFormRef)">Reset</el-button>
        <el-button type="primary" @click="dialogFormVisible = false"
          >Confirm</el-button
        >
      </span>
    </template> -->
  </el-dialog>



</el-space>


</template>

<style scoped>

</style>
