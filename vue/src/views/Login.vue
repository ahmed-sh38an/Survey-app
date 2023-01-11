<template>
  <div>
    <img
      class="mx-auto h-12 w-auto"
      src="https://tailwindui.com/img/logos/mark.svg?color=indigo&shade=600"
      alt="Your Company"
    />
    <h2
      class="mt-6 text-center text-3xl font-bold tracking-tight text-gray-900"
    >
      Sign in to your account
    </h2>
    <p class="mt-2 text-center text-sm text-gray-600">
      Or
      {{ " " }}
      <router-link
        :to="{ name: 'Register' }"
        class="font-medium text-indigo-600 hover:text-indigo-500"
        >Register for free</router-link
      >
    </p>
  </div>
  <form class="mt-8 space-y-6" @submit="login">
    <Alert v-if="errorMsg">
      {{ errorMsg }}
      <span
        @click="errorMsg = ''"
        class="w-8 h-8 flex items-center justify-center rounded-full transition-colors cursor-pointer hover:bg-[rgba(0,0,0.2)]"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
          class="w-6 h-6"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M6 18L18 6M6 6l12 12"
          />
        </svg>
      </span>
    </Alert>
    <input type="hidden" name="remember" value="true" />
    <div class="-space-y-px rounded-md shadow-sm">
      <div>
        <label for="email-address" class="sr-only">Email address</label>
        <input
          id="email-address"
          name="email"
          type="email"
          autocomplete="email"
          required=""
          class="relative block w-full appearance-none rounded-none rounded-t-md border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 focus:z-10 focus:border-indigo-500 focus:outline-none focus:ring-indigo-500 sm:text-sm"
          placeholder="Email address"
          v-model="user.email"
        />
      </div>
      <div>
        <label for="password" class="sr-only">Password</label>
        <input
          id="password"
          name="password"
          type="password"
          autocomplete="current-password"
          required=""
          class="relative block w-full appearance-none rounded-none rounded-b-md border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 focus:z-10 focus:border-indigo-500 focus:outline-none focus:ring-indigo-500 sm:text-sm"
          placeholder="Password"
          v-model="user.password"
        />
      </div>
    </div>

    <div class="flex items-center justify-between">
      <div class="flex items-center">
        <input
          id="remember-me"
          name="remember-me"
          type="checkbox"
          class="h-4 w-4 rounded border-gray-300 text-indigo-600 focus:ring-indigo-500"
          v-model="user.remember"
        />
        <label for="remember-me" class="ml-2 block text-sm text-gray-900"
          >Remember me</label
        >
      </div>

      <div class="text-sm">
        <a href="#" class="font-medium text-indigo-600 hover:text-indigo-500"
          >Forgot your password?</a
        >
      </div>
    </div>

    <div>
      <button
        :disabled="loading"
        type="submit"
        class="group relative flex w-full justify-center rounded-md border border-transparent bg-indigo-600 py-2 px-4 text-sm font-medium text-white hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2"
        :class="{
          'cursor-not-allowed': loading,
          'hover:bg-indigo-500': loading,
        }"
      >
        <div role="status">
          <svg
            v-if="loading"
            aria-hidden="true"
            class="w-5 h-5 mr-2 text-gray-200 animate-spin dark:text-gray-600 fill-blue-600"
            viewBox="0 0 100 101"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z"
              fill="currentColor"
            />
            <path
              d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z"
              fill="currentFill"
            />
          </svg>
          <span class="sr-only">Loading...</span>
        </div>

        <span class="absolute inset-y-0 left-0 flex items-center pl-3">
          <LockClosedIcon
            class="h-5 w-5 text-indigo-500 group-hover:text-indigo-400"
            aria-hidden="true"
          />
        </span>
        Sign in
      </button>
    </div>
  </form>
</template>

<script setup>
import { LockClosedIcon } from "@heroicons/vue/20/solid";
import { useRouter } from "vue-router";
import store from "../store";
import { ref } from "vue";
import Alert from "../components/Alert.vue";

const router = useRouter();
const user = {
  email: "",
  password: "",
  remember: false,
};

const loading = ref(false);
let errorMsg = ref("");

function login(ev) {
  ev.preventDefault();
  loading.value = true;
  store
    .dispatch("login", user)
    .then(() => {
      loading.value = false;
      router.push({
        name: "Dashboard",
      });
    })
    .catch((err) => {
      loading.value = false;
      errorMsg.value = err.response.data.error;
    });
}
</script>
