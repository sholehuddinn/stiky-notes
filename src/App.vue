<template>
  <main>
    <div class="container">
      <header>
        <h1 class="title">MEMO</h1>
        <button class="btn-title" @click="showForm = true">+</button>
      </header>
      <div class="card-container" >
        <div class="card" v-for="item in memos" :key="item.id" :style="{backgroundColor: item.bacgroundColour}">
          <p class="card-content">{{ item.memo }}</p>
          <p class="card-date">{{ item.date }}</p>
          <button class="btn-delete" @click="deleteMemo(item.id)">Delete</button>
        </div>
      </div>
    </div>
    <div class="form-overlay" v-if="showForm">
      <div class="form-modal">
        <div class="form-content">
          <span>{{ memo }}</span>
          <button class="btn-close" @click="showForm = false">&times;</button>
          <span class="err">{{ errorMessage }}</span>
          <textarea placeholder="Memo" v-model="memo" cols="30" rows="10" ></textarea>
          <button class="btn-save" @click="addMemo()" >Save</button>
        </div>
      </div>
    </div>
  </main>
</template>

<script setup>
import { ref } from 'vue';

const showForm = ref(false );
const memo = ref('');
const memos = ref([]);
const errorMessage = ref('');


function getRandomColor() {
  const letters = '0123456789ABCDEF';
  let color = '#';
  for (let i = 0; i < 6; i++) {
    color += letters[Math.floor(Math.random() * 16)];
  }
  return color;
}


function addMemo () {
  if (!memo.value) {
    errorMessage.value = 'Please enter a memo'
    return;
  };
  memos.value.push({
    id: Date.now(),
    memo: memo.value,
    date: new Date().toLocaleDateString('en-GB'),
    bacgroundColour: getRandomColor()
  });
  showForm.value = false;
  memo.value = '';
}

function deleteMemo (id) {
  memos.value = memos.value.filter((memo) => memo.id !== id);
}


</script>

<style scoped>
  main {
    height: 100vh;
    width: 100vw;
  }
  .container {
    max-width: 900px;
    margin: 0 auto;
    padding: 10px;
  }
  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 10px;
    border-bottom: 1px solid #ccc;
  }
  .title {
    font-size: 48px;
    color: #333;
    font-weight: bold;
  }
  .btn-title {
    background-color: #333;
    padding: 10px;
    border-radius: 50%;
    color: #fff;
    border: none;
    cursor: pointer;
    height: 50px;
    width: 50px;
    cursor: pointer;
    font-size: large;
  }
  .card-container {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
  }
  .card {
    width:255px;
    height: 255px;
    padding: 10px;
    background-color: #ffa;
    margin-bottom: 20px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }
  .form-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.8);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 10;
    transition: opacity 0.3s ease-in-out;
  }
  .form-modal {
    background-color: #fff;
    padding: 20px;
    border-radius: 10px;
    max-width: 500px;
  }
  .form-content {
    display: flex;
    flex-direction: column;
  }
  .btn-save {
    background-color: #333;
    padding: 10px 20px;
    border-radius: 5px;
    color: #fff;
    border: none;
    cursor: pointer;
    font-size: large;
    margin-top: 10px;
    width: 100%;
  }
  .btn-close {
    position: absolute;
    top: 10px;
    right: 10px;
    font-size: 30px;
    cursor: pointer;
    color: #333;
    background-color: none;
    transition: color 0.3s ease-in-out;
  }
  .err {
    color: red;
  }
  .btn-delete {
    background-color: #f00;
    padding: 5px 10px;
    border-radius: 5px;
    color: #fff;
    border: none;
    cursor: pointer;
    margin-top: 5px;
    width: 100%;
    font-size: large;
    transition: background-color 0.3s ease-in-out;
  }
</style>