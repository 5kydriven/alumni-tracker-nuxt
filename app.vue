<script setup>
useHead({
  title: 'Alumni Tracker',
  bodyAttrs: {
    class: 'test'
  },
})

const error = ref(false);
const errorMessage = ref('');
const username = ref('');
const password = ref('');
const loading = ref(false)

const getAuth = async () => {
  loading.value = true
  try {
    let usern = username.value;
    if (!usern.includes('@gmail.com')) {
      usern += '@gmail.com';
    }
    const res = await signInWithEmailAndPassword(auth, usern, password.value);

    console.log(res);
    error.value = false; // Reset error state on successful login
    loading.value = false
  } catch (err) {
    console.log(err.code);
    error.value = true; // Set error state to true when an error occurs
    loading.value = false
    switch (err.code) {
      case 'auth/invalid-email':
        errorMessage.value = 'Please input username or password';
        break;
      case 'auth/user-not-found':
        errorMessage.value = 'User is not registered';
        break;
      case 'auth/wrong-password':
        errorMessage.value = 'Wrong password';
        break;
      case 'auth/invalid-credential':
        errorMessage.value = 'Incorrect username or password';
        break;
      default:
        errorMessage.value = 'Please input password';
    }
  }
};
</script>

<template>
  <div class="flex bg-white md:flex-row w-full">

    <!-- left side -->
    <div class="relative">
      <img src="/cpsu.png" alt="img" class="w-[850px] h-screen hidden md:block object-cover" />
      <!-- text on image  -->
      <!-- <div
                class="absolute hidden bottom-10 right-6 p-6 bg-white bg-opacity-30 backdrop-blur-sm rounded drop-shadow-lg md:block">
                <span class="text-black text-xl">We've been using Untitle to kick"<br />start every new project
                    and can't <br />imagine working without it."
                </span>
            </div> -->
    </div>

    <!-- {/* right side */} -->
    <form class="flex flex-col justify-center mt-14 sm:mt-0 p-8 w-full lg:w-[450px] mx-auto" @submit.prevent="getAuth">
      <span class="mb-3 text-4xl font-bold">Welcome back</span>
      <span class="font-light text-gray-400 mb-4">
        Welcom back! Please enter your details
      </span>
      <div
        class="text-center font-semibold p-2 mb-4 text-sm text-red-800 border border-red-300 rounded-lg bg-red-50 dark:bg-gray-800 dark:text-red-400 dark:border-red-800"
        v-show="error">
        {{ errorMessage }}
      </div>
      <div class="py-4">
        <label class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Username</label>
        <input type="text" v-model="username"
          class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
      </div>
      <div class="py-4">
        <label class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Password</label>
        <input type="password" v-model="password"
          class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
      </div>

      <div class="flex justify-between items-center w-full py-4 mb-4">
        <div>
          <input type="checkbox" name="ch" id="ch" class="mr-2" />
          <span class="text-sm md:text-md">Remember me</span>
        </div>
        <a class="font-bold text-sm md:text-md cursor-pointer">Forgot password</a>
      </div>
      <UButton type="submit" label="Sign In" size="lg" :ui="{
        inline: '',
      }"/>
    </form>
  </div>
</template>
