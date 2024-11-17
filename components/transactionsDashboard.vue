<template>
  <v-container>
    <v-row>
      <v-col cols="1">
        <v-btn @click="$emit('returnToAccounts')">
          Return
        </v-btn>
      </v-col>
      <v-spacer />
      <v-col cols="8">
        <h1> {{ accountName }} - Transactions Dashboard</h1>
      </v-col>
      <v-spacer />
    </v-row>
    <v-row v-if="transactions.length === 0">
      <v-col>
        <h1>
          No transactions available.
        </h1>
      </v-col>
    </v-row>
    <v-row v-else>
      <v-spacer />
      <v-col cols="1">
        <h2>KPI</h2>
      </v-col>
      <v-col>
        <p>Inflow: <b>{{ kpi.inflow }}</b></p>
      </v-col>
      <v-col>
        <p>Outflow: <b>{{ kpi.outflow }}</b></p>
      </v-col>
      <v-col>
        <p>Balance: <b>{{ kpi.total }}</b></p>
      </v-col>
      <v-spacer />
    </v-row>
    <v-row>
      <v-col>
        <v-card class="mx-auto">
          <v-list-item-group>
            <v-list-item v-for="(item, index) in transactions" :key="index">
              <v-list-item-title>
                {{ item.value_date }} - ${{ item.amount }} - {{ item.description }}
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
  name: 'TransactionsDashboard',
  props: {
    accountId: {
      type: String,
      required: true
    },
    accountName: {
      type: String,
      required: true
    }
  },
  data () {
    return {
      transactions: [],
      kpi: {
        inflow: 0,
        outflow: 0,
        total: 0
      }
    }
  },
  created () {
    this.fetchTransactions(this.accountId)
  },
  methods: {
    async fetchTransactions (accountId) {
      try {
        this.transactions = await this.$axios.$get('/api/accounts/' + accountId + '/transactions')
        this.kpi = await this.$axios.$get('/api/accounts/' + accountId + '/transactions/kpi')
      } catch (error) {
        alert('Error fetching transactions:', error)
      }
    }
  }
}
</script>

<style scoped>
h1 {
  font-size: 24px;
  margin-bottom: 20px;
}
h2 {
  font-size: 20px;
  margin-bottom: 10px;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  padding: 10px;
  border-bottom: 1px solid #ccc;
}
</style>
