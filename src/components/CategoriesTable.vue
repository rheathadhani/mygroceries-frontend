<template>
  <div class="row mb-4">
    <div class="col-12">
      <div class="card shadow-sm">
        <div class="card-header category bg-dark text-white d-flex justify-content-between align-items-center">
          <!-- Header Title -->
          <h5 class="m-0">Category Management</h5>
          
          <div>
            <!-- Generate Report Dropdown -->
            <div class="dropdown d-inline me-2">
              <ul class="dropdown-menu" aria-labelledby="generateReportDropdown">
                <li><a class="dropdown-item" href="#" @click="generateReport('Excel/CSV')">Excel/CSV</a></li>
                <li><a class="dropdown-item" href="#" @click="generateReport('PDF')">PDF</a></li>
              </ul>
              <button class="btn btn-sm btn-success dropdown-toggle" type="button" id="generateReportDropdown"
                data-bs-toggle="dropdown" aria-expanded="false">
                Generate Report
              </button>
            </div>

            <!-- Add New Category Button -->
            <button class="btn btn-sm btn-success mr-0" @click="showAddCategory">Add New Category</button>
          </div>
        </div>
        <div class="card-body">
          <div v-if="!showAddCategoryComponent">
            <!-- Search and Filter Controls -->
            <div class="row mb-3">
              <div class="col-md-9">
                <input type="text" v-model="searchQuery" class="form-control" placeholder="Search by name">
              </div>
              <div class="col-md-3">
                <!-- Button to toggle Filter Categories row -->
                <button class="btn btn-secondary w-100" @click="toggleFilters">Filter Categories</button>
              </div>
            </div>

            <!-- Filter Row (Visible based on filterVisibility) -->
            <div v-if="filterVisibility" class="row mb-3">
              <div class="col-md-2">
                <label for="operator">Filter by:</label>
                <select v-model="filterOperator" class="form-control">
                  <option value="all">All</option>
                  <option value="less">Less than</option>
                  <option value="more">More than</option>
                  <option value="equal">Equal to</option>
                </select>
              </div>
              <div class="col-md-4">
                <label for="numProducts">Number of Products:</label>
                <input type="number" v-model="filterValue" class="form-control">
              </div>
              <div class="col-md-4">
                <label for="updatedTime">Updated Time:</label>
                <input type="date" v-model="filterDate" class="form-control">
              </div>
              <div class="col-md-2 d-flex align-items-end"> <!-- Modified class to align button -->
                <button class="btn btn-secondary w-100" @click="resetFilters">Reset Filters</button>
              </div>
            </div>

            <!-- Table -->
            <table class="table table-striped table-bordered w-100">
              <thead class="thead-dark">
                <tr>
                  <th>ID</th>
                  <th>Category Name</th>
                  <th>Number of Products</th>
                  <th>Updated Time</th>
                  <th>Actions</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="category in filteredCategories" :key="category.id">
                  <td>{{ category.id }}</td>
                  <td v-if="editCategoryId !== category.id">{{ category.name }}</td>
                  <td v-else>
                    <input type="text" v-model="editCategoryData.name" class="form-control form-control-sm">
                  </td>
                  <td v-if="editCategoryId !== category.id">{{ category.numProducts }}</td>
                  <td v-else>
                    <input type="number" v-model="editCategoryData.numProducts" class="form-control form-control-sm">
                  </td>
                  <td>{{ category.updatedTime }}</td>
                  <td>
                    <div v-if="editCategoryId !== category.id">
                      <button class="btn btn-sm btn-primary me-2" @click="editCategory(category)">Edit</button>
                      <button class="btn btn-sm btn-danger" @click="deleteCategory(category.id)">Delete</button>
                    </div>
                    <div v-else>
                      <button class="btn btn-sm btn-success me-2" @click="saveCategory(category.id)">Save</button>
                      <button class="btn btn-sm btn-secondary" @click="cancelEdit">Cancel</button>
                    </div>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>

          <!-- Add New Category Component -->
          <div v-if="showAddCategoryComponent">
            <AddNewCategory @save-category="addCategory" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import AddNewCategory from './AddNewCategory.vue'; // Import the AddNewCategory component
import jsPDF from "jspdf"; // Import jsPDF for generating PDF files
import "jspdf-autotable"; // Import AutoTable plugin for jsPDF

