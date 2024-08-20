<template>
  <nav id="nav-bar" class="navbar navbar-expand-lg navbar-dark bg-dark">
    <router-link to="/products" id="products-link" class="navbar-brand">
      <h1>MyGroceries</h1>
    </router-link>

    <div id="search-bar" class="d-flex mx-auto pt-2">
      <input type="text" class="form-control" placeholder="Search for items">
      <button class="btn btn-light search-button">Search</button>
    </div>

    <div id="nav-icons" class="d-flex align-items-center">  
      <router-link to="/settings" id="setting-icon" class="nav-link">
        <img src="../assets/icons/setting.png" alt="setting icon">
      </router-link>

      <router-link to="/profile" id="profile-icon" class="nav-link">
        <img src="../assets/icons/user.png" alt="profile icon">
      </router-link>

      <router-link to="/cart" id="cart-link" class="nav-link">
        <button class="btn btn-light cart-button">My Cart</button>
      </router-link>
    </div>
  </nav>
</template>

<script>
export default {
  name: "NavBar",
}
</script>

<style scoped>
#nav-bar {
  border-bottom: 0.5px solid #ddd;
  height: 75px;
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 20px;
  box-sizing: border-box;

}

#products-link {
  color: white;
  font-size: 22px;
  text-decoration: none;
}

#products-link h1 {
  margin: 0;
  color: white;
}

#search-bar input {
  padding: 8px;  /* Reduced padding */
  border: 0.5px solid #fff;
  border-radius: 5px 0 0 5px;
  outline: none;
  width: 30vw;
}

#search-bar button.search-button {
  padding: 8px 12px;  /* Reduced padding */
  border-radius: 0 5px 5px 0;
  cursor: pointer;
  height: 5.8vh; /* Auto height */
}

#nav-icons img, #nav-icons button {
  margin-left: 20px;
}

img {
  width: 30px;
  height: 30px;
}

#cart-link button.cart-button {
  padding: 8px 12px; /* Reduced padding */
  text-transform: uppercase;
  transition: background-color 0.3s, color 0.3s;
  height: auto; /* Auto height */
}

</style>
