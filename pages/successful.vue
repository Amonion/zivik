<template>
  <div class="dashboard-wrapper withdraw">
    <dashboard-navigation />
    <div class="dashboard-content">
      <dashboard-header />
      <div class="content-body">
        <div class="dashboard-card-wrap">
          <div class="card-types">
            <div class="card-types-wrapper success-wrap">
              <div class="card-title success">
                Your transaction was successful and will be approved after
                confirmation
              </div>
              <div class="type-card select card">
                <div class="card-type-flex">
                  <h4 class="type-card-title spand">Transaction Type</h4>
                  <div>{{ transaction.transactionType }}</div>
                </div>
                <div class="card-type-flex">
                  <h4 class="type-card-title spand">Amount</h4>
                  <div>${{ transaction.amount }}</div>
                </div>
                <div class="card-type-flex">
                  <h4 class="type-card-title spand">Username</h4>
                  <div>J{{ transaction.username }}</div>
                </div>
                <div class="card-type-flex">
                  <h4 class="type-card-title spand">Receiver Name</h4>
                  <div>{{ transaction.receiverAccountName }}</div>
                </div>
                <div class="card-type-flex">
                  <h4 class="type-card-title spand">Receiver Account Number</h4>
                  <div>{{ transaction.receiverAccountNumber }}</div>
                </div>
                <div class="card-type-flex">
                  <h4 class="type-card-title spand">Receiver Bank</h4>
                  <div>{{ transaction.receiverBank }}</div>
                </div>
                <div class="card-type-flex">
                  <h4 class="type-card-title spand">Transaction Time</h4>
                  <div>
                    <span>{{ formatNumberToDate(transaction.time) }}</span>
                    <br />
                    <span>{{ formatDateToTime(transaction.time) }}</span>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="personal-form payment-input w-form">
            <div class="form-flex">
              <div class="button-holder cent">
                <NuxtLink to="/dashboard" class="btn-custom w-button"
                  >Cancel</NuxtLink
                >
              </div>
            </div>
          </div>
        </div>

        <dashboard-transactions />
        <dashboard-activities />
      </div>
      <dashboard-footer />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      company: "",
      amount: 0,
      account: "",
      accounts: [],

      pin: "",
      newPin: "",
      confirmPin: "",
      routingNumber: "",
      setPin: false,

      confirmWithdrawal: false,

      msg: "",
      colour: false,
      showMsg: false,
    };
  },
  methods: {
    showMessage(msg) {
      this.msg = msg;
      this.showMsg = true;
      setTimeout(() => {
        this.msg = "";
        this.showMsg = false;
      }, 6000);
    },

    formatMoney(amount) {
      if (amount == "" || amount == null || amount == undefined) {
        return "0.00";
      } else {
        return amount.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
      }
    },

    formatNumberToDate(inputNumber) {
      if (typeof inputNumber !== "number") {
        return "Not Available";
      }

      const date = new Date(inputNumber);

      const day = ("0" + date.getDate()).slice(-2);
      const month = ("0" + (date.getMonth() + 1)).slice(-2);
      const year = date.getFullYear();

      const formattedDate = `${day}/${month}/${year}`;

      return formattedDate;
    },

    formatDateToTime(date) {
      const options = {
        hour: "numeric",
        minute: "2-digit",
        hour12: true,
      };

      return new Intl.DateTimeFormat("en-US", options).format(date);
    },
  },
  computed: {
    user() {
      return this.$store.state.auth.user;
    },

    transaction() {
      return this.$store.state.transaction;
    },
  },
  mounted() {
    if (!this.transaction) {
      this.$router.push("/dashboard");
    }
  },
  head() {
    return {
      link: [
        { rel: "stylesheet", type: "text/css", href: "/css/dashboard.css" },
      ],
    };
  },
};
</script>

<style>
.msg {
  width: 100%;
  text-align: left;
}

.type-card.other.select {
  cursor: default;
}

.each-account {
  cursor: pointer;
}

.each-account.selected {
  border: 1px solid #e524c5;
  background: #fff6f5;
}
</style>
