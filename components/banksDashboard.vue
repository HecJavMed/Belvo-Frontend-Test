<template>
  <v-container>
    <v-row>
      <v-col>
        <h1>Banks Dashboard</h1>
      </v-col>
      <v-col class="text-right">
        <v-btn color="primary" @click="$emit('logout')">
          Logout
        </v-btn>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-card class="mx-auto">
          <v-list-item-group>
            <v-list-item v-for="(item, index) in banks" :key="index" @click="selectBank(item)">
              <v-list-item-title class="text-center align-self-center">
                {{ item.display_name }} - {{ item.country_code }}
              </v-list-item-title>
            </v-list-item>
          </v-list-item-group>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'BanksDashboard',
  emits: ['select-bank'],
  data: () => ({
    banks: []
  }),
  async created () {
    this.banks = await this.$axios.$get('/api/banks')
  },
  methods: {
    selectBank (bank) {
      this.$emit('select-bank', bank)
    }
  }
}
</script>

<style scoped>
ul {
  list-style-type: none;
  padding: 0;
}

li {
  cursor: pointer;
  padding: 10px;
  border: 1px solid #ccc;
  margin-bottom: 5px;
}

li:hover {
  background-color: #f0f0f0;
}
</style>
