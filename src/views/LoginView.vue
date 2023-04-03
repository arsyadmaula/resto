<script setup>
import { reactive,ref } from "vue";
import {useRoute,useRouter} from "vue-router";
import { useAuthRepository } from "../../composables";

const repository = useAuthRepository();
const route = useRoute();
const router = useRouter();

const credentials =reactive({
  email: "",
  password: "",
  device_name: "browser",
});

const isLoginIn = ref(false)

const onSubmit= async () => {
  isLoginIn.value = true;
try {
  const {data} = await repository.login(credentials);
  if (data) {
    localStorage.setItem('access_token',data.access_token);
    localStorage.setItem('user',data.user);
  
    router.replace({name :"about"});

}
} catch(e){
  console.error(e);
}

  isLoginIn.value = false;

};

</script>

<template>
  <main class="grid grid-cols-12 gap-4 min-h-screen">
  <section class="col-span-6 bg-white h-full">
    <form method="post" :action="route.path" class="p-40" @submit.prevent="onSubmit">
    <div class="mb-4">
        <label for="email" class="block mb-2">email</label>
        <input v-model="credentials.email" 
        type="email"
        required
        placeholder="someone@gmail.com"
        class="border p-2 w-full bg-gray-100 outline-none focus:ring-4 focus:ring-blue-600 rounded">
    </div>
    <div class="mb-4">
        <label for="Password" class="block mb-2">Password</label>
        <input v-model="credentials.password" 
        type="Password"
        required
        class="border p-2 w-full bg-gray-100 outline-none focus:ring-4 focus:ring-blue-600 rounded">
    </div>
    <button type="submit" class="bg-blue-700 text-white p-2 w-full rounded hover:bg-blue-900 transition-colors duration-200">Masuk</button>
    </form>
  </section>  
  </main>
</template>
