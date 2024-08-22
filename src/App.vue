<template>
  <div>
    <AnswerStats :percentage="percentage" :text="statsText" :visible="statsVisible" />
    <div class="elements">
      <TheQuestion :question="currentQuestion" />
      <button id="buttonyes" @click="next('yes')">YES</button>
      <button id="buttonno" @click="next('no')">NO</button>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'
import TheQuestion from './components/TheQuestion.vue'
import AnswerStats from './components/AnswerStats.vue'

export default {
  components: {
    TheQuestion,
    AnswerStats
  },
  setup() {
    const cacheList = ref([])
    const prevIndex = ref(2)
    const data = ref(null)
    const currentQuestion = ref('Are you ready!')
    const percentage = ref('00 %')
    const statsText = ref('answered YES to<br>previous question')
    const statsVisible = ref(false)

    const baseUrl = process.env.VUE_APP_API_URL;

    const fetchData = async () => {
      try {
        const response = await fetch(`${baseUrl}/questions`)
        data.value = await response.json();
        console.log(data.value);
      } catch (error) {
        console.error('Failed to fetch data:', error);
      }
    };

    const resetAnimation = () => {
      const questionElement = document.querySelector('.question');
      if (questionElement) {
        questionElement.style.animation = 'none';
        questionElement.offsetHeight;
        questionElement.style.animation = null;
      }
    };

    const next = async (answer) => {
      if (!data.value) {
        await fetchData();
      }
      if (prevIndex.value !== -1) {
        await sendAnswer(prevIndex.value, answer);
      }
      console.log(prevIndex.value);
      randomQn();
      resetAnimation();
    };

    const randomQn = () => {
      let qnNo = Math.floor(Math.random() * Object.keys(data.value).length);
      while (cacheList.value.includes(qnNo)) {
        qnNo = Math.floor(Math.random() * Object.keys(data.value).length);
      }
      currentQuestion.value = data.value[qnNo];
      cacheList.value.push(qnNo);
      prevIndex.value = qnNo;

      if (data.value && cacheList.value.length === Object.keys(data.value).length) {
        cacheList.value = [];
      }
    };

    const sendAnswer = async (index, answer) => {
      try {
        const response = await fetch(`${baseUrl}/submit-answer`, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({ index, answer }),
        });
        const result = await response.json();
        percentage.value = `${result.percentage}%`;
        statsText.value = `answered ${answer.toUpperCase()} to<br>${currentQuestion.value}`;
        statsVisible.value = true;
      } catch (error) {
        console.error('Failed to send answer:', error);
      }
    };

    onMounted(() => {
      fetchData();
    });

    return { currentQuestion, percentage, statsText, statsVisible, next };
  }
}
</script>

<style>
.elements {
  display: flex;
  flex-direction: column;
  align-items: center;
}

button {
  margin: 5px;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}

.question {
  font-size: 24px;
  text-align: center;
}

@media (max-width: 600px) {
  .question {
    font-size: 20px;
  }

  button {
    font-size: 14px;
    padding: 8px 16px;
  }
}
</style>
