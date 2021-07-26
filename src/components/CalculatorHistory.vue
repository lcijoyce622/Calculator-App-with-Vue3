<template>
  <div id="history">
    <h2 class="col">
      歷史紀錄
      <button type="button" class="btn btn-light" @click="clearHistory">
        clear
      </button>
    </h2>
    <div class="history_box">
      <ul>
        <li v-for="(history, key) in storeHistory" :key="key">
          {{ history }}
        </li>
        <li v-if="storeHistory.length === 0">沒有歷史資料</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  inject: ["storeHistory"],

  methods: {
    clearHistory() {
      localStorage.removeItem("calculateHistory");
      localStorage.clear();
      localStorage;
      this.storeHistory.splice(0, this.storeHistory.length);
    },
    setHistory() {
      if (JSON.parse(localStorage.getItem("calculateHistory")) != null) {
        for (
          let i = 0;
          i < JSON.parse(localStorage.getItem("calculateHistory")).length;
          i++
        ) {
          this.storeHistory.push(
            JSON.parse(localStorage.getItem("calculateHistory"))[i]
          );
        }
      }
    },
  },
  mounted() {
    this.setHistory();
  },
};
</script>
<style lang="scss" scoped>
#history {
  color: whitesmoke;
  margin: auto;
  .history_box {
    border: 5px rgba(75, 85, 99, $alpha: 1);
    border-style: dotted;
    border-radius: 20px;
    padding: 1rem;
  }
}
</style>