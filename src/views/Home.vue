<template>
    <div class="home">


        <v-container grid-list-lg>
            <v-layout row wrap>
                <v-flex md12>
                    <v-card>
                        <v-card-title primary-title>
                            <div>
                                <h3 class="headline mb-0">About this website</h3>
                                <div> MIT is currently in subscription negotiations with Elsevier. This website provides information about the Elsevier journals under discussion, including percentages of Elsevier content available as open access. This depicts only future issues, usage of perpetual-access backfile content is not included. The data sets provided through this site are both searchable and downloadable.

                                </div>
                            </div>
                        </v-card-title>

                        <v-card-actions class="pa-4">
                            <v-btn dark to="./subscriptions">View Subscriptions</v-btn>
                            <v-btn dark to="./articles">View Articles</v-btn>
                            <v-btn flat to="./faq">Read FAQ</v-btn>

                        </v-card-actions>
                    </v-card>

                </v-flex>





            </v-layout>


            <v-layout row>
                <v-flex shrink>
                    <v-card v-if="numArticles">
                        <v-card-title>
                            <h3 class="headline mb-0">Affected by cancellation:</h3>
                        </v-card-title>
                        <v-card-text>
                            <h1>
                                {{numArticles.toLocaleString()}} articles
                            </h1>
                            <h1>
                                {{numJournals.toLocaleString()}} journals
                            </h1>
                        </v-card-text>
                        <v-card-actions>
                            <p class="ma-2">Data updated nightly</p>

                        </v-card-actions>

                    </v-card>


                </v-flex>

                <v-flex grow>
                    <v-card v-if="numArticles">
                        <v-card-title>
                            <h3 class="headline mb-0">Articles breakdown:</h3>
                        </v-card-title>
                        <v-card-text>
                            <div class="bar-container">
                                <div class="bar"
                                     :class="k"
                                     :style="{width: bar.percent+'%'}"
                                     v-for="(bar, k) in bars"></div>
                            </div>

                            <div class="stats mt-4">
                                <div class="stat pa-1" :class="k" v-for="(bar, k) in bars">
                                    <span class="num headline pa-1">{{Math.round(bar.percent)}}%</span>
                                    <span class="label">{{bar.name}}</span>
                                </div>
                            </div>
                        </v-card-text>
                        <v-card-actions>
                            <p class="ma-2">Data updated nightly</p>

                        </v-card-actions>

                    </v-card>


                </v-flex>


            </v-layout>



        </v-container>



    </div>
</template>


<script>
    import axios from 'axios'

    export default {
        name: 'Home',
        data: () => ({
            articleBreakdown: {},
            numArticles: null,
            numJournals: null
        }),
        computed: {
            bars(){
                return {
                    repository: {
                        name: "Open Access in a repository",
                        percent: 100 * this.articleBreakdown.num_has_repository_hosted_and_not_publisher_hosted / this.numArticles
                    },
                    both: {
                        name: "Open Access at publisher and in repository",
                        percent: 100 * this.articleBreakdown.num_has_repository_hosted_and_has_publisher_hosted / this.numArticles
                    },
                    publisher: {
                        name: "Open Access at publisher",
                        percent: 100 * this.articleBreakdown.num_not_repository_hosted_and_has_publisher_hosted / this.numArticles
                    },
                    closed: {
                        name: "No Open Access found",
                        percent: 100 * this.articleBreakdown.num_closed / this.numArticles
                    },



                }
            },

            bars2(){
                return {
                    repository: 100 * this.articleBreakdown.num_has_repository_hosted_and_not_publisher_hosted / this.numArticles,
                    both: 100 * this.articleBreakdown.num_has_repository_hosted_and_has_publisher_hosted / this.numArticles,
                    publisher: 100 * this.articleBreakdown.num_not_repository_hosted_and_has_publisher_hosted / this.numArticles,
                    closed: 100 * this.articleBreakdown.num_closed / this.numArticles,
                }
            },
            results() {
                return this.rawResults.filter(x => {
                    if (!this.articleSearch) {
                        return true
                    } else if (x.issns.join().indexOf(this.articleSearch) > -1) {
                        return true
                    } else if (x.journal_name.indexOf(this.articleSearch) > -1) {
                        return true
                    } else {
                        return false
                    }
                })
            }
        },
        methods: {
            fetch() {
                let url = "https://rickscafe-api.herokuapp.com/breakdown?package=mit_elsevier"
                return axios.get(url)
                    .then(resp => {
                        this.articleBreakdown = resp.data.article_breakdown
                        this.numArticles = resp.data.num_articles_total
                        this.numJournals = resp.data.num_journals_total
                        return true
                    })
                    .catch(e => {
                        console.log("error from server", e)
                        return false
                    })
            }
        },
        mounted() {
            this.fetch()
        }
    }
</script>



<style scoped lang="scss">

    .bar-container {
        width: 100%;
        height: 100px;
        display: flex;
        .bar {
            height: 100%;
            &.repository {background: #4CAF50;}
            &.both {background: #AE9E24;}
            &.publisher {background: #FF8F00;}
            &.closed {background: gray;}
        }
    }
    .stats {
        .stat {
            &.repository {color: #4CAF50;}
            &.both {color: #AE9E24;}
            &.publisher {color: #FF8F00;}
            &.closed {color: gray;}
            font-size: 22px;
            .num {
                width: 3em;
                font-size: 34px !important;
                display: inline-block;
                text-align: right;
                margin-right: 10px;
                font-weight: bold;
                }
        }
    }



</style>
