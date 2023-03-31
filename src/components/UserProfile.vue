<template>
  <div class="hello">
    <h1>My user profile</h1>
    <div v-if="user.status === true">
      <p>Gender : {{ user.gender }}</p>
      <p>Country : {{ user.country }}</p>
      <img :src="user.image_url" />
      <div>
        <button class="btn btn-primary" v-on:click="showForm">
          select another user
        </button>
      </div>
    </div>
    <div v-if="user.status === false">
      <form v-on:submit.prevent="submitForm">
        <div class="form-group">
          <label for="country">Country</label>
          <select
            name="country"
            class="form-control"
            id="country"
            v-model="form.country"
            required
          >
            <option
              v-for="(name, id) in countries[0]"
              v-bind:value="id"
              v-bind:key="id"
            >
              {{ name }}
            </option>
          </select>
        </div>
        <div class="form-group">
          <label for="gender">Gender</label>
          <select
            name="gender"
            class="form-control"
            id="refre"
            v-model="form.gender"
            required
          >
            <option value="male" selected>Male</option>
            <option value="female">Female</option>
          </select>
        </div>
        <div class="form-group">
          <button class="btn btn-primary">Submit</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "HelloWorld",
  data() {
    return {
      form: {
        country: "",
        gender: "",
      },
      user: {
        gender: "ridddhi",
        country: "",
        country_code: "",
        image_url: "",
        status: false,
      },
      countries: [
        {
          AU: "Australia",
          BR: "Brazil",
          CA: "Canada",
          CH: "China",
          DE: "Germany",
          DK: "Denmark",
          ES: "Spain",
          FI: "Finland",
          FR: "France",
          GB: "United Kingdom",
          IE: "Ireland",
          IN: "India",
          IR: "Iran",
          MX: "Mexico",
          NL: "Netherlands",
          NO: "Norway",
          NZ: "New Zealand",
          RS: "Russia",
          TR: "Turkey",
          US: "United States",
          UA: "Ukraine",
        },
      ],
    };
  },
  methods: {
    submitForm() {
      axios
        .get(
          "http://localhost:8000/user?country=" +
            this.form.country +
            "&gender=" +
            this.form.gender
        )
        .then((res) => {
          console.log(res.data);
          this.user.gender = res.data.gender;
          this.user.country = res.data.country;
          this.user.country_code = res.data.country_code;
          this.user.image_url = res.data.image_url;
          this.user.status = true;
        })
        .catch((error) => {
          console.log(error);
        })
        .finally(() => {
          //Perform action in always
        });
    },
    showForm() {
      this.user.status = false;
    },
  },
  props: ["image_url"],
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
a {
  color: #42b983;
}
label {
  margin-right: 20px;
}
</style>
