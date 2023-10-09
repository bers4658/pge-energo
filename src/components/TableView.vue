<template>
  <div class="table-container">
    <table>
      <thead>
        <tr>
          <th>Дата</th>
          <th>Важность</th>
          <th>Оборудование</th>
          <th>Сообщение</th>
          <th>Ответственный</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in tableData" :key="index">
          <td>{{ item.date }}</td>
          <td>{{ item.importance }}</td>
          <td>{{ item.equipment }}</td>
          <td>{{ item.message }}</td>
          <td>{{ item.responsible }}</td>
          <td>
              <button @click="editItem(index)">Редактировать</button>
          </td>
          <td>
              <button @click="deleteItem(index)">Удалить</button>
          </td>
        </tr>
      </tbody>
    </table>
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
    methods: {

    editItem(index) {
      this.$emit("edit-item", index);
    },
    deleteItem(index) {
      this.$emit("delete-item", index);
    },
  },
  };
  </script>
  
  <style scoped>
  .table-container {
    text-align: center;
    margin: 0 auto;
  }
  
  table {
    width: 100%;
    border-collapse: collapse;
  }
  
  table, th, td {
    border: 1px solid #ccc;
  }
  
  th, td {
    padding: 10px;
  }
  </style>
  