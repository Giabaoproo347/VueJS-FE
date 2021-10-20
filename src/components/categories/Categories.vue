<template>
  <div class="container">
    <h3 class="title">Danh mục điện thoại</h3>
    <table  id="categories">
      <thead>
      <tr>
        <th>ID</th>
        <th>Name</th>
        <th>Slug</th>
      </tr>
      </thead>

      <tbody>
      <tr v-for="(item, index) in categories" :key="item.id">
        <td>{{index + 1}}</td>
        <td>{{item.name}}</td>
        <td>{{item.slug}}</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>


<script>
import axios from "axios";

export default {
  name: "Categories",
  data() {
    return {
      categories: null,
      isModalVisible: false,
    };
  },
  created: function () {
    axios.get("https://localhost:44328/api/getCategories").then(res => {
      this.categories = res.data;
      console.log(JSON.stringify(this.categories), 'categories')
    });
  },
  methods: {
    onUpdate: function(e){
      console.log(e)
    },
    onDelete: function(e){
      console.log(e)
    },
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

#categories tr:nth-child(even){background-color: #f2f2f2;}

#categories tr:hover {background-color: #ddd;}

#categories th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: center;
  background-color: #04AA6D;
  color: white;
}

</style>