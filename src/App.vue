<template>
  <div>
    <header>
      <div class="buttons">
        <button @click="switchView('table')" :class="{ 'button': true, 'active-button': activeView === 'table', 'inactive-button': activeView !== 'table' }">Таблица</button>
        <button @click="switchView('cards')" :class="{ 'button': true, 'active-button': activeView === 'cards', 'inactive-button': activeView !== 'cards' }">Карточки</button>
      </div>
      <div class="header-right">
        <input v-model="searchQuery" placeholder="Поиск" type="search">
      </div>
    </header>
    <div class="buttons">
      <button @click="showForm()" :class="{ 'button': true, 'add-button': true, 'left-margin': true }">Добавить</button>
    </div>
    <div class="content">
      <div v-if="activeView === 'table'">
         <TableView :tableData="filteredTableData" @delete-item="deleteItem" @edit-item="editItem" />
      </div>
      <div v-else-if="activeView === 'cards'">
        <CardsView :tableData="filteredTableData" />
      </div>
    </div>
    <div class="modal" v-if="isFormVisible">
      <div class="modal-content">
        <span class="close" @click="closeForm">&times;</span>
        <form @submit.prevent="addNewItem">
          <div class="form-group">
            <label for="date">Дата:</label>
            <input v-model="newItem.date" id="date" type="text" placeholder="Дата">
          </div>
          <div class="form-group">
            <label for="importance">Важность:</label>
            <input v-model="newItem.importance" id="importance" type="text" placeholder="Важность">
          </div>
          <div class="form-group">
            <label for="equipment">Оборудование:</label>
            <input v-model="newItem.equipment" id="equipment" type="text" placeholder="Оборудование">
          </div>
          <div class="form-group">
            <label for="message">Сообщение:</label>
            <input v-model="newItem.message" id="message" type="text" placeholder="Сообщение">
          </div>
          <div class="form-group">
            <label for="responsible">Ответственный:</label>
            <input v-model="newItem.responsible" id="responsible" type="text" placeholder="Ответственный">
          </div>
          <button type="submit">Добавить</button>
        </form>
      </div>
    </div>
    <div class="modal" v-if="isEditFormVisible">
      <div class="modal-content">
        <span class="close" @click="closeEditForm">&times;</span>
        <form @submit.prevent="saveEditedItem">
          <div class="form-group">
            <label for="editedDate">Дата:</label>
            <input v-model="editedItem.date" id="editedDate" type="text" placeholder="Дата">
          </div>
          <div class="form-group">
            <label for="editedImportance">Важность:</label>
            <input v-model="editedItem.importance" id="editedImportance" type="text" placeholder="Важность">
          </div>
          <div class="form-group">
            <label for="editedEquipment">Оборудование:</label>
            <input v-model="editedItem.equipment" id="editedEquipment" type="text" placeholder="Оборудование">
          </div>
          <div class="form-group">
            <label for="editedMessage">Сообщение:</label>
            <input v-model="editedItem.message" id="editedMessage" type="text" placeholder="Сообщение">
          </div>
          <div class="form-group">
            <label for="editedResponsible">Ответственный:</label>
            <input v-model="editedItem.responsible" id="editedResponsible" type="text" placeholder="Ответственный">
          </div>
          <button type="submit">Сохранить</button>
        </form>
      </div>
    </div>
    <div class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1">Предыдущая</button>
      <span>{{ currentPage }} / {{ totalPages }}</span>
      <button @click="nextPage" :disabled="currentPage === totalPages">Следующая</button>
    </div>
  </div>
</template>
<script>
import TableView from './components/TableView.vue';
import CardsView from './components/CardsView.vue';
import {tableData} from './data/data'
export default {
  data() {
    return {
      tableData: [],
      currentPage: 1,
      itemsPerPage: 10,
      activeView: 'table',
      searchQuery: '',
      searchResults: [],
      isFormVisible: false,
      newItem: {
        date: '',
        importance: '',
        equipment: '',
        message: '',
        responsible: '',
        read: false,
      },
      editedItem: null,
      isEditFormVisible: false,
      editItemIndex: null,
    };
  },
  components: {
    TableView,
    CardsView,
  },
  created() {
    this.tableData = tableData;
  },
  computed: {
    searchHandler(){
      return this.tableData.filter(elem => {
        return elem.message.includes(this.searchQuery);
      })
    },  
    filteredTableData() {
      const startIndex = (this.currentPage - 1) * this.itemsPerPage;
      const endIndex = startIndex + this.itemsPerPage;
      return this.tableData.slice(startIndex, endIndex);
    },
    totalPages() {
      return Math.ceil(this.tableData.length / this.itemsPerPage);
    },
  },
  methods: {
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },
    deleteItem(index) {
      this.tableData.splice(index, 1);
    },
    switchView(view) {
      this.activeView = view;
    },
    search() {
      const query = this.searchQuery.toLowerCase();
      this.filteredData = this.tableData.filter(item =>
      item.message.toLowerCase().includes(query));
      if (this.filteredData.length === 0) {
        alert('Текст не найден');
      }
    },
    showForm() {
      this.isFormVisible = true;
    },
    editItem(index) {
      this.editedItem = { ...this.tableData[index] };
      this.isEditFormVisible = true;
      this.editItemIndex = index;
    },
    closeEditForm() {
      this.isEditFormVisible = false;
    },
    saveEditedItem() {
      const index = this.editItemIndex
      if (index !== -1) {
        this.tableData[index] = { ...this.editedItem };
        this.closeEditForm();
      }
    },
    closeForm() {
      this.isFormVisible = false;
    },
    addNewItem() {
      this.tableData.push({ ...this.newItem });
      this.newItem = {
        date: '',
        importance: '',
        equipment: '',
        message: '',
        responsible: '',
        read: false,
      };
      this.isFormVisible = false;
    },
  },
};
</script>

<style>

header {
  display: flex;
  justify-content: space-between;
  padding: 10px;
  background-color: #f0f0f0;
}

.header-left button, .header-right button {
  margin-left: 10px;
}

.active {
  font-weight: bold;
}

.content {
  margin: 20px;
}

.modal {
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgba(0, 0, 0, 0.4);
}

.modal-content {
  background-color: #fefefe;
  margin: 15% auto; 
  padding: 20px;
  border: 1px solid #888;
  width: 60%;
  box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.2);
  position: relative;
  text-align: center;
}

.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
  cursor: pointer;
}

.form-group {
  margin-bottom: 20px;
}

label {
  display: block;
  margin-bottom: 5px;
}

.buttons {
  display: flex;
  justify-content: space-between;
}

.button {
  padding: 10px 20px;
  border: none;
  cursor: pointer;
}

.active-button {
  background-color: #007bff;
  color: #fff;
}

.inactive-button {
  background-color: #ccc;
  color: #666;
}

.add-button {
  background-color: #28a745;
  color: #fff;
  margin-top: 10px;
}

.left-margin {
  margin-left: 20px;
}

.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}

.pagination button {
  padding: 5px 10px;
  border: none;
  cursor: pointer;
  margin: 0 5px;
}

.pagination button:disabled {
  background-color: #ccc;
  color: #666;
  cursor: not-allowed;
}
</style>
