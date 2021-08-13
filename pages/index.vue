<template>
  <div>
    <div class="uk-section">
      <div class="uk-container uk-container-large">
        <h1>{{ homepage.hero.title }}</h1>
        <br>
        <h4>The Science Nation blog is a space for passionate researchers to share their knowledge, as well as a place for new scientists to submit their first publications. Because publications are an incredibly effective means of boosting applicants' likelihood of receiving internship positions and job offers, Science Nation is pleased to present a space for people relatively new to their fields to publish their first research papers and scholarly articles.</h4>
        <a href="https://forms.gle/n9dFSC6BoL3acoRk6" target="_blank"><button>Publish Your Paper ></button></a>
        <br>
        <Articles :articles="articles" />
      </div>
    </div>
  </div>
</template>

<script>
import Articles from "../components/Articles";
import { getMetaTags } from "../utils/seo";
import { getStrapiMedia } from "../utils/medias";

export default {
  components: {
    Articles,
  },
  async asyncData({ $strapi }) {
    return {
      articles: await $strapi.find("articles"),
      homepage: await $strapi.find("homepage"),
      global: await $strapi.find("global"),
    };
  },
  head() {
    const { seo } = this.homepage;
    const { defaultSeo, favicon, siteName } = this.global;

    // Merge default and article-specific SEO data
    const fullSeo = {
      ...defaultSeo,
      ...seo,
    };

    return {
      titleTemplate: `%s | ${siteName}`,
      title: fullSeo.metaTitle,
      meta: getMetaTags(fullSeo),
      link: [
        {
          rel: "favicon",
          href: getStrapiMedia(favicon.url),
        },
      ],
    };
  },
};
</script>
