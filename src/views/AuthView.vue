<template>
  <div class="auth">
    <h1>LeadHit</h1>
    <div v-if="isError"
         class="error">
      id сайта должен содержать 24 символа
    </div>
    <input v-model="id"
           @focus="isError=false"
           type="text">
    <button @click="checkIdLength">Войти</button>
  </div>
</template>

<script>
export default {
  name: 'AuthView',
  data() {
    return {
      id: '5f8475902b0be670555f1bb3',
      isError: false,
    }
  },
  methods: {
    async onAuth() {
      try {
        const response = await fetch('https://track-api.leadhit.io/client/test_auth', {
          method: "GET",
          withCredentials: true,
          headers: {
            "Api-Key": "5f8475902b0be670555f1bb3:eEZn8u05G3bzRpdL7RiHCvrYAYo",
            "Leadhit-Site-Id": this.id,
          }
        });
        let authKeyResponse = await response.json();
        if (authKeyResponse.message === 'ok') {
          localStorage.setItem('leadhit-site-id', this.id);
          await this.$router.push({path: `/`});
        }
      } catch (e) {
        console.dir(e);
      }
    },
    checkIdLength() {
      if (this.id.length !== 24) {
        this.isError = true;
      } else {
        this.isError = false;
        this.onAuth();
      }
    },
  },
}
</script>