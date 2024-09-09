<template>
  <div class="row mb-4">
    <div class="col-12">
      <div class="card shadow-sm">
        <div class="card-header category bg-dark text-white d-flex justify-content-between align-items-center">
          <!-- Header Title -->
          <h5 class="m-0">Order Management</h5>

          <div>
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

          <!-- Search and Filter Controls -->
          <div class="row mb-3">
            <div class="col-md-9">
              <input type="text" v-model="searchQuery" class="form-control"
                placeholder="Search by ID, Customer Name, or Order Details">
            </div>
            <div class="col-md-3">
              <button class="btn btn-secondary w-100" @click="toggleFilters">
                Filter Orders
              </button>
            </div>
          </div>

          <!-- Filter Row -->
          <div v-if="filterVisibility" class="row mb-3">
            <div class="col-md-3">
              <label for="filterStatus">Filter by Order Status:</label>
              <select v-model="filterStatus" class="form-control">
                <option value="">All</option>
                <option value="Scheduled">Scheduled</option>
                <option value="In Transit">In Transit</option>
                <option value="Delivered">Delivered</option>
              </select>
            </div>
            <div class="col-md-3">
              <label for="filterScheduledDate">Scheduled Date & Time:</label>
              <input type="datetime-local" v-model="filterScheduledDate" class="form-control"
                placeholder="Scheduled Date & Time">
            </div>
            <div class="col-md-3">
              <label for="filterUpdatedDate">Updated Status Date & Time:</label>
              <input type="datetime-local" v-model="filterUpdatedDate" class="form-control"
                placeholder="Updated Status Date & Time">
            </div>
            <div class="col-md-3 d-flex align-items-end mt-2">
              <button class="btn btn-secondary w-100" @click="resetFilters">Reset Filters</button>
            </div>
          </div>

          <!-- Table -->
          <table class="table table-striped table-bordered w-100">
            <thead class="thead-dark">
              <tr>
                <th>ID</th>
                <th>Customer <br>Name</th>
                <th>Order <br> Details</th>
                <th>Shipping <br>Address</th>
                <th>Order <br> Status</th>
                <th>Scheduled <br> Date & Time</th>
                <th>Updated <br> Date & Time</th>
                <th>Actions</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="order in filteredOrders" :key="order.id">
                <td>{{ order.id }}</td>
                <td>{{ order.customerName }}</td>
                <td>
                  <div v-for="(detail, index) in order.orderDetails" :key="index">
                    {{ detail }}
                  </div>
                </td>
                <td>{{ order.shippingAddress }}</td>
                <td>
                  <select v-model="order.orderStatus" class="form-control form-control-sm"
                    @change="updateStatus(order.id, order.orderStatus)">
                    <option value="Scheduled">Scheduled</option>
                    <option value="In Transit">In Transit</option>
                    <option value="Delivered">Delivered</option>
                  </select>
                </td>
                <td>{{ formatDateTime(order.scheduledDateTime) }}</td>
                <td>{{ formatDateTime(order.updatedStatusDateTime) }}</td>
                <td>
                  <button class="btn btn-sm btn-danger" @click="deleteOrder(order.id)">Delete</button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import jsPDF from "jspdf"; // Import jsPDF for generating PDF files
import "jspdf-autotable"; // Import AutoTable plugin for jsPDF

