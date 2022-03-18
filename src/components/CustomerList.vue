<script setup lang="ts">
import { ref, computed, watch } from "vue";
import { customers as customersData } from "../Customers";
// import CustomerListItem from "./CustomerListItem.vue";

interface Customer {
  id: number;
  name: string;
  balance: number;
  yearsOwing: number;
  salaryAccountDeactivated: boolean;
}

const customers = ref<Customer[]>(customersData);
const cardToOpen = ref<number | null>(null);

const canNotifyContacts = (
  balance: number,
  yearsOwing: number,
  salaryAccountDeactivated: boolean
) => {
  return balance > 4999 && yearsOwing > 2 && salaryAccountDeactivated;
};

const names = ref([
  {
    name: "mike",
    age: 40,
  },
  {
    name: "John",
    age: 30,
  },
  {
    name: "Jane",
    age: 25,
  },
  {
    name: "anna",
    age: 20,
  },
])

/* const sortedCustomers = computed(() => {
  return customers.value.sort((a, b) => {
    return a.name.localeCompare(b.name);
  })
}*/

const sortedCustomers = computed(() => {
  const items = names.value.slice();
  return items.sort((a, b) => {
    return a.name.localeCompare(b.name);
  });
});

console.log(sortedCustomers.value);
</script>

<template>
  <div>
    <h1>Customers</h1>
    <table class="customers">
      <thead>
        <tr>
          <th>Name</th>
          <th>Balance</th>
          <th>Years Owing</th>
          <th>Salary Account Deactivated</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="customer in customers" :key="customer.id">
          <td>{{ customer.name }}</td>
          <td>{{ customer.balance }}</td>
          <td>{{ customer.yearsOwing }}</td>
          <td>{{ customer.salaryAccountDeactivated ? "Yes" : "No" }}</td>
          <td>
            <div>
              <button class="btn" @click="cardToOpen = customer.id">
                Actions <span class="drop-down-icon">&#x25BC;</span>
              </button>
              <div class="drop-down-list" v-if="cardToOpen === customer.id">
                <div class="drop-down-list-item">View history</div>
                <div class="drop-down-list-item">Nudge customer</div>
                <div
                  class="drop-down-list-item"
                  v-if="
                    canNotifyContacts(
                      customer.balance,
                      customer.yearsOwing,
                      customer.salaryAccountDeactivated
                    )
                  "
                >
                  Notify contacts
                </div>
              </div>
            </div>
          </td>
        </tr>
        <!-- <CustomerListItem
          v-for="customer in customers"
          :key="customer.id"
          :customer="customer"
          :card-to-open="cardToOpen"
          @show-actions="cardToOpen = $event"
        /> -->
      </tbody>
    </table>
  </div>

  {{ sortedCustomers }}
</template>

<style scoped>
.customers {
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

.customers td,
.customers th {
  border: 1px solid #ddd;
  padding: 8px;
}

.customers tr:nth-child(even) {
  background-color: #f2f2f2;
}

.customers tr:hover {
  background-color: #ddd;
}

.customers th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #04aa6d;
  color: white;
}

.btn {
  color: #fff;
  background-color: #04aa6d;
  border: none;
  outline: none;
  padding: 5px 10px;
  border-radius: 3px;
  cursor: pointer;
  font-size: 15px;
}
.drop-down-icon {
  font-size: 10px;
}
.drop-down-list {
  position: absolute;
  background-color: #fff;
  min-width: 160px;
  box-shadow: 0px 8px 16px 0px rgba(0, 0, 0, 0.2);
  z-index: 1;
}

.drop-down-list-item {
  color: #04aa6d;
  padding: 12px 16px;
  text-decoration: none;
  display: block;
  cursor: pointer;
}
.drop-down-list-item:hover {
  background-color: #f1f1f1;
}
</style>
