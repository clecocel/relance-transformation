<template>
    <div class="aides">

        <Header></Header>

        <div class="rf-container">

            <Breadcrumbs :dynamic_page_title="title"></Breadcrumbs>

            <div class="rf-grid-row rf-grid-row--center">
                <div class="rf-col-md-10 rf-col-lg-8" v-if="aide">

                        <div class="aides-intro">
                            <a href="./" class="rf-link rf-link--icon-left rf-margin-bottom-3N">Retour aux appels à projet</a>
                            <div class="aides-title">
                                <img src="@/assets/FranceRelance.png" alt="France Relance" />
                                <h1>{{ aide.short_title }}</h1>
                            </div>
                            <p class="rf-text--lg">{{ aide.name }}</p>
                        </div>

                        <div class="rf-margin-top-8N">
                            <h2>Description</h2>
                            <div v-html="aide.description" class="rf-text"></div>
                        </div>

                        <div class="rf-margin-top-8N">
                            <h2>Conditions</h2>
                            <div v-html="aide.eligibility" class="rf-text"></div>
                        </div>

                        <div class="rf-margin-top-8N">
                            <h2>Exemples d’applications</h2>
                            <div>
                                <div v-html="aide.project_examples" class="rf-text">À titre d’exemple, les thématiques suivantes pourront être explorées :</div>
                            </div>
                        </div>

                        <div class="rf-margin-top-8N">
                            <h2>Candidater</h2>
                            <div v-html="aide.contact"></div>
                            <div><a :href="aide.application_url" title="Candidater à cet appel à projet - Nouvelle fenêtre" target="_blank" class="rf-btn candidate-button rf-text">Candidater à cet appel à projet</a></div>
                        </div>

                </div>
            </div>
        </div>

        <Footer></Footer>
    </div>
</template>

<script>
    import aidService from '../services/aidService'
    import Breadcrumbs from "@/components/Breadcrumbs";
    import Footer from "@/components/Footer";
    import Header from "@/components/Header";

    export default {
        name: "Aides",

        components: { Header, Breadcrumbs, Footer },

        data() {
            return {
                aide: null,
                title: "",
                meta_title: "",
                description: "",
                previewImg: require('@/assets/Preview.png'),
            }
        },

        created() {
            aidService.fetchAidDetail(this.$route.params.slug)
                .then(aidDetail => {
                    this.aide = aidDetail
                    this.title = aidDetail.short_title;
                    this.meta_title = this.title + " - Ministère de la Transformation et de la Fonction publiques"
                    this.description = aidDetail.name;
            })
        },

        metaInfo() {
          return {
            title: this.meta_title,
            meta: [
              {
                name: "description",
                content: this.description
              },
              {
                property: 'og:title',
                content: this.meta_title
              },
              {
                property: 'og:description',
                content: this.description
              },
                {
                property: 'og:image',
                content: "https://france-relance.transformation.gouv.fr" + this.previewImg
              },
              {
                name: "twitter:card",
                content: "summary_large_image"
              },
              {
                name: "twitter:site",
                content: "@AdeMontchalin"
              },
              {
                name: "twitter:title",
                content: this.meta_title
              },
              {
                name: "twitter:description",
                content: this.description
              },
              {
                name: "twitter:image",
                content: "https://france-relance.transformation.gouv.fr" + this.previewImg
              },
            ],
          }
        },
    }
</script>

<style>


    .aides-intro a {
        color: #107449;
    }

    .aide a {
      box-shadow: none;
    }

    .aides-title {
        display: flex;
        align-items: flex-start;
        justify-content: flex-start;
    }

    .aides-title h1 {
        color: #107449;
        margin-top: 0;
        margin-bottom: 8px;
    }

    .aides-title img {
        height: 120px;
        margin-right: 32px;
    }

    @media screen and (max-width: 767px) {

        .aides-title {
            display: inline-block;
        }

        .aides-title h1 {
            margin-top: 8px;
            font-size: 2.5rem;
        }

        .aides-title img {
            height: 48px;
            margin-right: 0px;
        }
    }

    .aides h2 {
        margin-bottom: 16px;
    }

    .candidate-button {
        background-color: #107449 !important;
        margin-top: 16px;
    }

</style>
