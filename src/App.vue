<template>
  <div class="container background-color-c">
    <!-- Client Section -->
    <div class="client background-color-a scrollable">
      <h2>ОБЩИЕ ЗАБОРЫ</h2>
      <div>
        <div
            class="step-box background-color-d"
            v-for="(fence, index) in fences"
            :key="index"
            @click="showModal(fence)"
        >
          <h3 class="color-b ">{{ fence.title }}</h3>
        </div>
      </div>
    </div>

    <!-- Content Section -->
    <div class="content background-color-b scrollable">
      <div class="color-e speech-element" v-for="(speech, index) in speeches[currentSpeech]?.speech"
           :key="index"><p v-if="speech !== ''">{{ speech }}</p><br v-else></div>
    </div>

    <!-- Buttons Section -->
    <div class="buttons background-color-b">
      <button
          class="background-color-d btn"
          :class="{ 'btn-disabled': currentSpeech === 0 }"
          @click="prevStep"
          :disabled="currentSpeech === 0"
      >← НАЗАД
      </button>
      <button
          class="background-color-d btn"
          :class="{ 'btn-disabled': currentSpeech === speeches.length - 1 }"
          @click="nextStep"
          :disabled="currentSpeech === speeches.length - 1"
      >ДАЛЕЕ →
      </button>
    </div>

    <!-- Fences Section -->
    <div class="fences background-color-a scrollable">
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
    <div v-if="isModalVisible" class="modal-overlay" @click.self="closeModal">
      <div class="modal-content color-a background-color-c">
        <h3>{{ selectedFence?.title }}</h3>

        <div v-if="Array.isArray(selectedFence?.text)"
             v-for="(text, index) in selectedFence?.text"
             :key="index">
          <br v-if="text === ''">
          <p v-else-if="text !== ''">{{ text }}</p>
          <br>
        </div>
        <div v-else>
          <br v-if="selectedFence?.text === ''">
          <p v-else-if="selectedFence?.text !== ''">{{ selectedFence?.text }}</p>
        </div>


        <!-- Nested fences in modal -->
        <div class="" v-if="selectedFence?.fences && selectedFence.fences.length > 0">

          <div class="modal-buttons">
            <button v-for="(subFence, index) in selectedFence.fences" :key="index" class="nested-fence-btn"
                    @click="showNestedFence(subFence)">
              {{ subFence.title }}
            </button>
          </div>
        </div>

        <!-- Display nested fence content if available -->
        <div v-if="nestedFence">
          <h4>{{ nestedFence.title }}</h4>
          <p v-if="Array.isArray(nestedFence?.text)"
             v-for="(text, index) in nestedFence?.text"
             :key="index">{{ text }}
          </p>
          <p v-else>{{ nestedFence.text }}</p>
        </div>

        <div class="modal-buttons">
          <button v-if="nestedFence" class="btn-close-nested" @click="closeNestedFence">Закрыть подзабор</button>
          <button class="btn-close" @click="closeModal">Закрыть</button>
        </div>

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
      fences: [],
      currentSpeech: 0,
      isModalVisible: false,
      selectedFence: null,
      nestedFence: null, // Stores the selected nested fence
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
        this.fences = response.data.fences;
      } catch (error) {
        console.error("Failed to load speeches:", error);
      }
    },
    showModal(fence) {
      this.selectedFence = fence;
      this.nestedFence = null; // Clear any previous nested selection
      this.isModalVisible = true;
    },
    closeModal() {
      this.isModalVisible = false;
      this.selectedFence = null;
      this.nestedFence = null;
    },
    showNestedFence(subFence) {
      this.nestedFence = subFence; // Show selected nested fence content
    },
    closeNestedFence() {
      this.nestedFence = null; // Close nested fence view
    }
  },
  mounted() {
    this.loadSpeeches();
  }
};
</script>

<style scoped>

.speech-element {
  margin: 20px;
}

.modal-buttons {
  display: block;
  margin: 16px;
}

.modal-content h3, .modal-content h4 {
  margin-bottom: 20px;
}

.modal-content p {
  text-align: justify;
}

.btn-close, .btn-close-nested {
  margin-top: 20px;
  padding: 16px 32px;
  border: none;
  background-color: #444;
  color: white;
  border-radius: 8px;
  cursor: pointer;
  margin: 8px;
}

.nested-fence-btn {
  padding: 16px 32px;
  border: none;
  background-color: #444;
  color: white;
  border-radius: 5px;
  cursor: pointer;
  font-size: 14px;
  margin: 8px;
}

.btn-close {
  margin-top: 20px;
  padding: 16px 32px;
  border: none;
  background-color: #444;
  color: white;
  border-radius: 8px;
  cursor: pointer;
}

.nested-fence-btn:hover {
  background-color: #bbb;
}

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
  border-radius: 8px;
  width: 70%;
  text-align: center;
  font-size: 17pt;
  padding: 32px;
}

.modal-content h3 {
  margin-bottom: 20px;
}


.container {
  display: grid;
  grid-template-columns: 1fr 3fr 1fr;
  grid-template-rows: 1fr 1fr 1fr 1fr 1fr;
  grid-template-areas:
    "client content fences"
    "client content fences"
    "client content fences"
    "client content fences"
    "client buttons fences";
  height: 100vh;
}

.btn-disabled {
  background-color: #ccc;
  color: #666;
  cursor: not-allowed;
}


.fences {
  grid-area: fences;
  border: 5px solid black;
  padding: 10px;
  border-radius: 16px;
  margin: 16px;
}

.client {
  grid-area: client;
  border: 5px solid black;
  padding: 10px;
  border-radius: 16px;
  margin: 16px;
}

.fences h2 {
  text-align: center;
  border: 5px solid black;
  border-radius: 16px;
  margin: 8px;
}

.client h2 {
  text-align: center;
  border: 5px solid black;
  border-radius: 16px;
  margin: 8px;
}

.step-box {
  border: 5px solid black;
  height: fit-content;
  margin: 8px;
  padding: 10px;
  border-radius: 15px;
  text-align: center;
  font-size: 14pt;
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
  font-size: 22px;
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
