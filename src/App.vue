<template>
    <div id="app" class="container">
        <h1 class="text-center">{{ msg }}</h1>
        <h5 class="text-center small text-gray">{{msgSmall}}</h5>
        <div class="row" v-if="countries.length>0">
            <div class="col-xs-12 col-md-4 col-sm-4"
                 v-for="country in countries | limitBy entriesPerPage getOffset()">
                <h4>{{country.Name}}, {{country['Country Code']}}</h4>
                <p class="small">{{country['Canonical Name']}}</p>
                <hr>
            </div>
        </div>
        <nav>
            <ul class="pager">
                <li v-if="page>1">
                    <a v-on:click="switchPage('prev')" href="#">Previous</a>
                </li>
                <li v-if="page*entriesPerPage < countries.length" >
                    <a v-on:click="switchPage('next')" href="#">Next</a>
                </li>
            </ul>
        </nav>
    </div>
</template>

<script>

    export default {
        ready() {
            this.getCountries();
        },
        methods: {
            switchPage: function (action) {
                this.$event.preventDefault();
                this.$event.stopPropagation();
                this.page = action == 'prev' ? this.page - 1 : this.page + 1;
                history.pushState({page: this.page}, 'Page', '#/page/'+ this.page);
            },
            getCountries() {
                this.$http.get('/dist/countries.json').then(function (res) {
                    this.countries = res.data;
                })
            },
            getOffset() {
                return (this.page - 1) * this.entriesPerPage
            }
        },
        data () {
            return {
                msg: 'Cities in the World',
                msgSmall: 'Below is a list of all the top cities in the world. Feel free to browse.',
                page: 1,
                entriesPerPage: 15,
                countries: []
            }
        }
    }
</script>

<style >
    @import '../node_modules/bootstrap/dist/css/bootstrap.css';
    h1{
        background-color: yellow;
        border: 1px solid black;
        padding-top: 20px;
        padding-bottom: 20px;
    }
    .text-gray{
        color: gray;
    }
    .pager a{
        background-color: magenta!important;
        color: white;
        border: 1px solid black!important;
        border-radius: 10px!important;
    }
</style>