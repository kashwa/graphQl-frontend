<template>
  <div id="app" class="font-sans antialiased text-black">
    <div id="nav" class="bg-purple-dark">
      <div class="container flex item-center justify-between py-6">
        <div>
          <a href>
            <img src="./assets/logo.svg" alt="logo" srcset />
          </a>
        </div>
        <div>
          <router-link to="/" class="text-white hover:text-grey-light pl-6">Home</router-link>
          <router-link to="/about" class="text-white hover:text-grey-light pl-6">About</router-link>
        </div>
      </div>
    </div>
    <router-view />
  </div>
</template>

<style src="./assets/main.css">
