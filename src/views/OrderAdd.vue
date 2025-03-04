<template>
<div class="order-add">
  <h2>Add new order</h2>
  <form @submit.prevent="add">
    <label for="sender">Sender</label>
    <input type="text" id="sender" v-model="sender" required />
    <label for="destination">Destination</label>
    <input type="text" id="destination" v-model="destination" required />
    <label for="weight">Weight</label>
    <input type="number" id="weight" v-model="weight" required />
    <label for="status">Status</label>
    <select id="status" v-model="status" required>
      <option>In delivering</option>
      <option>Confirmed</option>
      <option>Delivered</option>
    </select>

  <button @click="add">Save</button>
  <router-link to="/"><button>Cancel</button></router-link>
    </form>
  </div>
</template>

<script>
import { nanoid } from "nanoid"

export default {
  data() {
    return {
      sender: "",
      destination: "",
      weight: "",
      status: "In delivering"
    }
  },
  computed: {},
  methods: {
    add() {
      if (!this.sender || !this.destination || !this.weight) {
        alert("Please fill all fields")
        return
      }
      if (this.weight <= 0) {
        alert("Weight must be greater than 0")
        return
      }
      const newOrder = {
        id: this.newId(),
        date: new Date().toLocaleDateString(),
        sender: this.sender,
        destination: this.destination,
        weight: this.weight + "Kg",
        status: this.status
      }
      const orders = JSON.parse(localStorage.getItem("orders")) || []
      orders.push(newOrder)
      localStorage.setItem("orders", JSON.stringify(orders))
      this.$router.push("/");

    },
    newId() {
      return nanoid()
    }
  }
}
</script>