export default {
  name: "CategoriesTable",
  components: {
    AddNewCategory, // Register the AddNewCategory component
  },
  data() {
    return {
      showAddCategoryComponent: false, // State to control the visibility of AddNewCategory component
      filterVisibility: false, // State to control the visibility of the filter row
      editCategoryId: null, // ID of the category being edited
      editCategoryData: {}, // Temporary data for the category being edited
      searchQuery: "", // Search query for filtering categories by name
      filterOperator: "all", // Filter operator for number of products
      filterValue: "", // Value for filtering by number of products
      filterDate: "", // Date for filtering by updated time
      categories: [
        { id: 1, name: 'Electronics', numProducts: 120, updatedTime: '2024-09-01 10:00 AM' },
        { id: 2, name: 'Groceries', numProducts: 200, updatedTime: '2024-09-01 11:30 AM' },
        { id: 3, name: 'Clothing', numProducts: 85, updatedTime: '2024-09-01 01:15 PM' },
        { id: 4, name: 'Books', numProducts: 50, updatedTime: '2024-09-01 02:45 PM' },
        { id: 5, name: 'Toys', numProducts: 40, updatedTime: '2024-09-01 03:30 PM' },
        { id: 6, name: 'Furniture', numProducts: 70, updatedTime: '2024-09-01 04:10 PM' },
        { id: 7, name: 'Health & Beauty', numProducts: 130, updatedTime: '2024-09-01 05:00 PM' },
        { id: 8, name: 'Sports', numProducts: 90, updatedTime: '2024-09-01 05:45 PM' },
        { id: 9, name: 'Automotive', numProducts: 65, updatedTime: '2024-09-01 06:30 PM' },
        { id: 10, name: 'Garden', numProducts: 55, updatedTime: '2024-09-01 07:00 PM' },
      ]
    };
  },
  computed: {
    filteredCategories() {
      let filtered = this.categories;

      // Apply search filter by name
      if (this.searchQuery) {
        filtered = filtered.filter(category => category.name.toLowerCase().includes(this.searchQuery.toLowerCase()));
      }

      // Apply filter by number of products
      if (this.filterOperator !== "all" && this.filterValue) {
        if (this.filterOperator === "less") {
          filtered = filtered.filter(category => category.numProducts < parseInt(this.filterValue));
        } else if (this.filterOperator === "more") {
          filtered = filtered.filter(category => category.numProducts > parseInt(this.filterValue));
        } else if (this.filterOperator === "equal") {
          filtered = filtered.filter(category => category.numProducts === parseInt(this.filterValue));
        }
      }

      // Apply filter by updated time
      if (this.filterDate) {
        filtered = filtered.filter(category => new Date(category.updatedTime).toLocaleDateString() === new Date(this.filterDate).toLocaleDateString());
      }

      return filtered;
    }
  },
  methods: {
    showAddCategory() {
      this.showAddCategoryComponent = true; // Show the AddNewCategory component
    },
    toggleFilters() {
      this.filterVisibility = !this.filterVisibility; // Toggle filter row visibility
    },
    addCategory(newCategory) {
      this.categories.push({
        id: this.categories.length + 1,
        ...newCategory,
        updatedTime: new Date().toLocaleString('en-US', { dateStyle: 'short', timeStyle: 'short' })
      });
      this.showAddCategoryComponent = false; // Hide the AddNewCategory component
    },
    deleteCategory(categoryId) {
      this.categories = this.categories.filter(category => category.id !== categoryId);
    },
    editCategory(category) {
      this.editCategoryId = category.id; // Set the category ID being edited
      this.editCategoryData = { ...category }; // Create a copy of the category data for editing
    },
    saveCategory(categoryId) {
      const index = this.categories.findIndex(category => category.id === categoryId);
      if (index !== -1) {
        this.categories[index].name = this.editCategoryData.name;
        this.categories[index].numProducts = this.editCategoryData.numProducts;
        this.categories[index].updatedTime = new Date().toLocaleString('en-US', { dateStyle: 'short', timeStyle: 'short' });
      }
      this.editCategoryId = null; // Reset the edit state
      this.editCategoryData = {};
    },
    cancelEdit() {
      this.editCategoryId = null; // Reset the edit state
      this.editCategoryData = {};
    },
    generateReport(type) {
      const dataToExport = this.filteredCategories; // Use filtered data for export
      if (type === "Excel/CSV") {
        this.exportToCSV(dataToExport);
      } else if (type === "PDF") {
        this.exportToPDF(dataToExport);
      }
    },
    exportToCSV(filteredData) {
      const headers = ["ID", "Name", "Number of Products", "Updated Time"];
      const rows = filteredData.map(category => [
        category.id,
        category.name,
        category.numProducts,
        category.updatedTime
      ]);

      let csvContent = "data:text/csv;charset=utf-8,"
        + headers.join(",") + "\n"
        + rows.map(e => e.join(",")).join("\n");

      const encodedUri = encodeURI(csvContent);
      const link = document.createElement("a");
      link.setAttribute("href", encodedUri);
      link.setAttribute("download", "categories_report.csv");
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
    },
    exportToPDF(filteredData) {
      const doc = new jsPDF();
      doc.text("Categories Report", 20, 10);
      doc.autoTable({
        head: [["ID", "Name", "Number of Products", "Updated Time"]],
        body: filteredData.map(category => [
          category.id,
          category.name,
          category.numProducts,
          category.updatedTime
        ])
      });
      doc.save("categories_report.pdf");
    },
    resetFilters() {
      this.searchQuery = "";
      this.filterOperator = "all";
      this.filterValue = "";
      this.filterDate = "";
    }
  },
};
</script>

<style scoped>
.category {
  height: 7vh;
}

.table {
  margin-top: 10px;
}

.text-center {
  text-align: center;
}

.card-body {
  padding: 1rem;
  /* Adjusted padding */
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.btn-success {
  margin-right: 10px;
}
</style>
