<template>
  <h3> Edit Profile </h3>
  <div class="container mt-4">
    <div class="profile-container bg-white p-4 shadow-sm">
      <form class="profile-form" method="post">

        <div class="form-group row mb-3">
          <label for="username" class="col-sm-3 col-form-label">Username</label>
          <div class="col-sm-9">
            <input type="text" class="form-control" id="username" v-model="user.username">
          </div>
        </div>

        <div class="form-group row mb-3">
          <label for="name" class="col-sm-3 col-form-label">Name</label>
          <div class="col-sm-9">
            <input type="text" class="form-control" id="name" v-model="user.name">
          </div>
        </div>

        <div class="form-group row mb-3">
          <label for="email" class="col-sm-3 col-form-label">Email</label>
          <div class="col-sm-9">
            <input type="email" class="form-control" id="email" v-model="user.email">
          </div>
        </div>

        <div class="form-group row mb-3">
          <label for="phone" class="col-sm-3 col-form-label">Phone Number</label>
          <div class="col-sm-9">
            <input type="tel" class="form-control" id="phone" v-model="user.phone">
          </div>
        </div>

        <div class="form-group row mb-3">
          <label class="col-sm-3 col-form-label">Gender</label>
          <div class="col-sm-9 d-flex align-items-center">
            <div class="form-check form-check-inline">
              <input type="radio" id="male" value="Male" v-model="user.gender" class="form-check-input">
              <label for="male" class="form-check-label">Male</label>
            </div>
            <div class="form-check form-check-inline">
              <input type="radio" id="female" value="Female" v-model="user.gender" class="form-check-input">
              <label for="female" class="form-check-label">Female</label>
            </div>
            <div class="form-check form-check-inline">
              <input type="radio" id="other" value="Other" v-model="user.gender" class="form-check-input">
              <label for="other" class="form-check-label">Other</label>
            </div>
          </div>
        </div>

        <div class="form-group row mb-4">
          <label for="dob" class="col-sm-3 col-form-label">Date of birth</label>
          <div class="col-sm-9">
            <input type="date" class="form-control" id="dob" v-model="user.dob">
          </div>
        </div>

        <div class="form-group">
          <button type="submit" class="btn btn-dark w-100">Save</button>
        </div>

      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: "MyProfile",
  data() {
    return {
      user: {
        username: '',
        name: 'Rhea',
        email: '',
        phone: '',
        gender: '',
        dob: ''
      }
    }
  }
}
</script>

<style scoped>
.profile-container {
  width: 100%; /* Make the container take full width of the parent */
  max-width: 70vw; /* Limit the maximum width to 80% of the viewport */
  height: 70vh; /* Reduced height for the container */
  margin: 20px auto; /* Center the container horizontally */
  padding: 20px;
  background-color: #fff;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  overflow-x: hidden; /* Prevent horizontal overflow */
}

h3 {
  font-size: 28px;
  font-weight: bold;
  text-align: center; /* Center-align the "Edit Profile" heading */
}

button {
  transition: background-color 0.3s, color 0.3s;
  cursor: pointer;
  border-radius: 4px;
}

button:hover {
  background-color: #fff;
  color: #000;
}
</style>


