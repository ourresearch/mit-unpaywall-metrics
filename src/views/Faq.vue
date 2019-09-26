<template>
    <v-container>
        <v-layout row>
            <v-flex>
                <v-card min-height="400">
                    <v-card-title>
                        <div>
                            <h1 class="headline mb-4">Frequently Asked Questions</h1>

                            <h2 class="title mt-4 my-1">Why was this website created?</h2>
                            <p>
                                MIT wants to capture and openly surface data about the journals and articles that could be affected if MIT alters its contract with Elsevier, as well demonstrating open access options where available. The tool was built by
                                <a href="https://our-research.org">Our Research</a> with support from MIT.
                            </p>


                            <h2 class="title mt-4 my-1">What is the source for the data on affected journals and
                                articles?</h2>
                            <p>
                                This site is built on data from <a href="https://unpaywall.org">Unpaywall</a> combined
                                with affected journals usage from MIT. Counts of citations from MIT authors are compiled from Microsoft Academic Graph (MAG).  MAG citation coverage is lower than that of Google Scholar, but more than Scopus or Web of Science. (<a
                                    href="https://harzing.com/blog/2017/06/microsoft-academic-is-one-year-old-the-phoenix-is-ready-to-leave-the-nest">Anne-Wil Harzing's</a> work is a good place to start if you'd like to learn more about the relative coverage of different citation indices).
                            </p>

                            <h2 class="title mt-4 my-1">What are the options for downloading these data sets?</h2>
                            <p>
                                Downloads of journal and article data sets are available in both CSV and JSON formats.
                                CSV is offered for the sake of convenience and as a common standard format. JSON has the
                                advantage of being easier to work with and parse at this scale.
                            </p>


                            <h2 class="title mt-4 my-1">How do I get access to content without an open access copy?</h2>
                            <p>
                                See the <a
                                    href="https://open-access.mit.edu/sites/default/files/20180917_Provost_OpenAccessTF_WhitePaper.pdf">MIT
                                Open Access white paper [pdf]</a> for more information on alternative ways to access content.
                            </p>

                            <h2 class="title mt-4 my-1">How does this relate to the Unpaywall extension?</h2>
                            <p>
                                This project is from <a href="https://ourresearch.org">Our Research</a>, the nonprofit organization that made and supports the Unpaywall extension, and it uses the same
                                <a href="https://unpaywall.org/">underlying database</a>, which contains over twenty million open access resources. The Unpaywall extension is a free, open-source add-on for
                                <a href="https://chrome.google.com/webstore/detail/unpaywall/iplffkdpngmdjhlpjmppncnlhomiipha?hl=en">Chrome</a> and
                                <a href="https://addons.mozilla.org/en-CA/firefox/addon/unpaywall/">Firefox</a>, which automatically checks the Unpaywall database for an open access version when you encounter a paywalled article.
                            </p>

                            <h2 class="title mt-4 my-1">How up-to-date is the data?</h2>
                            <p>
                                The download numbers are from 2018, the most recent full year available. Citation and OA  numbers are updated nightly, but are for 2018 articles only, in order to keep the statistics comparable. This means that over time, the OA% and citation numbers will go up. There's a new version of this tool coming in November 2019 that will take a more sophisticated, cohort-style approach which will support more robust analysis and projections.
                            </p>

                            <h2 class="title mt-4 my-1">What do the "high, medium, low" categories mean?</h2>
                            <p>
                                MIT's Elsevier contract does permit publicizing exact download numbers. The "high, medium and low" numbers are simply the terciles (binning journals into equal thirds).
                            </p>

                            <h2 class="title mt-4 my-1">What does "Toll-access usage" mean?</h2>
                            <p>
                                This is the number of accesses that could not have been fulfilled by an Open Access copy, and therefore is a more accurate indicator of the value of a subscription.  A later version of this tool will also consider the impact of back-catalog content (which is not OA, but is permanently available since the library has already puchased it) on future subscription value, which is considerable.
                            </p>


                        </div>


                    </v-card-title>
                </v-card>
            </v-flex>
        </v-layout>
    </v-container>
</template>

<script>
    export default {
        name: "Faq"
    }
</script>

<style scoped>

</style>