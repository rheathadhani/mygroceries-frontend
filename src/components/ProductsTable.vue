<template>
  <!-- Table View -->
  <div v-if="!showDetails && !showAllDetails && !showAddNew" class="row mb-4">
    <div class="col-12">
      <div class="card shadow-sm">
        <div class="card-header category bg-dark text-white d-flex justify-content-between align-items-center">
          <h5 class="m-0">Product Management</h5>
          <div>
            <!-- Add New Product Button -->
            <button class="btn btn-sm btn-success me-2" @click="showAddNewProductForm">Add New Product</button>

            <!-- Generate Report Dropdown -->
            <div class="dropdown d-inline me-2">
              <button class="btn btn-sm btn-success dropdown-toggle" type="button" id="generateReportDropdown"
                data-bs-toggle="dropdown" aria-expanded="false">
                Generate Report
              </button>
              <ul class="dropdown-menu" aria-labelledby="generateReportDropdown">
                <li><a class="dropdown-item" href="#" @click="generateReport('Excel/CSV')">Excel/CSV</a></li>
                <li><a class="dropdown-item" href="#" @click="generateReport('PDF')">PDF</a></li>
              </ul>
            </div>
          </div>
        </div>

        <div class="card-body">
          <!-- Search and Toggle Filters Controls -->
          <div class="row mb-3">
            <div class="col-md-9">
              <input type="text" v-model="searchQuery" class="form-control"
                placeholder="Search by ID, Name, Price, or Description">
            </div>
            <div class="col-md-3">
              <button class="btn btn-secondary w-100" @click="toggleFilters">
                Filter Products
              </button>
            </div>
          </div>

          <!-- Filters Row (Toggle Visibility) -->
          <div v-if="filterVisibility" class="row mb-3">
            <div class="col-md-3">
              <label for="filterCategory">Filter by Category:</label>
              <select id="filterCategory" v-model="filterCategory" class="form-control">
                <option value="">All</option>
                <option v-for="category in categories" :key="category">{{ category }}</option>
              </select>
            </div>
            <div class="col-md-3">
              <label for="filterPrice">Filter by Price:</label>
              <div class="input-group">
                <select v-model="filterPriceCondition" class="form-select form-control">
                  <option value="lt">Less Than</option>
                  <option value="eq">Equal To</option>
                  <option value="gt">Greater Than</option>
                </select>
                <input type="number" v-model="filterPrice" class="form-control" placeholder="Price">
              </div>
            </div>
            <div class="col-md-3">
              <label for="filterDate">Filter by Created Date:</label>
              <input type="date" v-model="filterDate" class="form-control">
            </div>
            <div class="col-md-3 d-flex align-items-end">
              <button class="btn btn-secondary w-100" @click="resetFilters">
                Reset Filters
              </button>
            </div>
          </div>

          <!-- Table -->
          <table class="table table-striped table-bordered w-100">
            <thead class="thead-dark">
              <tr>
                <th>ID</th>
                <th>Name</th>
                <th>Price</th>
                <th>Category</th>
                <th>Updated Date</th>
                <th>Actions</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="product in filteredProducts" :key="product.id">
                <td>{{ product.id }}</td>
                <td>{{ product.name }}</td>
                <td>{{ product.price }}</td>
                <td>{{ product.category }}</td>
                <td>{{ formatDate(product.updatedDate) }}</td>
                <td>
                  <button class="btn btn-sm btn-warning me-2" @click="viewDetails(product)">View Details</button>
                  <button class="btn btn-sm btn-primary me-2" @click="editProduct(product)">Edit</button>
                  <button class="btn btn-sm btn-danger" @click="deleteProduct(product.id)">Delete</button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>

  <!-- Add New Product Form -->
  <div v-if="showAddNew" class="row mb-4">
    <div class="col-12">
      <div class="card shadow-sm">
        <div class="card-header category bg-dark text-white d-flex justify-content-between align-items-center">
          <h5 class="m-0">Add New Product</h5>
        </div>
        <div class="card-body">
          <form @submit.prevent="saveNewProduct">
            <div class="mb-3">
              <label for="newProductName" class="form-label">Name</label>
              <input type="text" id="newProductName" v-model="newProduct.name" class="form-control" required>
            </div>
            <div class="mb-3">
              <label for="newProductPrice" class="form-label">Price</label>
              <input type="number" id="newProductPrice" v-model="newProduct.price" class="form-control" required>
            </div>
            <div class="mb-3">
              <label for="newProductCategory" class="form-label">Category</label>
              <select id="newProductCategory" v-model="newProduct.category" class="form-control" required>
                <option v-for="category in categories" :key="category" :value="category">{{ category }}</option>
              </select>
            </div>
            <div class="mb-3">
              <label for="newProductDescription" class="form-label">Description</label>
              <textarea id="newProductDescription" v-model="newProduct.description" class="form-control" required></textarea>
            </div>
            <div class="mb-3">
              <label for="newProductImage" class="form-label">Image</label>
              <input type="file" id="newProductImage" @change="onNewImageChange" class="form-control" required>
            </div>
            <button type="submit" class="btn btn-success me-2">Save</button>
            <button type="button" class="btn btn-secondary" @click="cancelAddNewProduct">Cancel</button>
          </form>
        </div>
      </div>
    </div>
  </div>

  <!-- Product Edit Form -->
  <ProductEdit v-if="showDetails" :product="selectedProduct" :categories="categories" @back-to-table="backToTable"
    @save-product="saveProduct" />

  <!-- Product All Details View -->
  <ProductAllDetails v-if="showAllDetails" :product="selectedProduct" @back-to-table="backToTable" />
