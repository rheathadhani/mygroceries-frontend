<template>
    <div class="dashboard-container container">
      <!-- Top Row: Welcome Admin Card, Notifications Button, Profile Dropdown, Date & Time -->
      <div class="row mb-4 align-items-center">
        <!-- Welcome Admin Card -->
        <div class="col-md-8">
          <div class="card shadow-sm">
            <div class="card-header bg-dark"></div>
            <div class="card-body">
              <h4 class="card-title">Welcome, <strong>Admin</strong></h4>
              <p class="card-text">Manage your dashboard effectively.</p>
            </div>
          </div>
        </div>
  
        <!-- Notifications Button, Profile Dropdown, and Date & Time -->
        <div class="col-md-4 d-flex flex-column align-items-end">
          <div class="d-flex mb-2">
            <!-- Notifications Button -->
            <button class="btn btn-dark me-2" @click="toggleNotifications">
              <i class="fas fa-bell"></i> Notifications
            </button>
  
            <!-- Profile Dropdown -->
            <div class="dropdown">
              <button class="btn btn-dark dropdown-toggle d-flex align-items-center" type="button"
                id="profileDropdown" data-bs-toggle="dropdown" aria-expanded="false">
                <i class="fas fa-user-circle me-2"></i> Profile
              </button>
              <ul class="dropdown-menu dropdown-menu-end" aria-labelledby="profileDropdown">
                <li><a class="dropdown-item" href="#">Logout</a></li>
              </ul>
            </div>
          </div>
  
          <!-- Date and Time -->
          <div class="current-time text-center mt-2 p-2 border rounded shadow-sm">
            <div class="pb-2">{{ currentDate }}</div>
            <div>{{ currentTime }}</div>
          </div>

        </div>
      </div>
  
      <!-- Line Chart for Sales Over Time -->
      <div class="row mb-3">
        <div class="col-12">
          <SalesOverTimeChart />
        </div>
      </div>
  
      <!-- Cards Row -->
      <div class="row mb-4">
        <div class="col-md-3">
          <DashboardCard title="Total Customers" :value="totalCustomers" />
        </div>
        <div class="col-md-3">
          <DashboardCard title="Total Items Available" :value="totalItemsAvailable" />
        </div>
        <div class="col-md-3">
          <DashboardCard title="Total Revenue" :value="totalRevenue" />
        </div>
        <div class="col-md-3">
          <DashboardCard title="Total Orders" :value="totalOrders" />
        </div>
      </div>
  
      <!-- Product Performance Card -->
      <ProductPerformanceChart />
  
      <!-- Two Tables Row -->
      <div class="row mb-4">
        <!-- First Half: Sales by Category Table -->
        <div class="col-md-6">
          <SalesByCategoryTable :salesData="salesByCategoryData" />
        </div>
        <!-- Second Half: Number of Available Items Table -->
        <div class="col-md-6">
          <NumberOfAvailableItemsTable :itemsData="numberOfAvailableItemsData" />
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import DashboardCard from '../components/DashboardCard.vue';
  import SalesOverTimeChart from '../components/SalesOverTimeChart.vue';
  import ProductPerformanceChart from '../components/ProductPerformanceChart.vue';
  import SalesByCategoryTable from '../components/SalesByCategoryTable.vue';
  import NumberOfAvailableItemsTable from '../components/NumberOfAvailableItemsTable.vue';
  
  export default {
    name: 'A_Dashboard',
    components: {
      DashboardCard,
      SalesOverTimeChart,
      ProductPerformanceChart,
      SalesByCategoryTable,
      NumberOfAvailableItemsTable,
    },
    methods: {
      toggleNotifications() {
        // Logic to show/hide notifications list
      },
    },
    data() {
      return {
        totalCustomers: 100,
        totalRevenue: 1000,
        totalOrders: 1000,
        totalItemsAvailable: 500,
        salesByCategoryData: [
          { name: 'Dairy', sales: 1500 },
          { name: 'Beverages', sales: 1200 },
          { name: 'Snacks', sales: 900 },
          { name: 'Bakery', sales: 800 },
          { name: 'Produce', sales: 500 }
        ],
        numberOfAvailableItemsData: [
          { name: 'Lemon', quantity: 400 },
          { name: 'Apple', quantity: 350 },
          { name: 'Banana', quantity: 300 },
          { name: 'Orange', quantity: 250 },
          { name: 'Strawberry', quantity: 200 }
        ],
        currentDate: new Date().toLocaleDateString('en-US', { weekday: 'long', day: 'numeric', month: 'long' }),
        currentTime: new Date().toLocaleTimeString('en-US', { hour: '2-digit', minute: '2-digit' }),
      };
    },
  };
  </script>
  
  <style scoped>
  .dashboard-container {
    padding: 20px;
    background-color: #f8f9fa;
    border-radius: 8px;
  }
  
  .card-header {
    height: 4px;
  }
  
  .btn-dark {
    display: flex;
    align-items: center;
  }
  
  .me-2 {
    margin-right: 0.5rem; /* Spacing between buttons */
  }
  
  .current-time {
    font-size: 0.9em;
    color: #333;
    line-height: 1.5;
    width: 100%; /* Ensure full width */
    height: 12vh;
    background-color: #fff;
  }
  </style>
  