<template>
  <div class="container mt-4">

    <!-- Tabs for Order Status -->
    <ul class="nav nav-tabs mb-4" id="orderTabs" role="tablist">
      <li class="nav-item mr-2 mb-2" role="presentation">
        <button class="nav-link bg-dark" :class="{ 'active': activeTab === 'all', 'border-dark': activeTab === 'all' }"
          id="all-tab" type="button" role="tab" @click="setActiveTab('all')">All</button>
      </li>
      <li class="nav-item mr-2 mb-2" role="presentation">
        <button class="nav-link bg-dark"
          :class="{ 'active': activeTab === 'scheduled', 'border-dark': activeTab === 'scheduled' }" id="scheduled-tab"
          type="button" role="tab" @click="setActiveTab('scheduled')">Scheduled</button>
      </li>
      <li class="nav-item mr-2 mb-2" role="presentation">
        <button class="nav-link bg-dark"
          :class="{ 'active': activeTab === 'inTransit', 'border-dark': activeTab === 'inTransit' }" id="in-transit-tab"
          type="button" role="tab" @click="setActiveTab('inTransit')">In Transit</button>
      </li>
      <li class="nav-item mr-2 mb-2" role="presentation">
        <button class="nav-link bg-dark"
          :class="{ 'active': activeTab === 'delivered', 'border-dark': activeTab === 'delivered' }" id="delivered-tab"
          type="button" role="tab" @click="setActiveTab('delivered')">Delivered</button>
      </li>
    </ul>

    <!-- Tab Content -->
    <div class="tab-content">
      
      <!-- All Orders -->
      <div class="tab-pane fade" :class="{ 'show active': activeTab === 'all' }" id="all">
        
        <!-- Order Box -->
        <div v-for="(order, index) in allOrders" :key="index" class="order-box mb-4 p-3 border rounded">
          <!-- Dynamic Status and Time Heading -->
          <div v-if="order.status" class="d-flex justify-content-between align-items-center bg-light p-2 rounded-top">
            <span class="text-muted">{{ order.status }}</span>
            <span class="text-end">{{ order.time }}</span>
          </div>
          
          <!-- Order Items -->
          <div v-for="(item, itemIndex) in order.items" :key="itemIndex"
            class="order-item d-flex justify-content-between align-items-center mt-2">
            <div>
              <p><strong>{{ item.name }}</strong></p>
              <p>{{ item.quantity }} | RM{{ item.price }}</p>
            </div>
            <p class="text-end mb-0">RM{{ item.price }}</p>
          </div>
          <!-- Buy Again Button for the whole order -->
          <div class="text-end mt-3">
            <button class="btn btn-dark btn-sm">Buy Again</button>
          </div>
        </div>
      </div>

      <!-- Scheduled Orders -->
      <div class="tab-pane fade" :class="{ 'show active': activeTab === 'scheduled' }" id="scheduled">
        <div v-for="(order, index) in scheduledOrders" :key="index" class="order-box mb-4 p-3 border rounded">
          <div class="d-flex justify-content-between align-items-center bg-light p-2 rounded-top">
            <span class="text-muted">Scheduled for Delivery</span>
            <span class="text-end">{{ order.time }}</span>
          </div>
          <div v-for="(item, itemIndex) in order.items" :key="itemIndex"
            class="order-item d-flex justify-content-between align-items-center mt-2">
            <div>
              <p><strong>{{ item.name }}</strong></p>
              <p>{{ item.quantity }} | RM{{ item.price }}</p>
            </div>
            <p class="text-end mb-0">RM{{ item.price }}</p>
          </div>
          <div class="text-end mt-3">
            <button class="btn btn-dark btn-sm">Buy Again</button>
          </div>
        </div>
      </div>

      <!-- In Transit Orders -->
      <div class="tab-pane fade" :class="{ 'show active': activeTab === 'inTransit' }" id="in-transit">
        <div v-for="(order, index) in inTransitOrders" :key="index" class="order-box mb-4 p-3 border rounded">
          <div class="d-flex justify-content-between align-items-center bg-light p-2 rounded-top">
            <span class="text-muted">In Transit</span>
            <span class="text-end">{{ order.time }}</span>
          </div>
          <div v-for="(item, itemIndex) in order.items" :key="itemIndex"
            class="order-item d-flex justify-content-between align-items-center mt-2">
            <div>
              <p><strong>{{ item.name }}</strong></p>
              <p>{{ item.quantity }} | RM{{ item.price }}</p>
            </div>
            <p class="text-end mb-0">RM{{ item.price }}</p>
          </div>
          <div class="text-end mt-3">
            <button class="btn btn-dark btn-sm">Buy Again</button>
          </div>
        </div>
      </div>

      <!-- Delivered Orders -->
      <div class="tab-pane fade" :class="{ 'show active': activeTab === 'delivered' }" id="delivered">
        <div v-for="(order, index) in deliveredOrders" :key="index" class="order-box mb-4 p-3 border rounded">
          <div class="d-flex justify-content-between align-items-center bg-light p-2 rounded-top">
            <span class="text-muted">Delivered</span>
            <span class="text-end">{{ order.time }}</span>
          </div>
          <div v-for="(item, itemIndex) in order.items" :key="itemIndex"
            class="order-item d-flex justify-content-between align-items-center mt-2">
            <div>
              <p><strong>{{ item.name }}</strong></p>
              <p>{{ item.quantity }} | RM{{ item.price }}</p>
            </div>
            <p class="text-end mb-0">RM{{ item.price }}</p>
          </div>
          <div class="text-end mt-3">
            <button class="btn btn-dark btn-sm">Buy Again</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "MyOrders",
  data() {
    return {
      activeTab: 'all', // Set default active tab to 'all'
      allOrders: [
        {
          status: 'Scheduled',
          time: '12/10/2024, 10:00 AM',
          items: [
            { name: 'READY STOCK - LED Light 1M Warm Light Cake Topper (1pc)', quantity: 'x1', price: 1.99 },
            { name: 'READY STOCK - 6pcs 15cm Firework Candle Birthday Candle', quantity: 'x2', price: 1.49 }
          ]
        },
        {
          status: 'In Transit',
          time: '13/10/2024, 12:00 PM',
          items: [
            { name: 'READY STOCK - LED Light 1M Warm Light Cake Topper (1pc)', quantity: 'x1', price: 1.99 }
          ]
        },
        {
          status: 'Delivered',
          time: '14/10/2024, 3:00 PM',
          items: [
            { name: 'READY STOCK - 6pcs 15cm Firework Candle Birthday Candle', quantity: 'x2', price: 1.49 }
          ]
        }
      ],
      scheduledOrders: [
        {
          time: '12/10/2024, 10:00 AM',
          items: [
            { name: 'READY STOCK - LED Light 1M Warm Light Cake Topper (1pc)', quantity: 'x1', price: 1.99 }
          ]
        }
      ],
      inTransitOrders: [
        {
          time: '13/10/2024, 12:00 PM',
          items: [
            { name: 'READY STOCK - 6pcs 15cm Firework Candle Birthday Candle', quantity: 'x2', price: 1.49 }
          ]
        }
      ],
      deliveredOrders: [
        {
          time: '14/10/2024, 3:00 PM',
          items: [
            { name: 'READY STOCK - LED Light 1M Warm Light Cake Topper (1pc)', quantity: 'x1', price: 1.99 }
          ]
        }
      ]
    };
  },
  methods: {
    setActiveTab(tab) {
      this.activeTab = tab; // Update the active tab state
    }
  }
}
</script>

<style scoped>
h3 {
  text-align: center; 
}

.nav-tabs .nav-link {
  color: white !important;
  background-color: #343a40 !important; 
  border: 2px solid #343a40; 
  border-radius: 5px;
  transition: background-color 0.3s ease, border-color 0.3s ease; 
}

.nav-tabs .nav-link.active {
  border-color: #343a40 !important; 
  background-color: white !important; 
  color: black !important; 
  font-weight: bold;
  border: 2px solid #343a40 !important; 
}

.nav-tabs .nav-link:hover {
  background-color: #3a3f44 !important;
  border-color: #3a3f44 !important; 
  color: white !important;
}

.order-box {
  background-color: #ffffff;
  border: #ffffff !important;
}

.order-item {
  border-bottom: 1px solid #ddd;
  padding-bottom: 10px;
  margin-bottom: 10px;
}

.order-item:last-child {
  border-bottom: none;
  margin-bottom: 0;
}
</style>

