<template>
  <b-container>
    <alert :message="message" v-if="showMessage"></alert>
    <!-- <b-row class="mt-4"> -->
    <div v-if="!unitHouses[0]">No houses in the database</div>
    <b-card-group deck v-else>
      <div v-for="house in unitHouses" :key="house.id">
        <card
          :name="house.building_no"
          :address="house.address"
          :id="house._id.$oid"
        ></card>
      </div>
    </b-card-group>
    <!-- </b-row> -->
    <b-row>
      <b-col>
        <jw-pagination
          :items="Houses"
          @changePage="onChangePage"
          :pageSize="5"
          :styles="customStyles"
        ></jw-pagination>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
// import axios from 'axios';
import { mapGetters, mapActions } from "vuex";
import { required } from "vuelidate/lib/validators";
import JWPagination from "jw-vue-pagination";
import Alert from "../components/Alert.vue";
import Card from "../components/Card.vue";

const customStyles = {
  li: {
    display: "inline-block",
  },
  a: {
    color: "blue",
  },
};

export default {
  title: 'SLDB - Home Page',
  data() {
    return {
      loginForm: {
        username: "",
        password: "",
      },
      submitted: false,
      message: "",
      showMessage: false,
      customStyles,
      unitHouses: [],
    };
  },
  components: {
    alert: Alert,
    card: Card,
    "jw-pagination": JWPagination,
  },
  validations: {
    loginForm: {
      firstName: { required },
      username: { required },
    },
  },
  computed: {
    ...mapGetters({ Houses: "StateAllHouses", User: "StateUser" }),
  },
  methods: {
    ...mapActions(["GetAllHouses"]),
    // async getHouses() {
    //   const x = await this.GetHouses();
    //   // eslint-disable-next-line
    //   console.log(x);
    // },

    onSubmit(e) {
      e.preventDefault();
      this.$refs.loginModal.hide();
      this.submitted = true;
      // stop here if form is  invalid
      this.$v.$touch();
      if (this.$v.$invalid) {
        return;
      }
      // eslint-disable-next-line no-console
      console.log(`success!:-)\n\n${JSON.stringify(this.loginForm)}`);
    },
    onChangePage(unitHouses) {
      // update page of items
      this.unitHouses = unitHouses;
    },
  },
  created() {
    this.GetAllHouses();
  },
};
</script>

<style>
</style>
