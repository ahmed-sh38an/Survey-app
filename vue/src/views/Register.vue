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
      Register account
    </h2>
    <p class="mt-2 text-center text-sm text-gray-600">
      Or
      {{ " " }}
      <router-link
        :to="{ name: 'Login' }"
        class="font-medium text-indigo-600 hover:text-indigo-500"
        >Login to your account</router-link
      >
    </p>
  </div>
  <form class="mt-8 space-y-6" @submit="register">
    <Alert
      v-if="Object.keys(errors).length"
      class="flex-col items-stretch text-sm"
    >
      <div v-for="(field, i) of Object.keys(errors)" :key="i">
        <div v-for="(error, ind) of errors[field] || []" :key="ind">
          * {{ error }}
        </div>
      </div>
      <span
        @click="errors = ''"
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
        <label for="fullname" class="sr-only">Full Name</label>
        <input
          id="fullname"
          name="name"
          type="text"
          autocomplete="name"
          required=""
          class="relative block w-full appearance-none rounded-none rounded-t-md border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 focus:z-10 focus:border-indigo-500 focus:outline-none focus:ring-indigo-500 sm:text-sm"
          placeholder="Full Name"
          v-model="user.name"
        />
      </div>
      <div>
        <label for="email-address" class="sr-only">Email address</label>
        <input
          id="email-address"
          name="email"
          type="email"
          autocomplete="email"
          required=""
          class="relative block w-full appearance-none rounded-none border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 focus:z-10 focus:border-indigo-500 focus:outline-none focus:ring-indigo-500 sm:text-sm"
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
          class="relative block w-full appearance-none rounded-none border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 focus:z-10 focus:border-indigo-500 focus:outline-none focus:ring-indigo-500 sm:text-sm"
          placeholder="Password"
          v-model="user.password"
        />
      </div>
      <div>
        <label for="password_confirmation" class="sr-only"
          >Password Confirmation</label
        >
        <input
          id="password_confirmation"
          name="password_confirmation"
          type="password"
          autocomplete="current-password_confirmation"
          required=""
          class="relative block w-full appearance-none rounded-none rounded-b-md border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 focus:z-10 focus:border-indigo-500 focus:outline-none focus:ring-indigo-500 sm:text-sm"
          placeholder="Password Confirmation"
          v-model="user.password_confirmation"
        />
      </div>
    </div>

    <div>
      <button
        type="submit"
        class="group relative flex w-full justify-center rounded-md border border-transparent bg-indigo-600 py-2 px-4 text-sm font-medium text-white hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2"
      >
        <span class="absolute inset-y-0 left-0 flex items-center pl-3">
          <LockClosedIcon
            class="h-5 w-5 text-indigo-500 group-hover:text-indigo-400"
            aria-hidden="true"
          />
        </span>
        Sign Up
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
  name: "",
  email: "",
  password: "",
  password_confirmation: "",
};

const errors = ref({});

function register(ev) {
  ev.preventDefault();
  store
    .dispatch("register", user)
    .then((res) => {
      router.push({
        name: "Dashboard",
      });
    })
    .catch((err) => {
      if (err.response.status === 422) {
        errors.value = err.response.data.errors;
      }
    });
}
</script>
