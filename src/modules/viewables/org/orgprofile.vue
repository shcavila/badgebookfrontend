<template>
  <div id="Profile">
      <div class="prfl" v-bind:class="{small: resized}">
        <div class="logo"  v-bind:class="{smallLogo: resized}">
          <img :src="image" alt class="profile"  v-bind:class="{smallpro: resized}">
        </div>
        <h4 class="orgn">{{ orgName }}</h4>
         <p class="username">Since 1999</p>
      </div>
      <div class="info"  v-bind:class="{smallinfo: resized}">
        <!-- <p>Organization name: {{orgName}}</p> -->
        <p class="des">{{description}}</p>
        <p class="address">{{address}}</p>
        <p class="email">
          Email at
          <span class="emailadd">{{email}}</span>
        </p>
      </div>
  </div>
</template>

<script>
/*eslint linebreak-style: ["error", "windows"]*/
import $ from "jquery";
import axios from 'axios'
export default {
  name: "Profile",
  props: {
    userInfo: Object
  },
  data() {
    return {
      orgName: "",
      address: "",
      email: "",
      description: "",
      image: "",
      size: 0,
      resized: false,
    };
  },
  created() {
    window.addEventListener("resize", this.handleResize);
    this.size = window.innerWidth;
    this.handleResize();

    let url = "http://localhost:8081/auth/getuser";
    let body = { username: this.$route.params.username };
    axios.post(url, body).then(resp => {
      let user = resp.data.user
      this.orgName = user.orgName;
      this.address = user.address;
      this.email = user.email;
      this.description = user.description;
      this.image = `http://localhost:8081/static/${user.profile}`
    });
  },

  destroyed() {
    window.removeEventListener("resize", this.handleResize);
  },
  methods: {
    redirect(path) {
      this.$router.push(path);
    },
    handleResize() {
      // if (window.innerWidth >= this.size) {
        if (window.innerWidth < 900) {

            this.resized = true;
          
        } else {
          this.resized = false;
        }
      // } else if (window.innerWidth < this.size) {
      //   this.resized = true;
      // }
    },
    InsideMethod() {
      this.$emit("InsideMethod");
    }
  }
};
</script>
<style scoped>
.username {
  font-size: 17px;
  margin-top: 0px;
  color: #315273;
  width: 100%;
}
div {
  font-family: verdana;
}

.orgn {
   color:#364452;
   margin-bottom: 0;
}

.address {
  font-size: 15px;
  color: #3b4d70;

  font-weight: bold;
}

.des {
  color: #1e4773;
}
.email {
  color: #486399;
  font-size: 15px;
}
.emailadd {
  text-decoration: underline;
}
.prof {
  float: left;
}
p {
  margin-bottom: 5px;
  margin-top: 5px;
}

.profile {
  /* height: 200px; */

  width: auto;
  height: 250px;
}

.btn {
  border-radius: 0;
}

#Profile {
  height: 150px;
  background-image: url("~@/assets/orgbackground.jpg");
  background-size: cover;
  font-family: verdana;
}
.info {
  float: left;
  font-size: 14px;
  width: 74%;
  text-align: left;
  margin-top: 155px;
  padding: 10px;
}

.logo {
  overflow: hidden;
  margin-top: 30px;
  width: 250px;
  margin-bottom: 10px;
  -webkit-box-shadow: 0px 1px 5px #0e222b;
  box-shadow: 0px 1px 5px #0e222b;
  height: 250px;
  border:1px solid white;
  background: white;
}
.prfl {
  float: left;
  width: 25%;
  /* margin-left:25px; */
  /* margin-right: 25px; */
  margin-bottom: 0px;
  border-bottom: 2px solid #d5e7f5;
  /* background:grey; */
}

.small {
  width: 100%;
  margin: 0;
}

.smallinfo {
  width: 100%;
  margin-top: 4px;
  text-align: center;
}
.update {
  border-radius:2px;
}
.smallLogo {
  width: 180px;
  height: 180px;
  overflow: hidden;
}
.smallpro {
  width: auto;
  height: 180px;
}
</style>
