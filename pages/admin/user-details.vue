<template>
  <div class="main-wrapper user-details">
    <AdminNavigation />
    <div class="body-wrapper">
      <AdminTopHeader />

      <div class="content-wrapper">
        <div user class="custom-card plan">
          <h4 class="page-title">Users Profile</h4>
          <div class="div-block pix">
            <img
              v-if="user.profilePicture"
              :src="`${FILE_URL}/${user.profilePicture}`"
              alt=""
              class="user-account"
            />
            <img v-else src="/images/user.svg" alt="" class="user-account" />
          </div>
          <div class="identity">
            <img v-if="idPicture" :src="`${FILE_URL}/${idPicture}`" alt="" />
            <img v-else src="/images/image-id.svg" alt="" />
          </div>

          <div class="input-wrapper w-form">
            <h3 class="user-info-title">Personal Info</h3>
            <form action="#" class="input-form">
              <div class="each-input">
                <label for="name-4" class="input-label"
                  >First Name<span class="important">*</span></label
                ><input
                  type="text"
                  class="plan-input w-input"
                  v-model="user.firstName"
                  name="Name"
                />
              </div>
              <div class="each-input">
                <label for="name-4" class="input-label"
                  >Middle Name<span class="important">*</span></label
                ><input
                  type="text"
                  class="plan-input w-input"
                  name="Name"
                  v-model="user.middleName"
                />
              </div>
              <div class="each-input">
                <label for="name-4" class="input-label"
                  >Last Name<span class="important">*</span></label
                ><input
                  type="text"
                  class="plan-input w-input"
                  name="Name"
                  v-model="user.lastName"
                />
              </div>
              <div class="each-input">
                <label for="email-4" class="input-label"
                  >Username <span class="important">*</span></label
                ><input
                  type="text"
                  class="plan-input w-input"
                  name="username"
                  v-model="username"
                />
              </div>
              <div class="each-input">
                <label for="email-4" class="input-label"
                  >Status
                  <span class="important">{{ user.status }}</span></label
                ><input
                  type="text"
                  class="plan-input w-input"
                  name="username"
                  placeholder="Admin/Staff"
                  v-model="status"
                />
              </div>
              <div class="each-input">
                <label for="email-4" class="input-label">Pin </label
                ><input
                  type="text"
                  class="plan-input w-input"
                  name="username"
                  placeholder="Pin"
                  v-model="user.pin"
                />
              </div>
              <div class="each-input">
                <label for="email-4" class="input-label"
                  >Status
                  <span class="important">{{ user.staffType }}</span></label
                ><input
                  type="text"
                  class="plan-input w-input"
                  name="username"
                  placeholder="Admin/Staff"
                  v-model="staffType"
                />
              </div>
              <div class="each-input">
                <label for="email-4" class="input-label">Code </label
                ><input
                  type="number"
                  class="plan-input w-input"
                  name="username"
                  placeholder="Set Code"
                  v-model="code"
                />
              </div>
              <div class="each-input">
                <label for="email-4" class="input-label"
                  >Date of Birth<span class="important">*</span>
                  <span v-if="checkId"
                    >Age {{ getAge(Number(user.dob)) }}</span
                  > </label
                ><input type="date" class="plan-input w-input" v-model="dob" />
              </div>
              <div class="each-input">
                <label for="name-4" class="input-label">Source of Income</label>
                <h4
                  class="select-display"
                  @click="showIncomeList = !showIncomeList"
                >
                  {{ income }}
                </h4>
                <ul
                  role="list"
                  class="filter-list down"
                  :class="{ hide: !showIncomeList }"
                >
                  <li
                    class="filter-items"
                    v-for="(item, i) in incomeArray"
                    :key="i"
                  >
                    <div @click="selectIcome(item)">{{ item }}</div>
                  </li>
                </ul>
                <img
                  @click="showIncomeList = !showIncomeList"
                  src="/images/caret-down-gray.svg"
                  loading="lazy"
                  alt=""
                  class="filter-icon middle"
                />
              </div>

              <div class="each-input sm">
                <label for="name-4" class="input-label">Single</label>
                <div
                  class="check-wrapper marital"
                  @click="selectMaritalStatus($event, 'Single')"
                >
                  <div
                    :class="{ active: maritalStatus == 'Single' }"
                    class="check-indicator"
                  ></div>
                </div>
              </div>
              <div class="each-input sm">
                <label for="name-4" class="input-label">Married</label>
                <div
                  class="check-wrapper marital"
                  @click="selectMaritalStatus($event, 'Married')"
                >
                  <div
                    :class="{ active: maritalStatus == 'Married' }"
                    class="check-indicator"
                  ></div>
                </div>
              </div>
              <div class="each-input sm">
                <label for="name-4" class="input-label">Male</label>
                <div
                  class="check-wrapper gender"
                  @click="selectGender($event, 'Male')"
                >
                  <div
                    :class="{ active: gender == 'Male' }"
                    class="check-indicator"
                  ></div>
                </div>
              </div>
              <div class="each-input sm">
                <label for="name-4" class="input-label">Female</label>
                <div
                  class="check-wrapper gender"
                  @click="selectGender($event, 'Female')"
                >
                  <div
                    :class="{ active: gender == 'Female' }"
                    class="check-indicator"
                  ></div>
                </div>
              </div>
              <div class="each-input sm">
                <label for="name-4" class="input-label">Others</label>
                <div
                  class="check-wrapper gender"
                  @click="selectGender($event, 'Others')"
                >
                  <div
                    :class="{ active: gender == 'Others' }"
                    class="check-indicator"
                  ></div>
                </div>
              </div>
              <div v-if="status == 'Staff'" class="each-input">
                <label for="email-4" class="input-label"
                  >ID Type <span class="important">*</span></label
                ><input
                  type="text"
                  class="plan-input w-input"
                  name="username"
                  v-model="identity"
                />
              </div>
            </form>
          </div>

          <!--================================RESIDENTIAL===================================-->
          <div class="input-wrapper w-form">
            <h3 class="user-info-title">Residential Address</h3>
            <form action="#" class="input-form">
              <div class="each-input">
                <label for="name-4" class="input-label"
                  >Address 1<span class="important">*</span></label
                ><input
                  type="text"
                  class="plan-input w-input"
                  name="Address"
                  v-model="residentAddress1"
                />
              </div>
              <div class="each-input">
                <label for="name-4" class="input-label"
                  >Address 2<span class="important">*</span></label
                ><input
                  type="text"
                  class="plan-input w-input"
                  maxlength="256"
                  name="name-3"
                  v-model="residentAddress2"
                />
              </div>
              <div class="each-input">
                <label for="name-4" class="input-label"
                  >Land Mark/ District<span class="important">*</span></label
                ><input
                  type="text"
                  class="plan-input w-input"
                  maxlength="256"
                  v-model="residentDistrict"
                />
              </div>
              <div class="each-input">
                <label for="name-4" class="input-label"
                  >Zip Code<span class="important">*</span></label
                ><input
                  type="text"
                  class="plan-input w-input"
                  maxlength="256"
                  v-model="residentZipCode"
                />
              </div>
              <div class="each-input">
                <label for="name-4" class="input-label"
                  >State<span class="important">*</span></label
                ><input
                  type="text"
                  class="plan-input w-input"
                  maxlength="256"
                  v-model="residentState"
                />
              </div>

              <div class="each-input">
                <label for="name-4" class="input-label">Country</label>
                <h4
                  class="select-display"
                  @click="showCountryList = !showCountryList"
                >
                  {{ residentCountry }}
                </h4>
                <ul
                  role="list"
                  class="filter-list down"
                  :class="{ hide: !showCountryList }"
                >
                  <li
                    class="filter-items"
                    v-for="(country, i) in countryArray"
                    :key="i"
                  >
                    <div @click="selectResidentCountry(country)">
                      {{ country }}
                    </div>
                  </li>
                </ul>
                <img
                  @click="showCountryList = !showCountryList"
                  src="/images/caret-down-gray.svg"
                  loading="lazy"
                  alt=""
                  class="filter-icon middle"
                />
              </div>
            </form>
          </div>
          <!--===============================X=RESIDENTIAL=X==================================-->

          <!--================================CONTACT INFO===================================-->
          <div class="input-wrapper w-form">
            <h3 class="user-info-title">Contact Info</h3>
            <form action="#" class="input-form">
              <div class="each-input">
                <label for="password-3" class="input-label"
                  >Phone Number 1<span class="important">*</span></label
                ><input
                  type="tel"
                  class="plan-input w-input"
                  maxlength="256"
                  v-model="phoneNumber1"
                />
              </div>
              <div class="each-input">
                <label for="password-3" class="input-label">Phone Number</label
                ><input
                  type="tel"
                  class="plan-input w-input"
                  maxlength="256"
                  v-model="phoneNumber2"
                />
              </div>
              <div class="each-input">
                <label for="duration-3" class="input-label"
                  >Email <span class="important">*</span></label
                ><input
                  type="email"
                  class="plan-input w-input"
                  maxlength="256"
                  v-model="email"
                />
              </div>
            </form>
          </div>
          <!--==============================X==CONTACT INFO==X=================================-->

          <!--================================ACCOUNT INFO===================================-->
          <div class="input-wrapper w-form">
            <h3 class="user-info-title">Account Info</h3>
            <div class="input-form" @submit.prevent="">
              <div
                v-for="account in accounts"
                :key="account.id"
                class="each-input"
              >
                <label for="name-4" class="input-label"
                  >Balance (${{ account.balance }})</label
                ><input
                  type="number"
                  class="plan-input w-input"
                  maxlength="256"
                  name="Balance"
                  placeholder="Top Balance"
                  @keypress.enter="enterAccount"
                  v-model="balance"
                />
              </div>
            </div>
          </div>
          <!--==============================X==ACCOUNT INFO==X=================================-->

          <!--=================================KIN INFO====================================-->
          <div class="input-wrapper w-form">
            <h3 class="user-info-title">Login Info</h3>
            <div class="input-form">
              <div class="each-input">
                <label for="password-3" class="input-label"
                  >Old Password<span class="important">{{
                    user.pass
                  }}</span></label
                ><input
                  type="tel"
                  class="plan-input w-input"
                  maxlength="256"
                  v-model="oldPassword"
                />
              </div>
              <div class="each-input">
                <label for="password-3" class="input-label"
                  >Password <span class="important">*</span></label
                ><input
                  type="password"
                  class="plan-input w-input"
                  maxlength="256"
                  v-model="password"
                />
              </div>
              <div class="each-input">
                <label for="password-3" class="input-label"
                  >Confirm Password<span class="important">*</span></label
                ><input
                  type="password"
                  class="plan-input w-input"
                  maxlength="256"
                  v-model="cPassword"
                />
              </div>
              <div
                class="response"
                v-if="showResponse"
                :class="{ success: responseState }"
              >
                {{ response }}
              </div>
              <div class="button-holder">
                <input
                  v-if="showEditButton"
                  type="button"
                  value="Edit User"
                  class="button w-button"
                  @click="processUpdateUser"
                />

                <input
                  type="button"
                  v-if="!showEditButton"
                  value="Update User"
                  class="button w-button"
                  @click="processCreateUser"
                />

                <input
                  v-if="showEditButton"
                  type="button"
                  value="Create Transaction"
                  class="button w-button"
                  @click="createTransaction"
                />

                <input
                  v-if="showEditButton"
                  type="submit"
                  value="Reset Users"
                  @click="resetUser"
                  data-wait="Please wait..."
                  class="button reset w-button"
                />
              </div>
            </div>
          </div>
          <!--==============================X==KIN INFO==X=================================-->
        </div>
        <Instruction />
      </div>
      <AdminFooter />
    </div>
  </div>
