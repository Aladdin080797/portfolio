<template>
  <div>
    <div class="photo">
        <a>
            <img :src="user.photo.url" alt="photo"/>
        </a>
    </div>
  </div>
</template>

<script>
export default {
    name: 'Photo',
    props: ['user'],
}
</script>

<style scoped lang="scss">
    .photo {
        a:focus {
            outline: none;
        }
        img {
            height: 260px;
            border-radius: 50%;
            display: inline-block;
        }
    }
    img {
        max-width: 100%;
    }
</style>