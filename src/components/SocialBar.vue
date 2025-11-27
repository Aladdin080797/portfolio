 <template>
  <div class="social-wrap">
    <ul>
      <li>
        <a :href="user.linkedIn" target="_blank">
          <font-awesome-icon class="icon" :icon="['fab', 'linkedin-in']" 
                             title="linkedin profile"/>
        </a>
      </li>
      <li>
        <a v-if="user.resume" :href="user.resume.url" target="_blank">
          <font-awesome-icon class="icon" :icon="['fas', 'download']" 
                             title="Download Resume"/>
        </a>
      </li>
    </ul>
  </div>
</template>

<script>
    export default {
        name: 'SocialBar',
        props: ['user']
    }
</script>

<style scoped lang="scss">
  @import '@/styles/constants.scss';
 .social-wrap {
    li {
      display: inline-block;
      margin-right: 10px;
    }
    .icon {
      font-size: 3rem; 
      width: 3rem; 
      height: 3rem;
      line-height: 3rem;
      text-align: center;     
      vertical-align: middle;
    }
  }
</style>