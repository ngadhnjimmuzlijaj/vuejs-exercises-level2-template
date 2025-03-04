<template>
  <div class="container mt-5">
    <div class="d-flex justify-content-between align-items-center mb-3">
      <router-link to="add" class="btn btn-primary">Add new order</router-link>
      <p class="mb-0">Message is: {{ message }}</p>
    </div>
    <div class="form-row mb-3">
      <div class="col">
        <input
          v-model="searchQuery"
          class="form-control"
          placeholder="Search by sender or destination"
        />
      </div>
      <div class="col">
        <select v-model="statusFilter" class="form-control">
          <option>All status</option>
          <option>In delivering</option>
          <option>Confirmed</option>
          <option>Delivered</option>
        </select>
      </div>
    </div>
    <table class="table table-striped">
      <thead>
        <tr>
          <th @click="sortBy('date')">Date</th>
          <th @click="sortBy('sender')">Sender</th>
          <th @click="sortBy('destination')">Destination</th>
          <th @click="sortBy('weight')">Weight</th>
          <th @click="sortBy('status')">Status</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="order in filteredOrders" :key="order.id">
          <td>{{ order.date }}</td>
          <td>{{ order.sender }}</td>
          <td>{{ order.destination }}</td>
          <td>{{ order.weight }}</td>
          <td>{{ order.status }}</td>
          <td>
            <button @click="nextStatus(order)" class="btn btn-sm btn-info">Next status</button>
            <button @click="deleteOrder(order.id)" class="btn btn-sm btn-danger">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchQuery: "",
      statusFilter: "All status",
      orders: []
    }
  },
  computed: {
    filteredOrders() {
      return this.orders.filter((order) => {
        const sender = order.sender.toLowerCase()
        const destination = order.destination.toLowerCase()
        const searchQuery = this.searchQuery.toLowerCase()
        const statusFilter = this.statusFilter
        return (
          (sender.includes(searchQuery) || destination.includes(searchQuery)) &&
          (statusFilter === "All status" || order.status === statusFilter)
        )
      })
    }
  },
  mounted() {
    const storedOrders = localStorage.getItem("orders")
    if (storedOrders) {
      this.orders = JSON.parse(storedOrders)
    } else {
      this.orders = [
        {
          id: 1,
          date: "2023/03/05",
          sender: "Sawayn-Streitch",
          destination: "Rowan Group",
          weight: "1 Kg",
          status: "In delivering"
        },
        {
          id: 2,
          date: "2023/03/06",
          sender: "Doe-Johnson",
          destination: "Smith LLC",
          weight: "2 Kg",
          status: "In delivering"
        },
        {
          id: 3,
          date: "2023/03/07",
          sender: "Acme Corp",
          destination: "Globex Inc",
          weight: "3 Kg",
          status: "Confirmed"
        }
      ]
      this.saveOrders()
    }
  },
  methods: {
    saveOrders() {
      localStorage.setItem("orders", JSON.stringify(this.orders))
    },
    sortBy(key) {
      this.orders.sort((a, b) => (a[key] > b[key] ? 1 : -1))
      this.saveOrders()
    },
    nextStatus(order) {
      const status = order.status
      if (status === "In delivering") {
        order.status = "Confirmed"
      } else if (status === "Confirmed") {
        order.status = "Delivered"
      }
      this.saveOrders()
    },
    deleteOrder(id) {
      this.orders = this.orders.filter((order) => order.id !== id)
      this.saveOrders()
    }
  }
}
</script>