export default {
  name: "OrdersTable",
  data() {
    return {
      searchQuery: "", // Search query for filtering orders
      filterStatus: "", // Filter for order status
      filterScheduledDate: "", // Filter for scheduled date and time
      filterUpdatedDate: "", // Filter for updated status date and time
      filterVisibility: false, // Control the visibility of filters
      orders: [
        {
          id: 1,
          customerName: 'John Doe',
          orderDetails: ['Tomatoes x 2', 'Oranges x 5'],
          shippingAddress: '123 Oak Street',
          orderStatus: 'In Transit',
          scheduledDateTime: '2024-09-01T19:00:00',
          updatedStatusDateTime: '2024-09-01T19:00:00'
        },
        {
          id: 2,
          customerName: 'Jane Smith',
          orderDetails: ['Apples x 4', 'Bananas x 3', 'Grapes x 2', 'Blueberries x 1', 'Pears x 5'],
          shippingAddress: '789 Pine St',
          orderStatus: 'Scheduled',
          scheduledDateTime: '2024-09-01T10:00:00',
          updatedStatusDateTime: '2024-09-01T10:00:00'
        },
        {
          id: 3,
          customerName: 'Michael Brown',
          orderDetails: ['Milk x 2', 'Bread x 3'],
          shippingAddress: '456 Maple Ave',
          orderStatus: 'Delivered',
          scheduledDateTime: '2024-09-01T12:30:00',
          updatedStatusDateTime: '2024-09-01T12:30:00'
        },
        {
          id: 4,
          customerName: 'Emily Johnson',
          orderDetails: ['Chicken x 1', 'Eggs x 12'],
          shippingAddress: '321 Cedar Blvd',
          orderStatus: 'In Transit',
          scheduledDateTime: '2024-09-01T15:45:00',
          updatedStatusDateTime: '2024-09-01T15:45:00'
        },
        {
          id: 5,
          customerName: 'Chris Lee',
          orderDetails: ['Lettuce x 1', 'Cucumber x 2', 'Carrots x 3', 'Spinach x 1', 'Peppers x 4'],
          shippingAddress: '654 Spruce Ln',
          orderStatus: 'Scheduled',
          scheduledDateTime: '2024-09-01T09:15:00',
          updatedStatusDateTime: '2024-09-01T09:15:00'
        },
        {
          id: 6,
          customerName: 'Sophia Martinez',
          orderDetails: ['Juice x 2', 'Water x 6'],
          shippingAddress: '987 Birch Rd',
          orderStatus: 'Delivered',
          scheduledDateTime: '2024-09-01T11:00:00',
          updatedStatusDateTime: '2024-09-01T11:00:00'
        },
        {
          id: 7,
          customerName: 'James Wilson',
          orderDetails: ['Soda x 4', 'Chips x 3', 'Cookies x 5'],
          shippingAddress: '231 Elm St',
          orderStatus: 'In Transit',
          scheduledDateTime: '2024-09-01T14:20:00',
          updatedStatusDateTime: '2024-09-01T14:20:00'
        },
        {
          id: 8,
          customerName: 'Olivia Davis',
          orderDetails: ['Ice Cream x 2', 'Cake x 1'],
          shippingAddress: '369 Willow Dr',
          orderStatus: 'Scheduled',
          scheduledDateTime: '2024-09-01T08:30:00',
          updatedStatusDateTime: '2024-09-01T08:30:00'
        },
        {
          id: 9,
          customerName: 'Daniel Garcia',
          orderDetails: ['Coffee x 1', 'Tea x 2', 'Sugar x 1', 'Creamer x 1', 'Honey x 2'],
          shippingAddress: '159 Aspen Ct',
          orderStatus: 'Delivered',
          scheduledDateTime: '2024-09-01T17:00:00',
          updatedStatusDateTime: '2024-09-01T17:00:00'
        },
        {
          id: 10,
          customerName: 'Isabella Rodriguez',
          orderDetails: ['Pasta x 2', 'Tomato Sauce x 1'],
          shippingAddress: '753 Walnut Way',
          orderStatus: 'Scheduled',
          scheduledDateTime: '2024-09-01T16:00:00',
          updatedStatusDateTime: '2024-09-01T16:00:00'
        },
      ]
    };
  },
  computed: {
    filteredOrders() {
      let filtered = this.orders;

      // Apply search filter
      if (this.searchQuery) {
        const query = this.searchQuery.toLowerCase();
        filtered = filtered.filter(order =>
          order.id.toString().includes(query) ||
          order.customerName.toLowerCase().includes(query) ||
          order.orderDetails.some(detail => detail.toLowerCase().includes(query))
        );
      }

      // Apply status filter
      if (this.filterStatus) {
        filtered = filtered.filter(order => order.orderStatus === this.filterStatus);
      }

      // Apply scheduled date filter
      if (this.filterScheduledDate) {
        filtered = filtered.filter(order =>
          new Date(order.scheduledDateTime).toLocaleString('en-US', { dateStyle: 'short', timeStyle: 'short' }) ===
          new Date(this.filterScheduledDate).toLocaleString('en-US', { dateStyle: 'short', timeStyle: 'short' })
        );
      }

      // Apply updated date filter
      if (this.filterUpdatedDate) {
        filtered = filtered.filter(order =>
          new Date(order.updatedStatusDateTime).toLocaleString('en-US', { dateStyle: 'short', timeStyle: 'short' }) ===
          new Date(this.filterUpdatedDate).toLocaleString('en-US', { dateStyle: 'short', timeStyle: 'short' })
        );
      }

      return filtered;
    }
  },
  methods: {
    toggleFilters() {
      this.filterVisibility = !this.filterVisibility;
    },
    updateStatus(orderId, newStatus) {
      const order = this.orders.find(order => order.id === orderId);
      if (order) {
        order.orderStatus = newStatus;
        order.updatedStatusDateTime = new Date().toLocaleString('en-US', { dateStyle: 'short', timeStyle: 'short' });
      }
    },
    deleteOrder(orderId) {
      this.orders = this.orders.filter(order => order.id !== orderId);
    },
    generateReport(type) {
      const dataToExport = this.filteredOrders; // Use filtered data for export
      if (type === "Excel/CSV") {
        this.exportToCSV(dataToExport);
      } else if (type === "PDF") {
        this.exportToPDF(dataToExport);
      }
    },
    exportToCSV(filteredData) {
      const headers = ["Order ID", "Customer Name", "Order Details", "Shipping Address", "Order Status", "Scheduled Date & Time", "Updated Status Date & Time"];
      const rows = filteredData.map(order => [
        order.id,
        order.customerName,
        order.orderDetails.join(', '),
        order.shippingAddress,
        order.orderStatus,
        this.formatDateTime(order.scheduledDateTime),
        this.formatDateTime(order.updatedStatusDateTime)
      ]);

      let csvContent = "data:text/csv;charset=utf-8,"
        + headers.join(",") + "\n"
        + rows.map(e => e.join(",")).join("\n");

      const encodedUri = encodeURI(csvContent);
      const link = document.createElement("a");
      link.setAttribute("href", encodedUri);
      link.setAttribute("download", "orders_report.csv");
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
    },
    exportToPDF(filteredData) {
      const doc = new jsPDF();
      doc.text("Orders Report", 20, 10);
      doc.autoTable({
        head: [["Order ID", "Customer Name", "Order Details", "Shipping Address", "Order Status", "Scheduled Date & Time", "Updated Status Date & Time"]],
        body: filteredData.map(order => [
          order.id,
          order.customerName,
          order.orderDetails.join(', '),
          order.shippingAddress,
          order.orderStatus,
          this.formatDateTime(order.scheduledDateTime),
          this.formatDateTime(order.updatedStatusDateTime)
        ])
      });
      doc.save("orders_report.pdf");
    },
    resetFilters() {
      this.searchQuery = "";
      this.filterStatus = "";
      this.filterScheduledDate = "";
      this.filterUpdatedDate = "";
    },
    formatDateTime(dateTime) {
      return new Date(dateTime).toLocaleString('en-US', { dateStyle: 'short', timeStyle: 'short' });
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
</style>
