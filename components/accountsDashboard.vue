<template>
  <v-container>
    <v-row>
      <v-col cols="1">
        <v-btn @click="$emit('returnToBanks')">
          Return
        </v-btn>
      </v-col>
      <v-spacer />
      <v-col cols="8">
        <h1> {{ bankName }} - Accounts Dashboard</h1>
      </v-col>
      <v-spacer />
    </v-row>
    <v-row v-if="accounts.length === 0">
      <v-col>
        <h1>
          No accounts available.
        </h1>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-card class="mx-auto">
          <v-list-item-group>
            <v-list-item v-for="(item, index) in accounts" :key="index" @click="selectAccount(item)">
              <v-list-item-title>
                {{ item.name }} - Current: {{ item.balance.current }} - Available: {{ item.balance.available }}
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
  name: 'AccountsDashboard',
  props: {
    bankId: {
      type: Number,
      required: true
    },
    bankName: {
      type: String,
      required: true
    }
  },
  data () {
    return {
      accounts: []
    }
  },
  created () {
    this.fetchAccounts(this.bankId)
  },
  methods: {
    selectAccount (account) {
      this.$emit('selectAccount', account)
    },
    async fetchAccounts (bankId) {
      try {
        this.accounts = await this.$axios.$get('/api/banks/' + bankId + '/accounts')
      } catch (error) {
        alert('Error fetching accounts:', error)
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
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin-bottom: 10px;
}
</style>
