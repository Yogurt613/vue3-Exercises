<!--建立一個簡單的待辦事項清單應用程式：
1. 新增、刪除待辦事項
新增使用v-model刪除用v-on 綁定事件
新增待辦事項：於input欄位輸入後通過按下“新增”按鈕或Enter鍵來添加新的待辦事項。
刪除待辦事項：通過點擊“刪除”按鈕來移除指定的待辦事項
2. 標記已完成的事項
設一個checkbox去做標記 若已完成會用checkbox呈現勾勾狀態並且劃掉
3. 使用 v-for 和 v-if 指令來渲染清單和篩選未完成/已完成事項
使用v-for把每個項目生成出來 每個項目清單後面放置一個checkbox用來標記已完成未完成 用v-if 去做切換只顯示未完成 另一個地方只顯示已完成
4. 使用 computed 屬性來計算未完成事項的數量-->
<template>
    <div id="app" class="container">
      <h2 class="text-center mb-4">Todo List</h2>
      
      
      <div class="input-group mb-3">
        <input v-model="newTodo" @keyup.enter="addtodo" type="text" class="form-control" placeholder="請輸入代辦事項">
        <button type="button" class="btn btn-primary" @click="addtodo">新增</button>
      </div>
      
      <!-- Todo List -->
      <ul class="list-unstyled">
        <li v-for="(item, index) in todos" :key="item.id" class="d-flex align-items-center mb-2">
          <div class="form-check flex-grow-1 mb-0">
            <input type="checkbox" class="form-check-input me-2" v-model="item.completed">
            <label class="form-check-label mb-0">{{ (index + 1) + '.' + item.title }}</label>
          </div>
          <button type="button" class="btn btn-success btn-sm ms-2" v-if="item.completed">已完成事項</button>
          <button type="button" class="btn btn-warning btn-sm ms-2" v-else>未完成事項</button>
          <button type="button" class="btn btn-danger btn-sm ms-2" @click="removetodo(item.id)">刪除</button>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        todos: [
          { id: 1, title: '學習 Vue.js', completed: false },
          { id: 2, title: '完成專案', completed: true }
        ],
        newTodo: ''
      }
    },
    methods: {
      addtodo() {
        if (this.newTodo.trim() === '') return;
        this.todos.push({
          id: this.todos.length + 1,
          title: this.newTodo,
          completed: false
        });
        this.newTodo = '';
      },
      removetodo(id) {
        this.todos = this.todos.filter(todo => todo.id !== id);
      }
    }
  }
  </script>
  
  <style scoped>
  .container {
    max-width: 600px;
    margin: auto;
    padding: 20px;
  }
  
  .input-group {
    max-width: 100%;
  }
  
  .form-check {
    display: flex;
    align-items: center;
    margin-bottom: 0;
  }
  
  .form-check .form-check-input {
    margin-right: 10px;  
  }
  
  .form-check .form-check-label {
    margin-bottom: 0; 
  }
  
  .btn-primary {
    background-color: #007bff;
    border-color: #007bff;
  }
  
  .btn-success {
    background-color: #28a745;
    border-color: #28a745;
  }
  
  .btn-warning {
    background-color: #ffc107;
    border-color: #ffc107;
  }
  
  .btn-danger {
    background-color: #dc3545;
    border-color: #dc3545;
  }
  </style>
  