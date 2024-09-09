<template>
  <nav id="nav-bar" class="navbar navbar-expand-lg navbar-dark bg-dark">
    <!-- Brand -->
    <router-link to="/products" class="navbar-brand">
      <h1 class="d-none d-lg-block">MyGroceries</h1>
      <h4 class="d-lg-none mt-2">MyGroceries</h4>
    </router-link>

    <!-- Toggle button for mobile view -->
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarContent"
      aria-controls="navbarContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>

    <!-- Collapsible content -->
    <div class="collapse navbar-collapse" id="navbarContent">
      <!-- Search bar and button (centered in the navbar) -->
      <div class="search-container d-flex justify-content-center mx-auto my-lg-0">
        <input type="text" class="form-control me-2" placeholder="Search for items" style="width: 60%;" />
        <button class="btn btn-light search-button border-dark">Search</button>
      </div>

      <!-- Navbar links (aligned to the right in the navbar) -->
      <ul class="navbar-nav ms-auto">
        <li class="nav-item">
          <router-link to="/settings" class="nav-link">
            Settings
          </router-link>
        </li>
        <li class="nav-item">
          <router-link to="/profile" class="nav-link">
            Profile
          </router-link>
        </li>
        <li class="nav-item">
          <router-link to="/cart" class="nav-link">
            My Cart
          </router-link>
        </li>
      </ul>
    </div>
  </nav>
</template>

<script>
export default {
  name: "NavBar",
};
</script>

<style scoped>
#nav-bar {
  width: 100%;
  padding: 0 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-bottom: 0.5px solid #ddd;
  box-sizing: border-box;
}

.navbar-toggler {
  padding: 0.25rem 0.5rem;
  font-size: 1.25rem;
  line-height: 1;
}

.search-container {
  width: 100%;
  max-width: 700px;
  /* Adjust max-width for larger screens */
}

.navbar-nav {
  margin-left: auto;
}

@media (max-width: 992px) {
  .search-container {
    max-width: 60%;
    /* Adjust width for medium screens */
  }

  .navbar-brand h1 {
    display: none;
  }

  .navbar-brand h4 {
    display: block;
  }
}

@media (max-width: 576px) {
  .search-container {
    max-width: 120%;
    /* Adjust width for small screens */
  }
}

.search-button {
  width: auto;
  height: auto;
  padding: 5px 10px;
}

@media (max-width: 992px) {
  .search-button {
    padding: 4px 8px;
  }
}
</style>
