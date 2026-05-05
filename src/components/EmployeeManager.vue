<template>
  <div class="container mt-5">
    <h2 class="text-center mb-4">Employee Management System</h2>

    <!-- FORM -->
    <div class="card p-3 mb-4">
      <input v-model="form.name" class="form-control mb-2" placeholder="Name" />
      <input v-model="form.designation" class="form-control mb-2" placeholder="Designation" />
      <input v-model="form.department" class="form-control mb-2" placeholder="Department" />
      <input v-model="form.salary" class="form-control mb-2" placeholder="Salary" />

      <button
        @click="editId ? updateEmployee() : addEmployee()"
        class="btn btn-primary"
      >
        {{ editId ? 'Update Employee' : 'Add Employee' }}
      </button>
    </div>

    <!-- TABLE -->
    <table class="table table-bordered text-center">
      <thead class="table-dark">
        <tr>
          <th>Name</th>
          <th>Designation</th>
          <th>Department</th>
          <th>Salary</th>
          <th>Actions</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="emp in employees" :key="emp.id">
          <td>{{ emp.name }}</td>
          <td>{{ emp.designation }}</td>
          <td>{{ emp.department }}</td>
          <td>{{ emp.salary }}</td>
          <td>
            <button
              @click="editEmployee(emp)"
              class="btn btn-warning btn-sm me-2"
            >
              Edit
            </button>
            <button
              @click="deleteEmployee(emp.id)"
              class="btn btn-danger btn-sm"
            >
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import api from '../services/api.js'

export default {
  data() {
    return {
      employees: [],
      form: {
        name: '',
        designation: '',
        department: '',
        salary: ''
      },
      editId: null
    }
  },

  mounted() {
    this.getEmployees()
  },

  methods: {
    async getEmployees() {
      const res = await api.get('/employees')
      this.employees = res.data
    },

    async addEmployee() {
      await api.post('/employees', this.form)
      this.resetForm()
      this.getEmployees()
    },

    editEmployee(emp) {
      this.form = { ...emp }
      this.editId = emp.id
    },

    async updateEmployee() {
      await api.put(`/employees/${this.editId}`, this.form)
      this.resetForm()
      this.getEmployees()
    },

    async deleteEmployee(id) {
      await api.delete(`/employees/${id}`)
      this.getEmployees()
    },

    resetForm() {
      this.form = {
        name: '',
        designation: '',
        department: '',
        salary: ''
      }
      this.editId = null
    }
  }
}
</script>