</template>

<script>
import AdminFooter from "../../components/admin/AdminFooter.vue";
import AdminNavigation from "../../components/admin/AdminNavigation.vue";
import AdminTopHeader from "../../components/admin/AdminTopHeader.vue";
import Instruction from "../../components/admin/Instruction.vue";

export default {
  data() {
    return {
      firstName: "",
      middleName: "",
      lastName: "",
      username: "",
      dob: "",
      income: "Select Income",
      gender: "",
      maritalStatus: "",
      profilePicture: "",
      idPicture: "",
      identity: "",
      position: "",
      showIncomeList: false,
      showCountryList: false,
      showOriginCountryList: false,
      status: "",
      staffType: "",
      code: "",

      residentAddress1: "",
      residentAddress2: "",
      residentDistrict: "",
      residentState: "",
      residentZipCode: "",
      residentCountry: "Select Country",

      originAddress1: "",
      originAddress2: "",
      originDistrict: "",
      originState: "",
      originZipCode: "",
      originCountry: "Select Country",

      phoneNumber1: "",
      phoneNumber2: "",
      email: "",

      kinFullName: "",
      kinRelationship: "",
      kinAddress: "",
      kinEmail: "",
      kinPhoneNumber: "",

      password: "",
      cPassword: "",
      oldPassword: "",

      incomeArray: ["Salary", "Trading", "Freelancing", "Technician"],

      response: "",
      responseState: false,
      showResponse: false,

      editState: false,
      editId: "",
      user: "",
      transactionTypes: ["Deposit", "Transfer", "Withdrawal"],
      transction: "Select Transaction",
      balance: "",
      showCurrencyList: false,

      currencies: [],
      currency: "Choose Currency",
      showCurrencyList: false,
      accounts: [],

      showEditButton: false,
      editAccountState: false,
      editIndex: "",
    };
  },
  methods: {
    showResponseMsg(msg, status) {
      this.response = msg;
      this.responseState = status;
      this.showResponse = true;
      setTimeout(() => {
        this.response = "";
        this.showResponse = false;
      }, 6000);
    },

    createTransaction() {
      this.$router.push(`/admin/transactions/${this.$route.params.id}`);
    },

    checkId() {
      if (this.$route.params.id.length < 20) {
        this.showEditButton = false;
        return false;
      } else {
        this.editId = this.$route.params.id;
        this.showEditButton = true;
        return true;
      }
    },

    getAge(date) {
      const differenceMs = new Date().getTime() - new Date(date).getTime();
      return Math.floor(differenceMs / (1000 * 60 * 60 * 24 * 365));
    },

    formatDate(data) {
      console.log(data);
      const date = new Date(data);
      const options = {
        weekday: "short",
        day: "numeric",
        month: "long",
      };

      const formatter = new Intl.DateTimeFormat("en-US", options);
      return formatter.format(date);
    },

    removeClass(elements) {
      for (var i = 0; i < elements.length; i++) {
        var element = elements[i];

        if (element.classList.contains("active")) {
          // Remove the "active" class
          element.classList.remove("active");
        }
      }
    },

    removeAccount(int) {
      this.userAccounts.splice(int, 1);
    },

    editAccount(int, account) {
      this.editAccountState = true;
      this.editIndex = int;
      this.plan = account.name;
      this.currency = account.currency;
      this.balance = account.balance;
    },

    selectTransaction(item) {
      this.plan = item;
      this.showCurrencyList = false;
    },

    selectCurrency(item) {
      this.currency = item;
      this.showCurrencyList = false;
    },

    selectIcome(data) {
      this.income = data;
      this.showIncomeList = false;
    },

    selectMaritalStatus(e, maritalStatus) {
      var elements = document.querySelectorAll(".marital");

      this.removeClass(elements);

      e.srcElement.classList.add("active");
      this.maritalStatus = maritalStatus;
    },

    selectGender(e, gender) {
      var elements = document.querySelectorAll(".gender");

      this.removeClass(elements);

      e.srcElement.classList.add("active");
      this.gender = gender;
    },

    selectResidentCountry(data) {
      this.residentCountry = data;
      this.showCountryList = false;
    },

    selectOriginCountry(data) {
      this.originCountry = data;
      this.showOriginCountryList = false;
    },

    enterAccount() {
      const form = {
        balance: this.balance * 1 + this.account.balance * 1,
        accountId: this.account._id,
      };
      this.updateUser(form);
    },

    clearInputs() {
      this.editAccountState = false;
      this.editIndex = "";
    },

    populateUserData(user) {
      this.firstName = user.firstName;
      this.middleName = user.middleName;
      this.lastName = user.lastName;
      this.username = user.username;
      this.dob = user.dob;
      this.profilePicture = user.profilePicture;
      this.idPicture = user.idPicture;
      this.identity = user.identity;
      this.position = user.position;
      this.status = user.status;
      this.staffType = user.staffType;
      this.userAccounts = user.accounts;
      this.phoneNumber1 = user.phoneNumber1;
      this.email = user.email;
      this.code = user.code;
    },

    processUserData() {
      const message = "success";
      if (!message == "success") {
      } else {
        const form = new FormData();

        form.append("firstName", this.firstName);
        form.append("middleName", this.middleName);
        form.append("lastName", this.lastName);
        form.append("dob", new Date(this.dob).getTime());
        form.append("username", this.username);
        form.append("income", this.income);
        form.append("gender", this.gender);
        form.append("code", this.code);
        form.append("maritalStatus", this.maritalStatus);
        form.append("position", this.position);
        form.append("residentAddress1", this.residentAddress1);
        form.append("residentAddress2", this.residentAddress2);
        form.append("residentDistrict", this.residentDistrict);
        form.append("residentZipCode", this.residentZipCode);
        form.append("residentState", this.residentState);
        form.append("residentCountry", this.residentCountry);

        form.append("phoneNumber1", this.phoneNumber1);
        form.append("phoneNumber2", this.phoneNumber2);
        form.append("email", this.email);
        form.append("regDate", new Date().getTime());
        form.append("status", this.status);
        form.append("staffType", this.staffType);
        form.append("currency", this.currency);
        form.append("kinFirstName", this.kinFirstName);
        form.append("kinMiddleName", this.kinMiddleName);
        form.append("kinLastName", this.kinLastName);
        form.append("kinRelationship", this.kinRelationship);
        form.append("kinAddress", this.kinAddress);
        form.append("kinEmail", this.kinEmail);
        form.append("kinPhoneNumber", this.kinPhoneNumber);
        form.append("password", this.password);
        form.append("cPassword", this.cPassword);
        form.append("autoRegister", true);
        if (this.editState) {
          this.updateUser(form);
        } else {
          this.createUser(form);
        }
      }
    },

    processCreateUser() {
      if (this.password == "") {
        this.showResponseMsg("Please fill in password for this user.", false);
      } else if (this.password != this.cPassword) {
        this.showResponseMsg("Sorry, passwords do not match.", false);
      } else if (
        this.username == this.user.username ||
        this.email == this.user.email ||
        this.phoneNumber1 == this.user.phoneNumber1
      ) {
        this.showResponseMsg(
          "Please choose different username, email and phone number 1.",
          false
        );
      } else {
        this.processUserData();
      }
    },

    processUpdateUser() {
      this.editId = this.$route.params.id;
      this.editState = true;
      this.processUserData();
    },

    async resetUser() {
      try {
        await this.$axios.patch(`/users/reset/${this.editId}`);
        this.showResponseMsg("User data was reset successfully.", true);
      } catch (err) {
        this.showResponseMsg(err, true);
      }
    },

    async createUser(form) {
      try {
        await this.$axios.post(`/users/signup`, form);
        this.showResponseMsg("The user was created successfully.", true);
      } catch (err) {
        this.showResponseMsg(err.response.data.message, false);
      }
    },

    async updateUser(form) {
      try {
        await this.$axios.patch(`/users/edit-user/${this.editId}`, form);
        this.showResponseMsg("The user was updated successfully", true);
      } catch (err) {
        this.showResponseMsg(err.response.data.message, false);
      }
    },

    async getAUser(id) {
      try {
        const result = await this.$axios.get(`/users/?id=${id}`);
        this.populateUserData(result.data);
        this.user = result.data;
        this.getAccount(this.user.username);
      } catch (err) {
        console.log(err.response);
      }
    },

    async getCurrencies() {
      try {
        const result = await this.$axios.get("/currency");
        this.currencies = result.data.data;
      } catch (err) {
        console.log(err.response.data.message);
      }
    },

    async getAccount(username) {
      try {
        const result = await this.$axios.get(
          `/accounts/user-accounts/?username=${username}`
        );
        this.accounts = result.data.data;
      } catch (err) {
        console.log(err.response.data.message);
      }
    },
  },

  mounted() {
    const id = this.$route.query.id;
    this.getAUser(id);
  },

  computed: {
    countryArray() {
      return this.$store.state.registrationDetails.countries;
    },
    FILE_URL() {
      return this.$store.state.fileURL;
    },
  },
  components: { AdminNavigation, AdminTopHeader, AdminFooter, Instruction },
};
</script>

<style>
.div-block.pix {
  position: relative;
  overflow: hidden;
  border-radius: 50%;
}

.pix img {
  min-width: 100%;
  min-height: 100%;
  object-fit: cover;
}
.flex-wrap {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  width: 100%;
  background: whitesmoke;
  position: relative;
  border-radius: 5px;
  align-items: center;
  padding: 1px 10px;
  margin-bottom: 10px;
}

.flex-wrap label {
  font-size: 15px;
  color: #e74d8a;
}

.flex-wrap .delete {
  position: absolute;
  right: 10px;
  top: 10px;
}

.flex-wrap .delete {
  position: absolute;
  right: 30px;
  top: 10px;
}
</style>
