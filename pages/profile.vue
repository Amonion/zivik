<template>
  <div class="dashboard-wrapper profile">
    <!-- <div class="alert-body" :class="{ hide: !alertBoxStatus }">
      <div class="alert-box">
        <img
          src="/images/Colored-Logo.png"
          loading="lazy"
          alt=""
          class="alert-logo"
        />
        <div class="notice-msg">
          {{ alertBoxMsg }}
        </div>
        <div class="button-holder">
          <span @click="confirmLogout" class="btn-custom w-button">Yes</span>
        </div>
      </div>
    </div> -->
    <dashboard-navigation />
    <div class="dashboard-content">
      <dashboard-header />
      <div class="content-body">
        <div class="dashboard-card-wrap">
          <div class="personal-form w-form">
            <div v-if="user" class="profile-picture">
              <div
                v-if="user.profilePicture"
                class="image"
                :style="{
                  backgroundImage: `url(${FILE_URL}/${user.profilePicture})`,
                }"
              ></div>
              <img
                v-else
                :src="avatar"
                loading="lazy"
                alt=""
                class="image-37"
              />
            </div>
            <div class="upload-div">
              <label for="file" class="picture-label">
                <input
                  type="file"
                  id="file"
                  class="idden"
                  @change="setProfile"
                />
                <img src="/images/upload.png" alt="" class="upload-icon" />
              </label>
            </div>
            <div v-if="user" class="form-flex">
              <div class="each-form-field">
                <label for="name" class="label">Username</label
                ><input
                  type="text"
                  class="profile-input w-input"
                  :placeholder="user.username"
                  disabled
                />
              </div>
              <div class="each-form-field">
                <label for="name-2" class="label">First Name</label
                ><input
                  type="text"
                  class="profile-input w-input"
                  :placeholder="user.firstName"
                  disabled
                />
              </div>
              <div class="each-form-field">
                <label for="name-3" class="label">Middle Name</label
                ><input
                  type="text"
                  class="profile-input w-input"
                  :placeholder="user.middleName"
                  disabled
                />
              </div>
              <div class="each-form-field">
                <label for="name-3" class="label">Last Name</label
                ><input
                  type="text"
                  class="profile-input w-input"
                  :placeholder="user.lastName"
                  disabled
                />
              </div>
              <div class="each-form-field">
                <label for="name-3" class="label">Account Number</label
                ><input
                  type="text"
                  class="profile-input w-input"
                  :placeholder="user.accountNumber"
                  disabled
                />
              </div>
              <div class="each-form-field">
                <label for="name-3" class="label">Swift Number</label
                ><input
                  type="text"
                  class="profile-input w-input"
                  :placeholder="user.swiftCode"
                  disabled
                />
              </div>
              <div class="each-form-field">
                <label for="name-2" class="label">Date of Birth</label
                ><input
                  type="text"
                  class="profile-input w-input"
                  disabled
                  :placeholder="`${new Date(user.dob * 1).getUTCDate()}/${
                    new Date(user.dob).getMonth() * 1 + 1
                  }/${new Date(user.dob).getFullYear()}`"
                />
              </div>
              <div class="each-form-field">
                <label for="name-2" class="label">Income Source</label
                ><input
                  type="text"
                  class="profile-input w-input"
                  :placeholder="user.income"
                  disabled
                />
              </div>
            </div>
          </div>
        </div>

        <dashboard-balance />

        <profile-settings />

        <comment-settings />

        <div class="dashboard-card-wrap">
          <div class="personal-form w-form">
            <form class="form-flex">
              <div class="each-form-field">
                <label for="name-2" class="label">New Code</label
                ><input
                  type="number"
                  class="profile-input w-input"
                  v-model="code"
                  placeholder="Enter New Code"
                  maxlength="6"
                />
              </div>

              <div class="each-form-field">
                <label for="name-4" class="label">Confirm Code</label
                ><input
                  type="number"
                  class="profile-input w-input"
                  v-model="cCode"
                  placeholder="Confirm Code"
                  maxlength="6"
                />
              </div>

              <div v-if="showMsg" class="msg" :class="{ error: !colour }">
                {{ msg }}
              </div>
              <div class="button-holder">
                <span @click="processCode" class="btn-custom w-button"
                  >Create Code</span
                >
              </div>
            </form>
          </div>
        </div>
      </div>
      <dashboard-footer />
    </div>
  </div>
</template>

<script>
import CommentSettings from "../components/CommentSettings";
import DashboardBalance from "../components/DashboardBalance.vue";
import ProfileSettings from "../components/ProfileSettings.vue";
export default {
  components: { ProfileSettings, CommentSettings, DashboardBalance },
  data() {
    return {
      code: "",
      cCode: "",
      avatar: "/images/avatar.svg",
      profilePicture: "",

      msg: "",
      colour: false,
      showMsg: false,
      alertBoxStatus: false,
      alertBoxMsg: "",
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

    async logout() {
      await this.$auth.logout(); // this method will logout the user and make token to false on the local storage of the user browser
    },

    cleareInputs() {
      this.cCode = "";
      this.code = "";
    },

    async setProfile(e) {
      const [file] = e.target.files;
      this.profilePicture = file;
      this.avatar = URL.createObjectURL(file);
      const form = new FormData();
      form.append("profilePicture", this.profilePicture);
      // form.append("dob", 504921600000);

      const result = await this.$axios.post(`/users/?id=${this.user.id}`, form);
      if (!result.response) {
        this.$store.commit("setUser", result.data);
      }
    },

    async processCode() {
      if (this.code.trim() == "" || this.code.trim().length < 6) {
        this.showMessage("Pin should be at least 6 digits long");
        return;
      }
      if (this.code != this.cCode) {
        this.showMessage("Sorry! Pins do not match, try again");
        return;
      }

      const form = {
        pin: this.code,
      };

      try {
        const result = await this.$axios.patch(
          `/users/?id=${this.user.id}`,
          form
        );
        this.colour = true;
        this.showMessage("Your code wass changed successfully");
        this.clearInputs();
      } catch (err) {
        this.showMessage(err.response.data.message);
      }
    },
  },

  computed: {
    FILE_URL() {
      return this.$store.state.fileURL;
    },

    user() {
      return this.$store.state.auth.user;
    },
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
.upload-div {
  display: flex;
  justify-content: center;
  width: 100%;
  margin: 10px 0;
}

.upload-icon {
  width: 20px;
  margin-right: 0;
}

.idden {
  width: 0.1;
  opacity: 0;
  z-index: -1;
  position: absolute;
}

.picture-label {
  cursor: pointer;
  position: relative;
}
</style>
