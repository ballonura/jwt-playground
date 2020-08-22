<template>
  <div class="bg-black text-white">
    <div class="h-screen container flex flex-col justify-center items-center">
      <h1 class="text-green-400 text-5xl lg:text-6xl mb-8 text-center">
        JWT Playground
      </h1>
      <div class="flex flex-col justify-center lg:flex-row w-full text-xl mb-6">
        <div class="flex justify-center lg:pr-4 mb-4 lg:mb-0 flex-1">
          <button @click="obtainJWT" class="button">Obtain JWT</button>
        </div>
        <div class="flex justify-center lg:px-4 mb-4 lg:mb-0 flex-1">
          <button @click="msg = ''" class="button">Reset JWT</button>
        </div>
        <div class="flex justify-center lg:px-4 mb-4 lg:mb-0 flex-1">
          <button @click="accessPrivateRoute" class="button button-middle">
            Access private route
          </button>
        </div>
        <div class="flex justify-center lg:pl-4 mb-4 lg:mb-0 flex-1">
          <button @click="accessPublicRoute" class="button">
            Access public route
          </button>
        </div>
      </div>
      <div
        v-if="error"
        class="border border-red-600 text-red-500 w-full rounded-lg px-3 py-2 mb-6"
      >
        {{ error }}
      </div>
      <div
        v-if="msg"
        class="border border-white text-white rounded-lg px-3 py-2 max-w-full"
        style="word-wrap: break-word;"
      >
        <p>
          {{ msg }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      baseURL: "https://jwt-playground.herokuapp.com",
      msg: "",
      error: false,
    };
  },
  methods: {
    async obtainJWT() {
      try {
        this.error = "";

        const {
          data: { accessToken },
        } = await axios.get(`${this.baseURL}/obtain-jwt`);
        this.msg = accessToken;
      } catch (error) {
        this.error = error;
      }
    },
    async accessPrivateRoute() {
      try {
        const {
          data: { msg },
        } = await axios.get(`${this.baseURL}/private-route`, {
          headers: { Authorization: `Bearer ${this.msg}` },
        });
        this.error = "";
        this.msg = msg;
      } catch (error) {
        this.msg = "";
        this.error = "Please obtain JWT before access private route";
      }
    },
    async accessPublicRoute() {
      try {
        const {
          data: { msg },
        } = await axios.get(`${this.baseURL}/public-route`);
        this.error = "";
        this.msg = msg;
      } catch (error) {
        this.msg = "";
        this.error = error;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
@import url("https://fonts.googleapis.com/css2?family=Roboto+Mono:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;1,100;1,200;1,300;1,400;1,500;1,600;1,700&display=swap");

.button {
  @apply px-3 py-2 flex-1 border-2 border-green-600 rounded-lg font-medium text-green-500 transition duration-200;

  &:focus {
    outline: none;
  }
  &:hover {
    @apply border-white text-white;
  }
}
</style>
