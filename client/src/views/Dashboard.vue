<template>
  <div>
    <!-- Navbar -->
    <nav class="navbar navbar-expand-lg navbar-light bg-light shadow">
      <a class="navbar-brand text-muted" href="#">Kanban</a>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav">
          <li class="nav-item active">
            <p class="text-muted">{{ active_user }}</p>
          </li>
          <li class="nav-item active">
            <a class="nav-link text-muted" href="#" @click="logout">Logout</a>
          </li>
        </ul>
      </div>
    </nav>

    <div class="container mt-2">
    <a @click.prevent="toggleAddCategory" href="#" class="btn-sm btn-secondary">+ Add Category</a>
    </div>
      <form @submit.prevent="addCategory" v-if="isAddCategory">
        <div class="form-group row mt-3">
          <div class="col-md-4">
            <input type="text" class="form-control" placeholder="Category Name" v-model="name" required />
          </div>
        </div>
        <div class="form-group row">
          <div class="col-md-5 ml-0">
            <button type="submit" class="btn btn-secondary">Submit</button>
          </div>
        </div>
      </form>

    <section class="container-fluid d-flex justify-content-around mt-5">
      <CategoryCard 
        v-for="category in categoriesData"
        :category="category"
        :tasksData="tasksData"
        :key="category.id"
        :isEdit="isEdit"
        :categoriesData="categoriesData"
        @addTaskSubmit="addTaskSubmit"
        @deleteClick="deleteClick"
        @editClick="editClick"
        @deleteCategory="deleteCategory"
        >
      </CategoryCard>
    </section>

    <!-- Footer -->
    <footer>
      <p class="mt-5 mb-3 text-muted text-center">Created by Full-Stuck Developer &copy; 2020</p>
    </footer>
  </div>
</template>

<script>
import CategoryCard from "../components/categoryCard";
export default {
  name: "Dashboard",
  props: ["tasksData", "categoriesData", "active_user", "isEdit"],
  components: {
    CategoryCard
  },
  data() {
    return {
      isAddCategory: false,
      name: ""
    };
  },
  methods: {
    logout() {
      localStorage.clear();
      this.$emit("backToLogin");
    },
    addTaskSubmit(payload) {
      this.$emit("addTaskSubmit", payload);
    },
    deleteClick(deleteId) {
      this.$emit("deleteClick", deleteId);
    },
    editClick(editId, payload) {
      this.$emit("editClick", editId, payload);
    },
    toggleAddCategory() {
      if (this.isAddCategory) {
        return this.isAddCategory = false;
      }
      this.isAddCategory = true;
    },
    addCategory() {
      let payload = {
        name: this.name
      };
      this.$emit("addCategory", payload);
      this.name = "";
      this.isAddCategory = false;
    },
    deleteCategory(deleteId) {
      this.$emit("deleteCategory", deleteId);
    }
  }
}
</script>

<style>

</style>