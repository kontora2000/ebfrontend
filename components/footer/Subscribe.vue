<template>
  <div class="footer-email-subscribe">
    <div class="footer-email-subscribe-header">
      <h3>
        <i>Еженедельная</i>
      </h3>
      <h5>Рассылка новостей</h5>
    </div>
    <template v-if="!isSubscribeSuccess">
      <form class="footer-email-subscribe-form" @submit.prevent="subsrcibe">
        <div class="input-wrapper">
          <input v-model="email" type="email" class="subscribe-form-input" required>
          <label class="floating-label" data-placeholder="Ваш имейл" />
        </div>
        <input type="submit" class="subscribe-form-submit" value="Подписаться">
      </form>
      <div class="footer-email-subscribe-note">
        Нажимая кнопку&nbsp;«Подписаться», вы&nbsp;соглашаетесь с&nbsp;
        <a
          class="link-underline"
          href="/page/privacy-policy"
        >условиями обработки персональных данных</a>
      </div>
    </template>
    <template v-if="isSubscribeSuccess">
      <div>Ваш email-адрес добавлен в рассылку</div>
      <button @click="unSubscribe()">
        Отписаться
      </button>
    </template>
    <template v-if="isAlreadySubscribed">
      <div>Данный email-адрес уже находится в рассылке </div>
      <a @click="toSubsrcibeForm()">Назад</a>
    </template>
  </div>
</template>
<script>

import urls from '@/assets/js/url'

export default {
  name: 'Subscribe',
  data () {
    return {
      email: '',
      isError: false,
      isSubscribeSuccess: false,
      isUnSubscribeSuccess: false,
      apiKey: '9af4d8381781baccb0f915e554f8798d',
      isAlreadySubscribed: false
    }
  },
  methods: {
    async subsrcibe () {
      const data = {
        api_key: this.apiKey,
        email: this.email,
        lists: 1
      }
      const res = await this.$axios.post(`${urls.apiBaseURL}/newsletter/v1/subscribe`, urls.restHeaders.data, data)
      if (res.data.code === -1) {
        this.isAlreadySubscribed = true
      }
      if (!res.data && res.status === 200) {
        this.isSubscribeSuccess = true
      }
    },
    async unSubscribe () {
      const data = {
        api_key: this.apiKey,
        email: this.email,
        lists: 1
      }
      const res = await this.$axios.post(`${urls.apiBaseURL}/newsletter/v1/unsubscribe`, urls.restHeaders.data, data)
      if (res.data.length === 0 && res.status === 200) {
        this.email = ''
        this.isSubscribeSuccess = false
      }
    }
  }
}
</script>
