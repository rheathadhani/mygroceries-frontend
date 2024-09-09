<template>
    <div class="sales-category-table card shadow-sm">
      <!-- Table Header -->
      <div class="card-header bg-dark text-white text-center">
        Sales by Category
      </div>
      <!-- Table Body -->
      <div class="card-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th scope="col">Category</th>
              <th scope="col">Sales</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(category, index) in salesData" :key="index">
              <td>{{ category.name }}</td>
              <td>{{ category.sales }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: "SalesByCategoryTable",
    props: {
      salesData: {
        type: Array,
        required: true,
        default: () => [
          { name: 'Dairy', sales: 1500 },
          { name: 'Beverages', sales: 1200 },
          { name: 'Snacks', sales: 900 },
          { name: 'Bakery', sales: 800 },
          { name: 'Produce', sales: 500 },
        ],
      },
    },
  };
  </script>
  
  <style scoped>
  .sales-category-table {
    margin-bottom: 10px; /* Add some spacing below the table */
  }
  </style>
  