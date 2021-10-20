<template>
  <div class="container">
    <h3 class="title">Danh sách sản phẩm</h3>
    <div class="add-new-category">
      <button id="add-category" @click="showModal()">Thêm mới sản phẩm</button>
    </div>
    <table id="categories">
      <thead>
      <tr>
        <th>ID</th>
        <th>Title</th>
        <th>Description</th>
        <th>Category</th>
        <th>Created Date</th>
        <th>Action</th>
      </tr>
      </thead>

      <tbody>
      <tr v-for="(item, index) in posts" :key="item.id">
        <td>{{ index + 1 }}</td>
        <td>{{ item.title }}</td>
        <td style="width: 400px">{{ item.description }}</td>
        <td>{{ item.categoryName }}</td>
        <td>{{ item.createdDate }}</td>
        <td>
          <div style="display: flex; justify-content: center">
            <div class="link link1" @click="onUpdate(item)">Sửa</div>
            |
            <div class="link link2" @click="onDelete(item)">Xóa</div>
          </div>
        </td>
      </tr>
      </tbody>
    </table>
  </div>

  <ModalCreate
      v-show="isModalVisible"
      @close="closeModal"
      @ok=" onSubmit"
  >
    <template v-slot:header>
      <span style="color: black; font-size: 18px">Thêm mới danh mục</span>
    </template>

    <template v-slot:body>
      <div class="flex">
        <label class="title">Title</label>
        <input v-model="title"
               style="width: 80%; outline: none; border-radius: 5px; height: 20px; border: 1px solid darkgrey">
      </div>

      <div class="flex">
        <label class="title">Description</label>
        <input v-model="description"
               style="width: 80%; outline: none; border-radius: 5px; height: 20px; border: 1px solid darkgrey">
      </div>

      <div class="flex">
        <label class="title">CategoryId</label>
        <select @change='onChange($event)' class="form-control"
                style="width: 80%; outline: none; border-radius: 5px; height: 20px; border: 1px solid darkgrey">
          <option value="" selected disabled>Chọn</option>
          <option v-for="(item) in categories" :key="item.id" v-bind:value="item.id">{{ item.name }}</option>
        </select>
      </div>
    </template>
  </ModalCreate>
</template>


<script>
// in Vue 3
import axios from 'axios'
import ModalCreate from "@/components/modal/ModalCreate";

export default {
  name: "Posts",
  components: {
    ModalCreate,
  },

  data() {
    return {
      posts: null,
      isModalVisible: false,
      title: null,
      description: null,
      categoryId: null,
      isEdit: false,
      object: null,
      categories: [
        {
          "id": 1,
          "name": "Samsung",
          "slug": "Samsung Korea",
          "posts": []
        },
        {
          "id": 2,
          "name": "Apple",
          "slug": "Apple USA",
          "posts": []
        },
        {
          "id": 3,
          "name": "Huawei",
          "slug": "Huawei China",
          "posts": []
        },
        {
          "id": 4,
          "name": "Vsmart",
          "slug": "Vsmart Vietnam",
          "posts": []
        },
        {
          "id": 5,
          "name": "Nokia",
          "slug": "Nokia Poland",
          "posts": []
        },
        {
          "id": 6,
          "name": "Oppo",
          "slug": "Oppo China",
          "posts": []
        },
        {
          "id": 7,
          "name": "LG",
          "slug": "LG Korea",
          "posts": []
        }
      ]
    };
  },
  created: function () {
    axios.get("https://localhost:44328/api/getPosts").then(res => {
      this.posts = res.data;
      console.log(JSON.stringify(this.posts), 'posts')
    });
  },
  methods: {
    onUpdate: function (e) {
      this.isModalVisible = true;
      this.isEdit = true;
      this.object = e;
    },
    onDelete: function (e) {
      const id = e.postId;
      axios.post(`https://localhost:44328/api/deletePost?postId=${id}`).then(res => {
        if (res) {
          axios.get("https://localhost:44328/api/getPosts").then(res => {
            this.posts = res.data;
          });
        }
      });
    },
    showModal() {
      this.isEdit = false;
      this.isModalVisible = true;
    },
    closeModal() {
      this.isModalVisible = false;
    },
    onChange(event) {
      console.log(event.target.value)
      this.categoryId = event.target.value
    },
    onSubmit() {
      if (this.isEdit) {

        this.title = this.object.title;
        this.description = this.object.description;
        this.categoryId = this.object.categoryId;
        const payload = {
          PostId: this.object.postId,
          Title: this.title,
          Description: this.description,
          CategoryId: this.categoryId,
          CreatedDate: "2020-09-09"
        }
        axios.post(`https://localhost:44328/api/updatePost`, payload).then(res => {
          if (res) {
            axios.get("https://localhost:44328/api/getPosts").then(res => {
              this.posts = res.data;
            });
          }
        });
      } else {
        const payload = {
          Title: this.title,
          Description: this.description,
          CategoryId: this.categoryId,
          CreatedDate: "2021-09-09"
        }
        axios.post("https://localhost:44328/api/AddPost", payload).then(res => {
          console.log(JSON.stringify(res), 'res')
          this.isModalVisible = false;
          axios.get("https://localhost:44328/api/getPosts").then(res => {
            this.posts = res.data;
          });
        });
      }
    }
  }
}
</script>

<style>

.container {
  padding: 0 50px 50px 50px;
}

#categories {
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

#categories td, #categories th {
  border: 1px solid #ddd;
  padding: 8px;
}

#categories tr:nth-child(even) {
  background-color: #f2f2f2;
}

#categories tr:hover {
  background-color: #ddd;
}

#categories th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #04AA6D;
  color: white;
}

#categories td {
  text-align: left;
}

.link {
  cursor: pointer;
  text-decoration: underline;
  color: dodgerblue;
}

.link1 {
  margin-right: 8px;
}

.link2 {
  margin-left: 8px;
}

.add-new-category {
  float: right;
  margin-bottom: 20px;
}

#add-category {
  background-color: #04AA6D;
  border-radius: 8px;
  height: 30px;
  color: white;
  outline: none;
}

.modal-backdrop {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background: #FFFFFF;
  box-shadow: 2px 2px 20px 1px;
  overflow-x: auto;
  display: flex;
  flex-direction: column;
}

.flex {
  display: flex;
  gap: 10px;
  padding-left: 20px;
  align-items: center;
  margin-top: 20px;
  margin-bottom: 20px;
}

.title {
  width: 20%;
}

.modal-header,
.modal-footer {
  padding: 15px;
  display: flex;
}

.modal-header {
  position: relative;
  border-bottom: 1px solid #eeeeee;
  color: #4AAE9B;
  justify-content: space-between;
}

.modal-footer {
  border-top: 1px solid #eeeeee;
  flex-direction: column;
  justify-content: flex-end;
}

.modal-body {
  position: relative;
  padding: 20px 10px;
}

.btn-close {
  position: absolute;
  top: 0;
  right: 0;
  border: none;
  font-size: 20px;
  padding: 10px;
  cursor: pointer;
  font-weight: bold;
  color: #4AAE9B;
  background: transparent;
}

.btn-green {
  color: white;
  background: #4AAE9B;
  border: 1px solid #4AAE9B;
  border-radius: 2px;
}
</style>