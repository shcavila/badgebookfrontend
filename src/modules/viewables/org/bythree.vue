<template>
  <div id="bythree">
    <b-col class="holder" v-for="(badge, index) in this.data" :key="index">
      <div class="box" @click="viewInfo(badge)" v-bind:class="{small: resized}">
        <div class="imgholder" v-bind:style='{backgroundImage: `url("${badge.imgnum}")`}'>
        </div>
        <img src="@/assets/image2.png" class="blogo">
        <div class="tholder">
          <h5 class="binfo bname">{{badge.badgename}}</h5>
          <p class="binfo">{{badge.venue}}</p>
          <p class="binfo date" >{{ badge.date.month+" "+badge.date.day+" "+badge.date.year }}</p>
          <!-- <p class="binfo rec">Given to&nbsp;<span class="num">{{ badge.recipient.length }}</span>&nbsp;<span v-if="badge.recipient > 1">recipients</span> -->
          <!-- <span v-else>recipient</span></p> -->
        </div>
      </div>
    </b-col>
    <b-modal
      class="modl"
      id="badgeinfo"
      size="lg"
      title="Certificate information"
      centered
      hide-footer
    >
    <div class="cerBody">
      <p class="name">
        This certificate of
        <br>
        <span class="cern">{{badge.certificateName}}</span>
      </p>
      <center>
      <span class="awardedto">is awarded to</span>
      <div class="rcpnt">
      <span v-for="(rec, i) in badge.recipient" :key="i">{{" "+rec.fullname}}
      <span v-if="i == badge.recipient.length - 2">&</span>
      <span v-else>
      <span v-if="i != badge.recipient.length - 1">,</span>
      </span>
      </span>
      </div>
      </center>
      <br>
      <p class="description">{{badge.descriptions}}</p>
      <br>
      <p>Given this {{ badge.date.month+" "+badge.date.day+", "+badge.date.year }}</p>
      <div class="byorg">
        <h5>Given by {{badge.organization}}</h5>
      </div>
      </div>
    </b-modal>
  </div>
</template>
<script>
import $ from "jquery";
import "@/Styles/cerStyle.css";

export default {
  name: "bythree",
  data() {
    return {
      size: 0,
      resized: false,
      hidden: true,
      badge: {
        granted: "",
        code: "",
        badgename: "",
        venue: this.venue,
        recipient: [],
        certificateName: "",
        descriptions: "",
        organization: "",
        date: {
          month: "",
          day: "",
          year: ""
        }
      }       
  }
  },
  props: {
    data: Array
  },
  created() {
    window.addEventListener("resize", this.handleResize);
    this.size = window.innerWidth;
    this.handleResize();
  },
  destroyed() {
    window.removeEventListener("resize", this.handleResize);
  },
  methods: {
    handleResize() {
      if (window.innerWidth < 900) {
        this.resized = true;
        
      } else {
        this.resized = false;
      }
    },
    viewInfo(badge) {
      this.$bvModal.show("badgeinfo");
      this.badge = badge;
      this.hidden = false;
    }, 
    close() {
      this.$bvModal.hide("badgeinfo");
      this.hidden = true;
    }
  }, 
  watch: {
    // hidden(val) {
    //   if (!val) {
    //     document.body.style.position = 'fixed';
    //     document.body.style.top = `-${window.scrollY}px`;
    //   } else {
    //     const scrollY = document.body.style.top;
    //     document.body.style.position = '';
    //     document.body.style.top = '';
    //     window.scrollTo(0, parseInt(scrollY || '0') * -1);
    //   }
    // }
  }
};
</script>

<style scoped>
.cerBody {
  width: 100%;
  padding-left: 20px;
  padding-right: 20px;
  padding-top: 30px;
  padding-bottom: 20px;
  color: #3d4c54;
  background: #e8f3fa;
  text-align: center;
  font-family: Verdana;
  font-size: 16px;
  height: 390px;
  /* border-right:6px solid #779cb5; */
  /* border-bottom:8px solid #e8f3fa; */
}

.cern {
  font-size:19px;
}

.rcpnt {
  width:60%;
  border-bottom: 1px solid #c4d2dd;
}

.description {
  font-size: 17px;
}

.name {
  font-size: 18px;
}

.imgholder {
   -webkit-filter: brightness(60%); /* Safari 6.0 - 9.0 */
  filter: brightness(60%);
  background-size: cover;
  margin: 0;
  height: 250px;
  text-align: left;
  transition: ease .2s
}

.imgholder:hover {
  -webkit-filter: brightness(90%); /* Safari 6.0 - 9.0 */
  filter: brightness(90%);
}
.blogo {
  top:14px;
  left:0;
  width: 100px;
  position: absolute;
   -webkit-filter: brightness(130%); /* Safari 6.0 - 9.0 */
  filter: brightness(130%);
}

.tholder {
  padding: 5px;
}
.box {
  width: 279px;
  overflow: auto;
  height: 400px;
  text-align: center;
  background: white;
  float: left;
  border-radius: 0;
  margin: 10px;
  padding: 0;
  color: #3b4d70;
  border: lightgrey;
  -webkit-box-shadow: 0px 1px 6px #8492a1;
  box-shadow: 0px 1px 6px #8492a1;
  position: relative;
  /* transition: ease 0.9s; */
}

.box:hover {
  margin-top: 0;
  margin-bottom: 20px;
  -webkit-box-shadow: 0px 4px 10px #8492a1;
  box-shadow: 0px 4px 10px #8492a1;
}
.small {
  width: 98%;
}
.holder {
  margin: 0px;
  padding:0;
}
.binfo {
  margin-bottom: 1px;
  margin-top: 1px;
  color:#214d70;
}
.rec {
  font-size: 12px;
  color:#2e77b0;
  padding:0px;
  /* background:#d5e5f0; */
  /* border-radius: 5px; */
}
.num  {
  margin-top:0;
  padding:0;
  margin-bottom: 0;
  font-size: 15px;
}

.date {
  font-size: 14px;
  margin-bottom: 0;
}
.bname {
  color: #1a4f78;
}

.text-center {
  background: lightblue;
}
#bythree {
  padding: 0;
  margin: 0;
}

.row {
  border-color: white;
}

.nmar2 {
  margin-right: 0;
}
</style>
