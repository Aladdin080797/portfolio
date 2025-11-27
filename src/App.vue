<template>
  <transition name="fade" tag="div" class="wrapper" mode="out-in">
    <div class="wrapper" v-if="isLoaded" id="app">
      <LandingPage :user="user" />
      <Experience :content="findSlug('experiences')" />
      <Skills :content="findSlug('skills')"/>
      <Projects :content="findSlug('projects')"/>
      <Description :user="user" :content="findSlug('description')"/>
      <Footer :user="user" :sticky="showStickyFooter"/>
    </div>
  </transition>
</template>

<script>
import LandingPage from "./components/LandingPage.vue";
import Description from "./components/Description.vue";
import Experience from "./components/Experience.vue";
import Skills from "./components/Skills.vue";
import Projects from "./components/Projects.vue";
import Footer from "./components/Footer.vue";

import { bucket } from "./cosmic.js";

export default {
  name: "App",
  components: {
    LandingPage,
    Description,
    Experience,
    Skills,
    Projects,
    Footer,
  },
  data: () => ({
    isLoaded: false,
    user: {},
    description: [],
    experiences:[],
    skills: [],
    projects: [],
    showStickyFooter: false,
  }),
  methods: {
    fetchType(type) {
      return bucket.objects.find({
       type: type,
      }).props("slug,title,metadata").limit(1);
    },
    fetchUser() {
      return bucket.objects.findOne({
        type: "portfolio-metadata",
        slug: "portfolio-info"
      }).props("title,metadata")
      .depth(1);
    },
    findSlug(slug) {
      return this.$data[slug]?.find((item) => {
        return item.slug === slug;
      });
    },
    extractFirstObject(objects) {
      return objects.object === null ? void 0 : objects.object;
    },
    checkLandingPageVisibility() {
      this.showStickyFooter = window.scrollY > 50
    },
  },
  async created() {
    document.body.classList.add("loading");

    try {
      const [descriptionRes, userRes, experiencesRes, skillsRes, projectsRes] =
      await Promise.all([this.fetchType("description"), this.fetchUser(),
        this.fetchType("experiences"), this.fetchType("skills"), 
        this.fetchType("projects")]);

      this.description = descriptionRes && Array.isArray(descriptionRes.objects) ? 
        descriptionRes.objects : [];
      this.experiences = experiencesRes && Array.isArray(experiencesRes.objects) ? 
        experiencesRes.objects : [];
      this.skills = skillsRes && Array.isArray(skillsRes.objects) ? 
        skillsRes.objects : [];
      this.projects = projectsRes && Array.isArray(projectsRes.objects) ? 
        projectsRes.objects : [];

      const userObject = this.extractFirstObject(userRes);

      this.user = {
      name: (userObject && userObject.metadata && userObject.metadata.name) || "",
      status: (userObject && userObject.metadata && userObject.metadata.status) || "",
      email: (userObject && userObject.metadata && userObject.metadata.email) || "",
      phone: (userObject && userObject.metadata && userObject.metadata.phone) || "",
      city: (userObject && userObject.metadata && userObject.metadata.city) || "",
      lang: (userObject && userObject.metadata && userObject.metadata.lang) || "",
      photo: (userObject && userObject.metadata && userObject.metadata.photo) || "",
      linkedIn: (userObject && userObject.metadata && userObject.metadata.linkedin_url) || "",
      resume:  (userObject && userObject.metadata && userObject.metadata.resume) || ""
      };

      console.log(this.user)

      this.isLoaded = true;
      this.$nextTick(() => {
        this.checkLandingPageVisibility();
        window.addEventListener("scroll", this.checkLandingPageVisibility);
      });
    } catch (err) {
      console.error("Failed to load portfolio data:", err);
      this.description = this.description || [];
      this.experiences = this.experiences || [];
      this.skills = this.skills || [];
      this.projects = this.projects || [];
    } finally {
      document.body.classList.remove("loading");
    }
  },
  beforeUnmount() {
    window.removeEventListener("scroll", this.checkLandingPageVisibility);
  },
};
</script>

<style scoped lang="scss">
@import "@/styles/constants.scss";

#app {
  font-family: Montserrat-Regular, serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

.wrapper {
  height: 100%;
}
</style>
