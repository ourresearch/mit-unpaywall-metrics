<template>
    <div class="page journals">

        <v-layout row>
            <h1>Affected subscriptions</h1>
            <v-spacer></v-spacer>
            <v-menu offset-y>
                <template v-slot:activator="{ on }">
                    <v-btn
                            small
                            flat
                            v-on="on"
                    >
                        <div>Export <i class="fas fa-file-export"></i></div>

                    </v-btn>
                </template>
                <v-list>
                    <v-list-tile @click="getCsv">
                        <v-list-tile-title>Download as CSV</v-list-tile-title>
                    </v-list-tile>
                    <v-list-tile @click="getJson">
                        <v-list-tile-title>View in API (JSON)</v-list-tile-title>
                    </v-list-tile>
                </v-list>
            </v-menu>


        </v-layout>


        <v-card>

            <v-card-title>

                <v-flex sm6>
                    <v-text-field
                            v-model="search"
                            append-icon="search"
                            label="Search"
                            single-line
                            hide-details
                    ></v-text-field>

                </v-flex>


            </v-card-title>
            <div class="state-info">
                <div class="loading state" v-show="!rawResults.length">
                      <v-progress-linear :indeterminate="true" color="green"></v-progress-linear>

                </div>
                <div class="no-results state" v-show="rawResults.length && !results.length">
                    No matches found for "{{search}}"
                </div>

            </div>


            <v-data-table
                    :headers="headers"
                    :items="results"
                    class="elevation-1 main-table"
                    hide-actions
                     v-show="results.length"

            >
                <template
                        v-slot:items="props">
                    <td @click="showArticles(props.item.journal_issn_l)">
                            <span class="journal-title" style="font-size: 16px;">
                                {{ props.item.journal_name }}
                            </span>
                        <span class="issns">
                                {{props.item.issns.join(", ")}}
                            </span>

                    </td>
                    <td>{{ props.item.displayClosedAccessDownloads }}</td>
                    <td>{{ props.item.displayDownloads }}</td>
<!--                    <td class="text-xs-right">{{ parseInt(props.item.num_dois) }}</td>-->
                    <td class="text-xs-right">{{ props.item.num_citations }}</td>
                    <td class="text-xs-right">{{ parseInt(props.item.proportion_oa * 100) }}%</td>
                    <td class="text-xs-right">{{ parseInt(props.item.proportion_publisher_hosted * 100)}}%</td>
                    <td class="text-xs-right">{{ parseInt(props.item.proportion_repository_hosted * 100)}}%</td>
                </template>
            </v-data-table>


            <!--                <v-list two-line>-->
            <!--                    <template v-for="(result, index) in results">-->
            <!--                        <v-list-tile :key="result.title">-->
            <!--                            <v-list-tile-content>-->
            <!--                                <v-list-tile-title v-html="result.journal_name"></v-list-tile-title>-->
            <!--                            </v-list-tile-content>-->
            <!--                        </v-list-tile>-->
            <!--                    </template>-->
            <!--                </v-list>-->

        </v-card>


    </div>
</template>

<script>
    import axios from 'axios'


    export default {
        name: 'Journals',
        data: () => ({
            rawResults: [],
            search: '',
            headers: [
                {text: "Journal name", value: "journal_name"},
                {text: "Subscribed views", value: "displayClosedAccessDownloads"},
                {text: "2018 Usage", value: "displayDownloads"},
                {text: "Citations by MIT authors", value: "num_citations"},
                // {text: "Affected DOIs", value: "num_dois"},
                {text: "%OA", value: "proportion_oa"},
                {text: "Publisher OA (%)", value: "proportion_publisher_hosted"},
                {text: "Repository OA (%)", value: "proportion_repository_hosted"}
            ],
        }),
        computed: {
            results() {
                return this.rawResults.filter(x => {
                    let mySearch = this.search.toLowerCase()

                    if (!mySearch) {
                        return true
                    }

                    if (x.issns.join().indexOf(mySearch) > -1) {
                        return true
                    } else if (x.journal_name.toLowerCase().indexOf(mySearch) > -1) {
                        return true
                    } else {
                        return false
                    }
                })
            }
        },
        methods: {
            getCsv() {
                window.location.href = "https://rickscafe-api.herokuapp.com/subscriptions.csv?package=mit_elsevier"
            },
            getJson() {
                window.location.href = "https://rickscafe-api.herokuapp.com/subscriptions?package=mit_elsevier"
            },
            showArticles(issn){
                this.$router.push({
                    path: "articles",
                    query: {
                        q: issn
                    }
                })
            },
            myFilter(searchTerm) {
                console.log("calling myFilter", searchTerm)
                return false
            },
            fetch() {
                 let url = "https://rickscafe-api.herokuapp.com/subscriptions?package=mit_elsevier"
                return axios.get(url)
                    .then(resp => {
                        const makeHiMedLowStrForSorting = function(apiStr){
                            const config = {
                                high: "high",
                                medium: "med",
                                low: "low",
                            }
                            if (!config[apiStr]){
                                return "<no data>"
                            }
                            else {
                                return config[apiStr]
                            }

                        }

                        let displayResp = resp.data.list.map(journal=>{
                            journal.displayDownloads = makeHiMedLowStrForSorting(journal.downloads)
                            journal.displayClosedAccessDownloads = makeHiMedLowStrForSorting(journal.closed_access_downloads)
                            return journal
                        })

                        this.rawResults = displayResp
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
    .page.journals {
        width: 100%;
        max-width: 1200px;

    }


    div.state-info {
        padding: 20px 20px;
    }

    table.v-table tbody {
        td {
            font-size: 16px;
            cursor: pointer;

            .issns {
                display: block;
                font-size: 11px;
                opacity: .6;
            }

            &:first-child {
                padding-top: 10px;
                padding-bottom: 10px;

            }
        }


    }


</style>
