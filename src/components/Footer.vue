<template>
  <footer :class="['footer', { sticky: sticky }]">
    <div>
        <font-awesome-icon class="copyright-icon" icon="copyright"/> 
        {{currentYear}} {{user.name}} 
    </div>
    <SocialBar :user="user"/>
  </footer>
</template>

<script>
import SocialBar from './SocialBar'
export default {
  name: 'Footer',
  props: {
    user: Object,
    sticky: Boolean
  },
  components: {
    SocialBar,
  },
  computed: {
    currentYear() {
      return new Date().getFullYear()
    }
  }
}
</script>

<style scoped lang="scss">
@import '@/styles/constants.scss';

$bg-footer: map-get($colors, primary) !default;

.footer {
  padding: 10px 20px;
  background-color: $bg-footer;
  color: map-get($colors, light);
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}

/* Sticky only when needed */
.footer.sticky {
  position: fixed;
  bottom: 0;
  left: 0;
  z-index: 50;
}

.copyright-icon {
  font-size: 1.5rem;
  vertical-align: middle;
}

::v-deep .social-wrap {
  float: right;

  .icon {
    font-size: 2.8rem;

    &:hover {
      color: map-get($colors, light);
    }
  }
}

::v-deep li {
  margin-bottom: 0 !important;
}
</style>