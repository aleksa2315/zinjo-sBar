<script setup>
import { RouterLink, RouterView, useRoute } from 'vue-router'
import store from "@/store/config.js";
import {onMounted} from "vue";
import {jwtDecode} from "jwt-decode";
const isRouteActive = (route) => useRoute().path === route
const activeRoute =
  'text-white bg-blue-700 md:bg-transparent md:text-blue-700 md:dark:text-blue-500'
const inactiveRoute =
  'text-gray-900 hover:bg-gray-100 md:hover:bg-transparent md:hover:text-blue-700 md:dark:hover:text-blue-500 dark:text-white dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent dark:border-gray-700'
const logout = () => {
  store.token = ''
  localStorage.clear()
  store.loggedIn = false
}
onMounted(async () => {
  const storedCart = localStorage.getItem('cart');
  if (storedCart && store.cart < 1) {
    store.cart = JSON.parse(storedCart);
  }
  const userAuth = localStorage.getItem('jwtToken');
  if (userAuth && !store.loggedIn) {
    const decoded = jwtDecode(userAuth)
    store.loggedIn = true;
    store.admin = decoded.admin;
  }
})

</script>

<template>
  <header :class="{ hidden: isRouteActive('/login') || isRouteActive('/register') }">
    <nav
      class="bg-white dark:bg-gray-900 fixed w-full z-20 top-0 start-0 border-b border-gray-200 dark:border-gray-600"
    >
      <div class="max-w-screen-xl flex flex-wrap items-center justify-between mx-auto p-4">
        <router-link to="/" class="flex items-center space-x-3 rtl:space-x-reverse">
          <img src="../public/bar.png" class="h-8" alt="Flowbite Logo" />
          <span class="self-center text-2xl font-semibold whitespace-nowrap dark:text-white"
            >Zinjo's bar</span
          >
        </router-link>
        <div class="flex md:order-2 space-x-3 md:space-x-0 rtl:space-x-reverse" v-if="!store.loggedIn">
          <router-link
            to="/login"
            class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-2 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
            >Login</router-link
          >
          <router-link
            to="/register"
            class="hover:underline focus:outline-none font-medium text-sm px-4 py-2 text-center text-black"
            >Register</router-link
          >
          <button
            onclick="document.getElementById('navbar-sticky').classList.toggle('hidden')"
            type="button"
            class="inline-flex items-center p-2 w-10 h-10 justify-center text-sm text-gray-500 rounded-lg md:hidden hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-gray-200 dark:text-gray-400 dark:hover:bg-gray-700 dark:focus:ring-gray-600"
          >
            <svg
              class="w-5 h-5"
              aria-hidden="true"
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 17 14"
            >
              <path
                stroke="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M1 1h15M1 7h15M1 13h15"
              />
            </svg>
          </button>
        </div>
        <div class="flex md:order-2 space-x-3 md:space-x-0 rtl:space-x-reverse" v-if="store.loggedIn">
          <button
              @click="logout"
              class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-2 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
          >Logout</button
          >
        </div>
        <div
          class="items-center justify-between hidden w-full md:flex md:w-auto md:order-1"
          id="navbar-sticky"
        >
          <ul
            class="flex flex-col p-4 md:p-0 mt-4 font-medium border border-gray-100 rounded-lg bg-gray-50 md:space-x-8 rtl:space-x-reverse md:flex-row md:mt-0 md:border-0 md:bg-white dark:bg-gray-800 md:dark:bg-gray-900 dark:border-gray-700"
          >
            <li>
              <router-link
                to="/"
                onclick="document.getElementById('navbar-sticky').classList.toggle('hidden')"
                :class="{ [activeRoute]: isRouteActive('/'), [inactiveRoute]: !isRouteActive('/') }"
                class="block py-2 px-3 rounded md:p-0"
                >Home</router-link
              >
            </li>
            <li>
              <router-link
                to="/cart"
                onclick="document.getElementById('navbar-sticky').classList.toggle('hidden')"
                :class="{
                  [activeRoute]: isRouteActive('/cart'),
                  [inactiveRoute]: !isRouteActive('/cart')
                }"
                class="block py-2 px-3 rounded md:p-0"
                >Cart</router-link
              >
            </li>
          </ul>
        </div>
      </div>
    </nav>
  </header>
  <main
    :class="{
      'pt-0': isRouteActive('/login') || isRouteActive('/register'),
      'pt-20': !isRouteActive('/login') && !isRouteActive('/register')
    }"
  >
    <RouterView />
  </main>
</template>
