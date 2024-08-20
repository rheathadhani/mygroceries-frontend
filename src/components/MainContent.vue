<template>
  <div class="container-fluid">
    <div class="row">
      <!-- Sidebar -->
      <div class="col-md-5 mt-0 col-lg-2 bg-dark p-4 text-white">
        <h2 class="mb-4 mt-3 ml-0">Manage <br> your account</h2>
        <ul class="list-group">
          <li class="list-group-item list-group-item-action bg-dark text-white border-light"
            @click="currentComponent = 'MyProfile'">
            My Profile
          </li>
          <li class="list-group-item list-group-item-action bg-dark text-white border-light"
            @click="currentComponent = 'MyAddresses'">
            My Addresses
          </li>
          <li class="list-group-item list-group-item-action bg-dark text-white border-light"
            @click="currentComponent = 'MyPayments'">
            My Payments
          </li>
          <li class="list-group-item list-group-item-action bg-dark text-white border-light"
            @click="currentComponent = 'MyOrders'">
            My Orders
          </li>
          <li class="list-group-item list-group-item-action bg-dark text-white border-light"
            @click="currentComponent = 'ChangePassword'">
            Change Password
          </li>
        </ul>
      </div>

      <!-- Content -->
      <div class="col-md-9 col-lg-10 p-4">
        <component :is="currentComponent"></component>
      </div>
    </div>
  </div>
</template>


<script>
import MyProfile from './MyProfile.vue'
import MyPayments from './MyPayments.vue'
import MyAddresses from './MyAddresses.vue'
import ChangePassword from './ChangePassword.vue'
import MyOrders from './MyOrders.vue'

export default {
  name: 'MainContent',
  components: {
    MyProfile,
    MyPayments,
    MyAddresses,
    MyOrders,
    ChangePassword
  },
  data() {
    return {
      currentComponent: 'MyProfile'
    }
  }
}
</script>

<style scoped>
.container {
  display: flex;
  height: 100vh;
}

h2 {
  font-size: 1.5rem;
  color: #fff;
  margin-bottom: 20px;
}

ul {
  padding-left: 0;
}

.list-group-item {
  padding: 15px 20px;
  border-radius: 8px;
  margin-bottom: 10px;
  color: #ffffff;
  border: 1px solid #495057;
  cursor: pointer;
  transition: all 0.3s ease;
}

.list-group-item:hover {
  background-color: #495057;
  /* Slightly darker on hover */
  transform: scale(1.1);
  /* Subtle lift on hover */
}

.content {
  background-color: #f8f9fa;
  color: #000;
  padding: 20px;
}
</style>
