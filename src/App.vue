<template>
  <div class="container background-color-c">
    <div class="steps background-color-a scrollable">
      <h2>STEPS</h2>
      <div class="">
        <div class="step-box background-color-d" v-for="step in steps" :key="step">
          <p class="color-b">{{ step }}</p>
        </div>
      </div>
    </div>

    <div class="content background-color-b">
      <p class="color-e">
        Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
      </p>
    </div>
    <div class="buttons background-color-b">
      <button class="background-color-d btn" v-for="button in buttons" :key="button">{{ button }}</button>
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      steps: ["СТАРТ","ЭТАП","ЭТАП","ЭТАП","ЭТАП","КОНЕЦ",],
      buttons: ["<- НАЗАД", "ЗАБОР", "ДАЛЕЕ ->"]
    };
  }
};
</script>

<style scoped>

.container {
  display: grid;
  grid-template-columns: 1fr 3fr;
  grid-template-rows: 1fr 1fr 1fr 1fr;
  grid-template-areas:
    "steps content"
    "steps content"
    "steps content"
    "steps buttons";
  height: 100vh;
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
  border-radius: 16px;
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
