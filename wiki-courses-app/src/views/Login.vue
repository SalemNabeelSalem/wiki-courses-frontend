<template>
  <div id="login">
    <div class="alert alert-danger" v-if="errorMessage">
      {{ errorMessage }}
    </div>

    <div class="container">
      <div class="row justify-content-center" style="margin:60px 0px">
        <div class="col-sm-6 col-md-4">
          <div class="account-wall">
            <img class="profile-img" src="img/resources/login/user.webp" />

            <form
              class="form-signin"
              method="post"
              @submit.prevent="handelSubmit"
            >
              <div class="form-group">
                <input
                  type="text"
                  class="form-control"
                  placeholder="Username"
                  required
                  autofocus
                  v-model="userLogin.fullName"
                />
              </div>

              <div>
                <input
                  type="password"
                  class="form-control"
                  placeholder="Password"
                  required
                  v-model="userLogin.email"
                />
              </div>

              <button class="btn btn-primary btn-block" type="submit">
                Login
              </button>

              <a href="/faq" class="pull-right need-help">Need help?</a
              ><span class="clearfix"></span>
            </form>
          </div>

          <a href="/sign-up" class="text-center new-account">
            Create an account
          </a>
        </div>
      </div>
    </div>

    <Footer />
  </div>
</template>

<script>
import axios from "axios";

// @ is an alias to /src
import Footer from "@/components/global/Footer.vue";

export default {
  name: "Login",
  data() {
    return {
      errorMessage: "",
      userLogin: {
        fullName: "",
        email: ""
      },
      userData: []
    };
  },
  methods: {
    handelSubmit() {
      axios
        .post("http://localhost:8383/api/v1/lecturer-login", this.userLogin)
        .then(response => {
          this.userData = response.data;

          console.log(this.userData);

          if (this.userData.id) {
            // console.log("lecturer id is not null.");

            this.goToLecturerControlPanel(this.userData.id, this.userData);
          } else {
            // console.log("lecturer id is null.");
            this.errorMessage = "Username or Password Is Not Correct.";
          }
        })
        .catch(error => {
          console.error("error worng username or password => ", error);
        });
    },
    goToLecturerControlPanel(lecturerId, lecturerData) {
      this.$router.push({
        name: "LecturerDashboard",
        params: { lecturerId: lecturerId, lecturerData: lecturerData }
      });
    }
  },
  components: {
    Footer
  }
};
</script>

<style lang="scss" scoped>
#login {
  .form-signin {
    max-width: 330px;
    padding: 15px;
    margin: 0 auto;
  }

  .form-signin .form-signin-heading,
  .form-signin .checkbox {
    margin-bottom: 10px;
  }

  .form-signin .checkbox {
    font-weight: normal;
  }

  .form-signin .form-control {
    position: relative;
    font-size: 16px;
    height: auto;
    padding: 10px;
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
  }

  .form-signin .form-control:focus {
    z-index: 2;
  }

  .form-signin input[type="text"] {
    margin-bottom: -1px;
    border-bottom-left-radius: 0;
    border-bottom-right-radius: 0;
  }

  .form-signin input[type="password"] {
    margin-bottom: 10px;
    border-top-left-radius: 0;
    border-top-right-radius: 0;
  }

  .account-wall {
    margin-top: 20px;
    padding: 40px 0px 20px 0px;
    background-color: #f7f7f7;
    -moz-box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
    -webkit-box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
    box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.3);
  }

  .profile-img {
    width: 96px;
    height: 96px;
    margin: 0 auto 10px;
    display: block;
    -moz-border-radius: 50%;
    -webkit-border-radius: 50%;
    border-radius: 50%;
  }

  .need-help {
    margin-top: 10px;
  }

  .new-account {
    display: block;
    margin-top: 10px;
  }
}
</style>
