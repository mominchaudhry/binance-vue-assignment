<template>
    <div class="addSection">
      <button v-if="hasPerms('user.list.add')" @click="showModal = true">
          Create List Info
      </button>
    </div> 
    <MyModal 
      :show="showModal"
      @close="showModal = false"
      @add-item="addItem"
    >
    </MyModal>

    <MyModal 
      :show="showEditModal"
      @close="showEditModal = false"
      @add-item="updateItem"
      :item="editItem"
    >
    </MyModal>
    <table>
      <tr>
        <th>Message ID</th>
        <th>Message Content</th>
        <th>Telegram Group</th>
        <th>Status</th>
        <th>Type</th>
        <th>Send Time</th>
        <th>Creation Time</th>
        <th>Creator</th>
        <th>Operations</th>
      </tr>
      <tr v-for="item in items" :key="item.id">
        <td>{{item.id}}</td>
        <td>{{item.content}}</td>
        <td>{{item.group}}</td>
        <td>{{item.status}}</td>
        <td>{{item.type}}</td>
        <td>{{item.time}}</td>
        <td>{{item.createTime}}</td>
        <td>{{item.creator}}</td>
        <td>
          <button v-if="hasPerms('user.list.update')" @click="edit(item.id)">Edit</button>
          <button v-if="hasPerms('user.list.delete')" @click="deleteItem(item.id)" class="delete">Delete</button>
        </td>
      </tr>
    </table>
</template>

<script>
import MyModal from './MyModal.vue'
export default {
  name: 'MyList',
  props: {
    permissions: Array
  },


  data() {
    return {
      items: this.getItems() || [],
      showModal: false,
      showEditModal: false,
      editItem: null,
    }
  },


  methods: {
    getItems() {
      const i = localStorage.getItem('items')
      if(i) {
        console.log(i)
        return JSON.parse(i)
      }
      return null
    },

    addItem(item) {
      this.items.push(item)
      localStorage.setItem('items', JSON.stringify(this.items))
      localStorage.setItem("id", item.id+1)
      this.showModal = false
    },

    edit(id) {
      this.showEditModal = true
      this.editItem = this.items.find(item => item.id == id)
    },

    updateItem(item) {
      const idx = this.items.findIndex((obj => obj.id == item.id))
      this.items[idx] = item
      localStorage.setItem('items', JSON.stringify(this.items))
      this.showModal = false
    },

    deleteItem(id) {
      this.items = this.items.filter(obj => obj.id != id)
      localStorage.setItem('items', JSON.stringify(this.items))
    },

    hasPerms(perm) {
      return this.permissions.includes(perm)
    }
  },

  
  components: {
    MyModal
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
button {
  float: left;
  margin: 10px;
  background: #f4c63c;
  border: none;
  border-radius: 5px;
  width: fit-content;
  height: 30px;
  cursor: pointer;
}
table {
  width: 100%;
  background: white;
}
th, td {
  padding: 10px;
  width: fit-content;
  max-width: 100px;
  overflow: auto;
}
td {
  background: #fafafa;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.addSection {
  width: 100%;
  height: 50px;
}
.delete {
  background: #f56c6d;
  color: white;
}
</style>
