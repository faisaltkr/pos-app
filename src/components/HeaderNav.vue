<template>
  <header class="header-nav">
      <div class="grid grid-cols-2 gap-4">
          <div class="col-start-1 col-end-3">
            <nav>
              <ul>
                <li>
                  <b-icon icon="camera" font-scale="7.5"></b-icon>
                  <router-link to="/dashboard">Dashboard</router-link>
                </li>
                <li>
                  <b-icon icon="camera" font-scale="7.5"></b-icon>
                  <router-link to="/sales">Sales</router-link>
                  
                </li>
                <li><router-link to="/customer">Customer</router-link></li>
                <li><a href="#">Receipt</a></li>
                <li><a href="#">Report</a></li>
              </ul>
            </nav>
          </div>
          <div class="col-end-7 col-span-2">
            <button @click="switchLanguage('en')">English</button>
            <button @click="switchLanguage('ar')">العربية</button>
          </div>
      </div>
      
      
    </header>
    
</template>

<script>
export default {
  name: 'HeaderNav',
  methods: {
    switchLanguage(lang) {
      this.$i18n.locale = lang;
      document.documentElement.setAttribute('dir', lang === 'ar' ? 'rtl' : 'ltr');
    }
  }
};
</script>

<style scoped>

.header-nav {
  background-color: #74a3d5;
  padding: 10px 20px;
  box-shadow: -1px -1px 7px 0px;
}

.header-nav nav ul {
  display: flex;
  list-style: none;
  padding: 0;
  margin: 0;
}

.header-nav nav ul li {
  margin-right: 20px;
}

.header-nav nav ul li a {
  color: white;
  text-decoration: none;
}
button {
  margin: 5px;
}
</style>