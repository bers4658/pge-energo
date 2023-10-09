<template>
  <div class="cards-container">
    <div
      v-for="(item, index) in tableData"
      :key="index"
      class="card"
      :style="{ 'background-color': item.read ? 'white' : 'lightgray' }"
      :class="{ 'read': item.read, 'selected': index === selectedIndex }" 
      tabindex="0"
      @keydown.space="markAsRead(index)"
      @click="selectCard(index)"
    >

      <div class="card-header">
        <h2>{{ item.equipment }}</h2>
        <p>{{ item.date }}</p>
      </div>
      <div class="card-body">
        <p><strong>Важность:</strong> {{ item.importance }}</p>
        <p><strong>Сообщение:</strong> {{ item.message }}</p>
        <p><strong>Ответственный:</strong> {{ item.responsible }}</p>
        <p v-if="item.read"><strong>Статус:</strong> Прочитано</p>
        <p v-else><strong>Статус:</strong> Не прочитано</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    tableData: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      selectedIndex: null,
      cardsData: [...this.tableData],
    };
  },
 
  methods: {
    selectCard(index) {
      this.selectedIndex = index;
    },
    markAsRead(index) {
      this.cardsData[index].read = true;
    },
  },
};
</script>

<style scoped>
.selected {
  border: 2px solid #007BFF;
}
.cards-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

.card {
  width: calc(30% - 20px);
  margin: 10px;
  padding: 10px;
  box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.2);
}

.card-header {
  text-align: center;
}

.card-header h2 {
  margin: 0;
}

.card-body p {
  margin: 5px 0;
}
</style>
