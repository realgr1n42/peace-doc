<template>
  <div class="container background-color-c">
    <div class="content background-color-b">
      <p class="color-e">{{ speeches[currentSpeech]?.speech }}</p>
    </div>
    <div class="buttons background-color-b">
      <button
          class="background-color-d btn"
          :class="{ 'btn-disabled': currentSpeech === 0 }"
          @click="prevStep"
          :disabled="currentSpeech === 0"
      ><- НАЗАД</button>
      <button
          class="background-color-d btn"
          :class="{ 'btn-disabled': currentSpeech === speeches.length - 1 }"
          @click="nextStep"
          :disabled="currentSpeech === speeches.length - 1"
      >ДАЛЕЕ -></button>
    </div>
    <div class="steps background-color-a scrollable">
      <h2>ЗАБОРЫ</h2>
      <div>
        <div
            class="step-box background-color-d"
            v-for="(fence, index) in speeches[currentSpeech]?.fences"
            :key="index"
            @click="showModal(fence)"
        >
          <h3 class="color-b">{{ fence.title }}</h3>
        </div>
      </div>
    </div>
    <!-- Modal -->
    <div v-if="isModalVisible" class="modal-overlay " @click.self="closeModal">
      <div class="modal-content color-a background-color-c">
        <h3>{{ selectedFence?.title }}</h3>
        <p>{{ selectedFence?.text }}</p>
        <button class="btn-close" @click="closeModal">Закрыть</button>
      </div>
    </div>
  </div>
</template>


<script>
import axios from 'axios';

export default {
  data() {
    return {
      speeches: [],
      currentSpeech: 0,
      isModalVisible: false,
      selectedFence: null
    };
  },
  methods: {
    nextStep() {
      if (this.currentSpeech < this.speeches.length - 1) {
        this.currentSpeech++;
      }
    },
    prevStep() {
      if (this.currentSpeech > 0) {
        this.currentSpeech--;
      }
    },
    async loadSpeeches() {
      try {
        const response = await axios.get('/speeches.json');
        this.speeches = response.data.speeches;
      } catch (error) {
        console.error("Failed to load speeches:", error);
      }
    },
    showModal(fence) {
      this.selectedFence = fence;
      this.isModalVisible = true;
    },
    closeModal() {
      this.isModalVisible = false;
      this.selectedFence = null;
    }
  },
  mounted() {
    this.loadSpeeches();
  }
};

</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}

.modal-content {
  background: white;
  padding: 20px;
  border-radius: 8px;
  width: 80%;
  max-width: 500px;
  text-align: center;
}

.btn-close {
  margin-top: 20px;
  padding: 8px 16px;
  border: none;
  background-color: #444;
  color: white;
  border-radius: 8px;
  cursor: pointer;
}

.container {
  display: grid;
  grid-template-columns: 3fr 1fr;
  grid-template-rows: 1fr 1fr 1fr 1fr 1fr;
  grid-template-areas:
    "content steps"
    "content steps"
    "content steps"
    "content steps"
    "buttons steps";
  height: 100vh;
}

.btn-disabled {
  background-color: #ccc;
  color: #666;
  cursor: not-allowed;
}


.steps {
  grid-area: steps;
  border: 5px solid black;
  padding: 10px;
  border-radius: 16px;
  margin: 16px;
}

.steps h2 {
  text-align: center;
  border: 5px solid black;
  border-radius: 16px;
  margin: 8px;
}

.step-box {
  border: 5px solid black;
  height: 150px;
  margin: 8px;
  padding: 10px;
  border-radius: 16px;
  text-align: center;
  font-size: 10pt;
}

.step-box p {
  font-size: 30px;
  margin: 16px;
  background-color: transparent;
  text-align: center;
}

.content {
  grid-area: content;
  border: 5px solid black;
  padding: 20px;
  border-radius: 16px;
  margin: 16px;
  font-weight: bold;
  font-size: 30px;
}

.buttons {
  grid-area: buttons;
  display: flex;
  justify-content: space-around;
  padding: 10px;
  border: 5px solid black;
  border-radius: 16px;
  margin: 16px;
}

.btn {
  min-width: 25%;
  margin: 16px;
  border: 5px solid black;
  border-radius: 16px;
  cursor: pointer;
  font-size: 20px;
}

.scrollable {
  overflow: auto;
}


</style>
