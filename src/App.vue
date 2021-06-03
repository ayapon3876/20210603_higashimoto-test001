<template>
  <div id="app">
    <div class="container">
      <div class="todo-wrap">
        <h2 class="title">Todo List</h2>
        <div class="text-wrap">
          <input
            class="text-box"
            type="text"
            name="text"
            id="text"
            v-model="newText"
          />
          <button class="add-btn" @click="insertContact">追加</button>
        </div>
        <div class="text-wrap2" v-for="item in contactLists" :key="item.id">
          <input class="text-box2" type="text" v-model="item.text" />
          <div class="btn">
            <button
              class="update-btn"
              @click="updateContact(item.id, item.text)"
            >
              更新
            </button>
            <button class="delete-btn" @click="deleteContact(item.id)">
              削除
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      newText: "",
      contactLists: [],
    };
  },
  methods: {
    async getContact() {
      const resData = await axios.get("http://127.0.0.1:8000/api/contact/");
      this.contactLists = resData.data.data;
    },
    async insertContact() {
      const sendData = {
        text: this.newText,
      };
      await axios.post("http://127.0.0.1:8000/api/contact/", sendData);
      await this.getContact();
    },
    async updateContact(id, text) {
      const sendData = {
        text: text,
      };
      await axios.put("http://127.0.0.1:8000/api/contact/" + id, sendData);
      await this.getContact();
    },
    async deleteContact(id) {
      await axios.delete("http://127.0.0.1:8000/api/contact/" + id);
      await this.getContact();
    },
  },
  created() {
    this.getContact();
  },
};
</script>

<style>
#app {
  position: relative;
  width: 100%;
  min-height: 100vh;
  background-color: #2D197C;
  display: flex;
  justify-content: center;
  align-items: center;
}
.container {
  width:50%;
  margin-right: 30px;
}
 .todo-wrap {
  background-color: white;
  width: 100%;
  height: 100%;
  margin: 0 auto;
  border-radius: 10px;
  padding: 10px 30px 30px 30px;
}
.text-wrap {
  display: flex;
  justify-content: space-between;
}
.text-wrap2 {
  display: flex;
  justify-content: space-between;
}
.text-box {
  width: 75%;
  border: 2px solid #dfdfdf;
  border-radius: 5px;
  margin-bottom: 15px;
  padding: 10px;
}
.text-box2 {
  width: 40%;
  border: 2px solid #dfdfdf;
  border-radius: 5px;
  padding: 10px;

}
.add-btn {
  cursor: pointer;
  background-color: white;
  color: #dc70fa;
  border: 3px solid;
  border-radius: 5px;
  margin-bottom: 15px;
  padding: 10px 15px;
}
.add-btn:hover {
  color: white;
  background-color: #dc70fa;
  border: 3px solid #dc70fa;
  transition: 1.0s;
}
.update-btn {
  margin-right: 5px;
  cursor: pointer;
  background-color: white;
  color: #faaa93;
  border: 3px solid;
  border-radius: 5px;
  padding: 10px 15px;
}
.update-btn:hover {
  color: white;
  background-color: #faaa93;
  border: 3px solid #faaa93;
  transition: 1.0s;
}
.delete-btn {
  cursor: pointer;
  background-color: white;
  color: #94fae0;
  border: 3px solid;
  border-radius: 5px;
  padding: 10px 15px;
}
.delete-btn:hover {
  color: white;
  background-color: #94fae0;
  border: 3px solid #94fae0;
  transition: 1.0s;
}
</style>