</template>

<script>
import jsPDF from "jspdf"; // Import jsPDF for generating PDF files
import "jspdf-autotable"; // Import AutoTable plugin for jsPDF
import ProductEdit from './ProductEdit.vue'; // Import ProductEdit component
import ProductAllDetails from './ProductAllDetails.vue'; // Import ProductAllDetails component

export default {
  name: "ProductsTable",
  components: {
    ProductEdit,
    ProductAllDetails,
  },
  data() {
    return {
      searchQuery: "", // Search query for filtering products
      filterCategory: "", // Filter for category
      filterPriceCondition: "eq", // Filter condition (lt, eq, gt)
      filterPrice: null, // Filter for price
      filterDate: "", // Filter for created date
      filterVisibility: false, // Control the visibility of filters
      showDetails: false, // Control the visibility of the edit view
      showAllDetails: false, // Control the visibility of the details view
      showAddNew: false, // Control the visibility of the add new product form
      selectedProduct: null, // Store selected product for details view
      newProduct: {
        name: "",
        price: null,
        category: "",
        description: "",
        image: null,
        createdDate: new Date().toISOString(),
        updatedDate: new Date().toISOString(),
      }, // Store data for new product
      products: [
        { id: 1, image: 'path/to/image1.jpg', name: 'Product 1', price: 100, category: 'Category A', description: 'Description for product 1', createdDate: '2024-09-01T19:00:00', updatedDate: '2024-09-02T15:00:00' },
        { id: 2, image: 'path/to/image2.jpg', name: 'Product 2', price: 150, category: 'Category B', description: 'Description for product 2', createdDate: '2024-08-30T14:30:00', updatedDate: '2024-09-01T10:00:00' },
        // More products...
      ],
      categories: ['Category A', 'Category B', 'Category C'], // Categories list
    };
  },
  computed: {
    filteredProducts() {
      let filtered = this.products;

      // Apply search filter by ID, Name, Price, or Description
      if (this.searchQuery) {
        const query = this.searchQuery.toLowerCase();
        filtered = filtered.filter(product =>
          product.id.toString().includes(query) ||
          product.name.toLowerCase().includes(query) ||
          product.price.toString().includes(query) ||
          product.description.toLowerCase().includes(query)
        );
      }

      // Apply category filter
      if (this.filterCategory) {
        filtered = filtered.filter(product => product.category === this.filterCategory);
      }

      // Apply price filter
      if (this.filterPrice !== null) {
        if (this.filterPriceCondition === "lt") {
          filtered = filtered.filter(product => product.price < this.filterPrice);
        } else if (this.filterPriceCondition === "eq") {
          filtered = filtered.filter(product => product.price === this.filterPrice);
        } else if (this.filterPriceCondition === "gt") {
          filtered = filtered.filter(product => product.price > this.filterPrice);
        }
      }

      // Apply created date filter
      if (this.filterDate) {
        filtered = filtered.filter(product => new Date(product.createdDate).toLocaleDateString() === new Date(this.filterDate).toLocaleDateString());
      }

      return filtered;
    }
  },
  methods: {
    toggleFilters() {
      this.filterVisibility = !this.filterVisibility; // Toggle filter row visibility
    },
    showAddNewProductForm() {
      this.showAddNew = true; // Show the Add New Product form
      this.showDetails = false;
      this.showAllDetails = false;
    },
    saveNewProduct() {
      // Generate a new product ID
      const newId = this.products.length ? Math.max(...this.products.map(p => p.id)) + 1 : 1;
      this.newProduct.id = newId;
      this.newProduct.updatedDate = new Date().toISOString();

      // Add the new product to the list
      this.products.push({ ...this.newProduct });

      // Reset new product form
      this.newProduct = {
        name: "",
        price: null,
        category: "",
        description: "",
        image: null,
        createdDate: new Date().toISOString(),
        updatedDate: new Date().toISOString(),
      };

      this.showAddNew = false; // Go back to the table view
    },
    onNewImageChange(event) {
      const file = event.target.files[0];
      if (file) {
        this.newProduct.image = URL.createObjectURL(file); // Update image preview for new product
      }
    },
    cancelAddNewProduct() {
      this.showAddNew = false; // Go back to the table without adding a new product
    },
    backToTable() {
      this.showDetails = false;
      this.showAddNew = false;
      this.showAllDetails = false; // Back to table view
    },
    viewDetails(product) {
      this.selectedProduct = product;
      this.showAllDetails = true; // Show detailed view
      this.showDetails = false; // Ensure edit view is hidden
    },
    editProduct(product) {
      this.selectedProduct = { ...product }; // Copy product data for editing
      this.showDetails = true; // Show edit view
      this.showAllDetails = false; // Ensure details view is hidden
    },
    saveProduct(updatedProduct) {
      const index = this.products.findIndex(p => p.id === updatedProduct.id);
      if (index !== -1) {
        this.products.splice(index, 1, updatedProduct); // Replace the old product with the updated one
        this.showDetails = false; // Hide edit view after saving
      }
    },
    deleteProduct(productId) {
      this.products = this.products.filter(product => product.id !== productId);
    },
    resetFilters() {
      this.filterCategory = "";
      this.filterPriceCondition = "eq";
      this.filterPrice = null;
      this.filterDate = "";
    },
    generateReport(type) {
      const dataToExport = this.filteredProducts; // Use filtered data for export
      if (type === "Excel/CSV") {
        this.exportToCSV(dataToExport);
      } else if (type === "PDF") {
        this.exportToPDF(dataToExport);
      }
    },
    exportToCSV(filteredData) {
      const headers = ["ID", "Name", "Price", "Category", "Description", "Created Date", "Updated Date"];
      const rows = filteredData.map(product => [
        product.id,
        product.name,
        product.price,
        product.category,
        product.description,
        this.formatDate(product.createdDate),
        this.formatDate(product.updatedDate)
      ]);

      let csvContent = "data:text/csv;charset=utf-8,"
        + headers.join(",") + "\n"
        + rows.map(e => e.join(",")).join("\n");

      const encodedUri = encodeURI(csvContent);
      const link = document.createElement("a");
      link.setAttribute("href", encodedUri);
      link.setAttribute("download", "products_report.csv");
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
    },
    exportToPDF(filteredData) {
      const doc = new jsPDF();
      doc.text("Products Report", 20, 10);
      doc.autoTable({
        head: [["ID", "Name", "Price", "Category", "Description", "Created Date", "Updated Date"]],
        body: filteredData.map(product => [
          product.id,
          product.name,
          product.price,
          product.category,
          product.description,
          this.formatDate(product.createdDate),
          this.formatDate(product.updatedDate)
        ])
      });
      doc.save("products_report.pdf");
    },
    formatDate(date) {
      return new Date(date).toLocaleString('en-US', { dateStyle: 'short', timeStyle: 'short' });
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
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.btn-success {
  margin-right: 10px;
}

.img-fluid {
  max-width: 100%;
  height: auto;
}

.row .col-md-3,
.row .col-md-9 {
  padding-bottom: 10px;
}
</style>
