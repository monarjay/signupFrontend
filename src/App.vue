<template>
  <div class="app-background min-vh-100 d-flex align-items-center justify-content-center">
    <div class="container py-5">
      <div class="row justify-content-center">
        <div class="col-md-6">
          <div class="card p-4 shadow">
            <h2 class="mb-4 text-center text-primary">Signup Form</h2>
            <form @submit.prevent="submitForm">
              <div class="mb-3">
                <label class="form-label">Name</label>
                <input v-model="name" class="form-control" required />
              </div>
              <div class="mb-3">
                <label class="form-label">Email</label>
                <input v-model="email" type="email" class="form-control" required />
              </div>
              <button class="btn btn-primary w-100">Submit</button>
            </form>
          </div>
        </div>
      </div>

      <div class="row justify-content-center mt-5">
        <div class="col-md-10">
          <div class="card p-4 shadow">
            <h3 class="mb-3 text-center text-success">All Signups</h3>
            <table class="table table-striped table-bordered">
              <thead class="table-light">
                <tr>
                  <th>Name</th>
                  <th>Email</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="signup in signups" :key="signup.id">
                  <td>{{ signup.name }}</td>
                  <td>{{ signup.email }}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: '',
      email: '',
      signups: [],
      API_BASE: 'http://localhost:3000' // Change to Render URL in production
    }
  },
  methods: {
    async submitForm() {
      await fetch(`${this.API_BASE}/signup`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ name: this.name, email: this.email }),
      });
      this.name = '';
      this.email = '';
      this.getSignups();
    },
    async getSignups() {
      const res = await fetch(`${this.API_BASE}/signups`);
      this.signups = await res.json();
    }
  },
  mounted() {
    this.getSignups();
  }
}
</script>

<style>
.app-background {
  background-image: url('https://images.unsplash.com/photo-1509228627152-72ae9ae6848c?auto=format&fit=crop&w=1600&q=80');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  min-height: 100vh;
  padding-top: 40px;
  padding-bottom: 40px;
}
</style>
