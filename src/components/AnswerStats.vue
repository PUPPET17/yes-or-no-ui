<template>
    <div class="stats" :style="{ visibility: visible ? 'visible' : 'hidden' }">
      <div class="stats-percentage">{{ percentage }}</div>
      <div class="stats-text" v-html="text"></div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'AnswerStats',
    props: ['percentage', 'text', 'visible']
  }
  </script>
  
  <style scoped>
  .stats {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-content: center;
  }
  
  .stats-percentage {
    width: 50px;
    background-color: black;
    color: white;
    font-size: 23px;
    padding: 0.5rem;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  .stats-text {
    width: 200px;
    background-color: white;
    color: black;
    border-color: black;
    border-width: 3px;
    border-style: solid;
    padding: 7px;
  }
  </style>
  