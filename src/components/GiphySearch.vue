<template>

<div id="GiphySearch" class="card text-center w-75 m-auto">
  <div class="card-header">
    <label for="searchInput">Start your search below</label>
    <input type="text" name="search" class="form-control" id="searchInput" placeholder="Search GIF's" v-model="searchQuery" @input="onTyping()" />
  </div>


  <div class="card-body">
    <h5 v-if="this.giphies.length" class="card-title">Results</h5>
          <div id="giphyImages">
            <gif v-for="gif in giphies" :gif="gif" :key="gif.id"/>
          </div>
  </div>
  <div class="card-footer text-muted">
      <div class="Pagination text-center w-100 m-auto">
        <pagination v-model="page" :records="this.totalRecords" :per-page="12" @paginate="startSearch(page)"/>
      </div>
      
      <div id="app" v-if="this.giphies.length">
      <div class="container">
        <div class="row">
          <div class="col-sm-12">
            <div class="form-control d-flex align-items-center px-20px mb-10px w-50 m-auto">
                <span class="btn btn-info text-white copy-btn m-auto" @click.stop.prevent="copyLink">
                  Copy link
                </span>
                <input type="hidden" id="search-url" :value="'https://giphy-azure.vercel.app/?query='+this.searchQuery+'&offset='+this.page">
            </div>
          </div>
        </div>
      </div>
      </div>

      <a href="https://developers.giphy.com/docs/api#quick-start-guide" target="_blank">
        <img src="../assets/giphy.png" alt="" class="giphyAttribution" />
      </a>
  </div>
</div>

</template>


<script>
import axios from "axios";
import Pagination from 'v-pagination-3';
import Gif from "@/components/Gif";

export default {
  name: 'GiphySearch',
  props: ['Gifs'],
  components: {
    Gif,
    Pagination
  },
  setup() {

  },
  data() {
        return {
            searchQuery: this.$route.query.query,
            giphies: [],
            timeout: null,
            page: this.$route.query.offset ?? 1,
            totalRecords: 0,
            paginationRecord: [],
            offset: 0,
        };
  },
  methods: {
    onTyping() {
      clearTimeout(this.timeout);
      this.timeout = setTimeout(() => {
        this.startSearch();
      }, 300)
    },
    startSearch: function(offset = 0) {

      switch (offset) {
        case 0:
          break;
        case 1:
          offset == 0;
          break;
        default:
          offset == (offset * 12) - 12;
      }
      if(offset == 1 ? offset = 0: offset > 1 ? offset = offset * 10 + 1: offset);
      let API_KEY = 'Vi7Nr1eFpasZNZSHg9yAoAbHmu3IP03V';
      let link = 'https://api.giphy.com/v1/gifs/search?api_key=' + API_KEY + '&limit=12&offset=' + offset + '&rating=g&lang=en&q=';
      let searchEndPoint = link + this.searchQuery;

      axios.get(searchEndPoint).then((response) => {
                this.giphies = response.data.data;
                this.paginationRecord = response.data.pagination;
                this.totalRecords = this.paginationRecord.total_count;
                    console.log(response);
                }).catch((error) => {
                    console.log(error);
      });

    },
    copyLink () {
          let LinkToCopy = document.querySelector('#search-url')
          LinkToCopy.setAttribute('type', 'text') 
          LinkToCopy.select()

          try {
            var successful = document.execCommand('copy');
            var msg = successful ? 'successful' : 'unsuccessful';
            alert('Search link copied ' + msg);
          } catch (err) {
            alert('Oops, unable to copy');
          }

          /* unselect the range */
          LinkToCopy.setAttribute('type', 'hidden')
          window.getSelection().removeAllRanges()
    },
  },
  created: function () {
    if(this.$route.query.query !== undefined && this.$route.query.query !== '') {
      this.startSearch(this.$route.query.offset);
    }
  }
  

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  margin: 40px 0 0;
}

a {
  color: #42b983;
}
.giphyAttribution {
    margin-top: 30px;
}
#giphyImages {
    margin-top: 30px;
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    width: 70%;
    justify-content: center;
    margin: auto;
    margin-top: 30px;
  }
.VuePagination__pagination.pagination {
    justify-content: center !important;
    margin-top: 20px;
    display: flex;
}
.VuePagination {
    justify-content: center !important;
    display: flex;
    font-size: small;
}
.page-link {
    color: #47ac41;
}
.pagination {
    display: inline-flex !important;
    padding-left: unset;
}
</style>
