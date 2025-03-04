<template>
  <div class="container mt-5">
    <h2 class="mb-4">Add New Order</h2>
    <form @submit.prevent="add">
      <div class="form-group row">
        <label for="sender" class="col-sm-2 col-form-label">Sender</label>
        <div class="col-sm-10">
          <input type="text" id="sender" v-model="sender" class="form-control" required />
        </div>
      </div>
      <div class="form-group row">
        <label for="destination" class="col-sm-2 col-form-label">Destination</label>
        <div class="col-sm-10">
          <input type="text" id="destination" v-model="destination" class="form-control" required />
        </div>
      </div>
      <div class="form-group row">
        <label for="weight" class="col-sm-2 col-form-label">Weight</label>
        <div class="col-sm-10">
          <div class="input-group">
            <input type="number" id="weight" v-model="weight" class="form-control" required />
            <div class="input-group-append">
              <span class="input-group-text">Kg</span>
            </div>
          </div>
        </div>
      </div>
      <div class="form-group row">
        <label for="status" class="col-sm-2 col-form-label">Status</label>
        <div class="col-sm-10">
          <select id="status" v-model="status" class="form-control" required>
            <option>In delivering</option>
            <option>Confirmed</option>
            <option>Delivered</option>
          </select>
        </div>
      </div>
      <div class="form-group row">
        <div class="col-sm-10 offset-sm-2">
          <button type="submit" class="btn btn-primary">Save</button>
          <router-link to="/" class="btn btn-secondary ml-2">Cancel</router-link>
        </div>
      </div>
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
