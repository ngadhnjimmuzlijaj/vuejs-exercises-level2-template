<template>
  <div class="container mt-5">
    <h2>Add new order</h2>
    <form @submit.prevent="add">
      <div class="form-group">
        <label for="sender">Sender</label>
        <input type="text" id="sender" v-model="sender" class="form-control" required />
      </div>
      <div class="form-group">
        <label for="destination">Destination</label>
        <input type="text" id="destination" v-model="destination" class="form-control" required />
      </div>
      <div class="form-group">
        <label for="weight">Weight</label>
        <div class="input-group">
          <input type="number" id="weight" v-model="weight" class="form-control" required />
          <div class="input-group-append">
            <span class="input-group-text">Kg</span>
          </div>
        </div>
      </div>
      <div class="form-group">
        <label for="status">Status</label>
        <select id="status" v-model="status" class="form-control" required>
          <option>In delivering</option>
          <option>Confirmed</option>
          <option>Delivered</option>
        </select>
      </div>
      <button type="submit" class="btn btn-primary">Save</button>
      <router-link to="/" class="btn btn-secondary ml-2">Cancel</router-link>
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
  methods: {
    add() {
      if (!this.sender || !this.destination || !this.weight) {
        alert("Please fill all fields");
        return;
      }
      if (this.weight <= 0) {
        alert("Weight must be greater than 0");
        return;
      }
      const newOrder = {
        id: this.newId(),
        date: new Date().toLocaleDateString(),
        sender: this.sender,
        destination: this.destination,
        weight: this.weight,
        status: this.status
      };
      const orders = JSON.parse(localStorage.getItem("orders")) || [];
      orders.push(newOrder);
      localStorage.setItem("orders", JSON.stringify(orders));
      this.$router.push("/");
    },
    newId() {
      return nanoid();
    }
  }
}
</script>
