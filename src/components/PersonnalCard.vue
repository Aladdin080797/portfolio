<template>
  <div>
    <h3>{{ user.name }}</h3>
    <div class="data"><strong>E-mail:</strong> {{ user.email }}</div>
    <div class="data"><strong>Tel.:</strong> {{ user.phone }}</div>
    <div class="data"><strong>City:</strong> {{ user.city }}</div>
    <div class="data"><strong>Languages:</strong> {{ user.lang }}</div>
    <SocialBar :user="user"/>
  </div>
</template>

<script>
import SocialBar from './SocialBar.vue'

export default {
    name: 'PersonnalCard',
    props: ['user'],
    components: {
        SocialBar,
    }
}
</script>

<style scoped lang="scss">
    @import '@/styles/constants.scss';

    .data {
        margin-bottom: 15px;
    }
</style>