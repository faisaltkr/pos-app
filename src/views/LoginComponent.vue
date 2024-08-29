<template>
  <div class="w-full h-screen overflow-hidden flex min-h-screen flex-col items-center justify-center bg-gray-50">
    <div class="grid grid-cols-1 md:grid-cols-2 gap-6 w-full max-w-full ">
      <div class="md:shrink-0">
        <img class="h-full w-full object-cover md:h-full md:w-full" src="../../public/images/whatis.jpg" alt="Modern building architecture">
      </div>
      <div>
        <div class="flex min-h-full flex-1 flex-col justify-center">
          <div class="sm:mx-auto sm:w-full sm:max-w-sm">
            <img class="mx-auto h-10 w-auto" src="https://tailwindui.com/img/logos/mark.svg?color=indigo&shade=600" alt="Your Company" />
            <h2 class="mt-10 text-center text-2xl font-bold leading-9 tracking-tight text-gray-900">Sign in to your account</h2>
          </div>
          <div class="sm:mx-auto sm:w-full sm:max-w-sm">
            <form @submit.prevent="login" class="space-y-6" method="POST">
              <div>
                <label for="email" class="block text-sm font-medium leading-6 text-gray-900">Email address</label>
                <div class="mt-2">
                  <input id="email" v-model="username" type="email" autocomplete="email" required class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
                </div>
              </div>
              <div>
                <div class="flex items-center justify-between">
                  <label for="password" class="block text-sm font-medium leading-6 text-gray-900">Password</label>
                  <div class="text-sm">
                    <a href="#" class="font-semibold text-indigo-600 hover:text-indigo-500">Forgot password?</a>
                  </div>
                </div>
                <div class="mt-2">
                  <input id="password" v-model="password" type="password" autocomplete="current-password" required class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
                </div>
              </div>
              <div>
                <button type="submit" class="flex w-full justify-center rounded-md bg-indigo-600 px-3 py-1.5 text-sm font-semibold leading-6 text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">
                  <span v-if="!loading">Sign in</span>
                  <span v-else>
                    <svg class="animate-spin h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                      <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                      <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.961 7.961 0 014 12H0c0 3.042 1.135 5.824 3 7.937l3-2.646z"></path>
                    </svg>
                  </span>
                </button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import router from "../router";

export default {
  data() {
    return {
      username: '',
      password: '',
      loading: false
    };
  },
  methods: {
    login() {
      this.loading = true;
      const data = {
        login: this.username,
        password: this.password
      };
      console.log(data);


      const clientId = '511cb2ac2d'; // Replace with your actual client ID
      const state = '444'; // Random string to maintain state between request and callback
      const codeVerifier = '21XaP8MJjpxCMRxgEzBP82sZ73PRLqkyBUta1R309J0'; // PKCE code verifier
      const responseType = 'code';
      const scope = 'openid all';
      const redirectUri = 'http://localhost:8080/api/login_response'; // Replace with your actual redirect URI

      const authorizeUrl = `https://{your_frappe_instance}/api/method/frappe.integrations.oauth2.authorize`;

      // Construct the full authorization URL
      const url = new URL(authorizeUrl);
      url.searchParams.append('client_id', clientId);
      url.searchParams.append('state', state);
      url.searchParams.append('code_verifier', codeVerifier);
      url.searchParams.append('response_type', responseType);
      url.searchParams.append('scope', scope);
      url.searchParams.append('redirect_uri', redirectUri);

      // Redirect the user to the ERPNext authorization URL
      window.location.href = url.toString();

      axios.get("https://recruitment-api.pyt1.stg.jmr.pl/login", data)
        .then((response) => {
          console.log(response.status);
          localStorage.setItem('token', response.data.token);
          router.push("/dashboard");
        })
        .catch((error) => {
          console.log("Cannot log in", error);
        })
        .finally(() => {
          this.loading = false;
        });
    }
  }
};
</script>

<style>
html, body {
  height: 100%;
  margin: 0;
  width: 100%;
}
body {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #f0f0f0;
}
form input {
  padding-left: 8px;
}
</style>
