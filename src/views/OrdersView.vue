<template>
  <div class="content">
    <router-link to="add"><button>Add new order</button></router-link>
    <p>Message is: {{ message }}</p>
    <input v-model="searchQuery" placeholder="Search by sender" />
    <select v-model="statusFilter">
      <option>All status</option>
      <option>In delivering</option>
      <option>Confirmed</option>
    </select>
    <table>
      <thead>
        <tr>
          <th @click="sortBy('date')">Date</th>
          <th @click="sortBy('sender')">Sender</th>
          <th @click="sortBy('destination')">Destination</th>
          <th @click="sortBy('weight')">Weight</th>
          <th @click="sortBy('status')">Status</th>
          <th>Actions</th> <!-- Nouvelle colonne ajoutée -->
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
            <button @click="nextStatus(order)">Next status</button>
            <button @click="deleteOrder(order.id)">Delete</button>
          </td> <!-- Nouvelle cellule ajoutée -->
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
export default {
  data() {
    return {
      searchQuery: '',
      statusFilter: 'All status',
      orders: [
        { id: 1, date: '2023/03/05', sender: 'Sawayn-Streitch', destination: 'Rowan Group', weight: '1 Kg', status: 'In delivering' },
        { id: 2, date: '2023/03/06', sender: 'Doe-Johnson', destination: 'Smith LLC', weight: '2 Kg', status: 'In delivering' },
        { id: 3, date: '2023/03/07', sender: 'Acme Corp', destination: 'Globex Inc', weight: '3 Kg', status: 'confirmed' }
      ]
    }
  },
  computed: {
    filteredOrders() {
      return this.orders.filter(order => {
        const filtreSender = order.sender.toLowerCase().includes(this.searchQuery.toLowerCase());
        const filtreStatus = this.statusFilter === 'All status' || order.status === this.statusFilter;
        return filtreSender && filtreStatus;
      });
    }
  },
  methods: {
    sortBy(attribute) {
      this.orders.sort((a, b) => (a[attribute] > b[attribute]) ? 1 : -1);
    },
    nextStatus(order) {
      if (order.status === 'In delivering') {
        order.status = 'Delivered';
      } else if (order.status === 'Confirmed') {
        order.status = 'In delivering';
      }
    },
    deleteOrder(orderId) {
      this.orders = this.orders.filter(order => order.id !== orderId);
    }
  }
}
</script>
