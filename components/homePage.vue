<template>
  <div>
    <v-container v-if="level == 'banks'">
      <BanksDashboard @select-bank="selectBank" @logout="onLogout" />
    </v-container>
    <v-container v-if="level == 'accounts'">
      <AccountsDashboard :bank-id="bankId" :bank-name="bankName" @selectAccount="selectAccount" @returnToBanks="returnToBanks" />
    </v-container>
    <v-container v-if="level == 'transactions'">
      <TransactionsDashboard :account-id="accountId" :account-name="accountName" @returnToAccounts="returnToAccounts" />
    </v-container>
  </div>
</template>

<script>
// import AuthPage from '../components/AuthPage.vue'

export default {
  name: 'HomePage',
  data () {
    return {
      level: 'banks',
      bankId: null,
      bankName: null,
      accountId: null,
      accountName: null
    }
  },
  methods: {
    selectBank (bank) {
      this.level = 'accounts'
      this.bankId = bank.id
      this.bankName = bank.display_name
    },
    selectAccount (account) {
      this.level = 'transactions'
      this.accountId = account.id
      this.accountName = account.name
    },
    returnToBanks () {
      this.level = 'banks'
    },
    returnToAccounts () {
      this.level = 'accounts'
    },
    onLogout () {
      this.$emit('logout')
    }
  }
}
</script>

<style scoped>
/* Add your styles here */
</style>
