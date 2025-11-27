<template>
    <section id="about">
        <AnimateOnVisible name="fadeDown" :duration="1">
            <Title :title="content.metadata.title" :description="content.metadata.description" />
        </AnimateOnVisible>

            <div class="section-content">
                <div class="container-fluid">
                    <div class="row">
                        <div class="col-md-7 mr-auto card-mobile">
                            <AnimateOnVisible name="fadeRight" :duration="1">
                                <Photo :user="user"/>
                            </AnimateOnVisible>
                           
                        </div>
                        <div class="col-md-4 mr-auto card-mobile">
                            <AnimateOnVisible name="fadeLeft" :duration="1">
                                <PersonnalCard :user="user"/>
                            </AnimateOnVisible>
                        </div>
                    </div>
                </div>
            </div>
    </section>
</template>

<script>
    import Title from './Title.vue'
    import PersonnalCard from './PersonnalCard.vue'
    //import Presentation from './Presentation.vue'
    import Photo from './Photo.vue'

    export default {
        name: 'AboutMe',
        props: ['user', 'content'],
        components: {
            Title,
            PersonnalCard,
     //       Presentation,
            Photo
        }
    }
</script>

<style scoped lang="scss">
    @import '@/styles/constants.scss';

    #about {
        background-color: lighten(map-get($colors, dark), 100%);
    }

    @media(min-width: #{map-get($breakpoints, medium)}) {
        .section-content {
            width: 80%;
            margin: 0 auto;
        }
    }

    @media(max-width: #{map-get($breakpoints, medium)}) {
        .card-mobile {
            text-align: center !important;
            margin-top: 20px;
        }
    }
</style>