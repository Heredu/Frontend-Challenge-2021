<template>
  <div class="container">
    <div class="card">
      <div class="heading">
        <h4 class="title">NYTimes Books</h4>
      </div>
      <div v-show= "selectCategory" class="search" >
        <input v-model.lazy="search" type="text" class="form-control" placeholder="Search by title" v-on:keyup.enter="search" />
      </div>
      <div class="content">
        <!-- ADD CONTENT HERE --> 
          <div class="card">
            <label class="title" for="category">Category: </label>
              <select v-model="selectCategory" id="category">
                <option value="">Select Category</option>
                <option v-for="category of categories" :value="category" v-bind:key="category.display_name">{{category.list_name}}</option>
              </select>
            </div>
          </div>
          <div v-for="book of searchData" v-bind:key="book.book_title">
          <div class="list">
            <li>
              <a :href= "argumentsUrl(book.title+' '+book.author)" target="_blank">
                {{book.title}} - {{book.author}}         
              </a>
              <span>{{book.description}}</span>
              <hr>
            </li>  
          </div>
          
          </div>
    </div>
  </div>
</template>

<script>
import vue from "vue";
import axios from "axios";
import VueAxios from "vue-axios";
vue.use(VueAxios, axios);

export default {
  name: "Books",
  data() {
    return {
      categories: [],
      selectCategory: '',
      search: null,
      url: 'https://www.google.com/search?q='
    };
  },

  methods: {
    argumentsUrl: function (arg) {
      return this.url + (arg.split(' ').join('+'))
    },
  },

  computed: {

        searchData: function () {
      if(this.search) {
        return this.selectCategory.books.filter((book) => {
        return book.title.toLowerCase().includes(this.search.toLowerCase())
        });        
      }
      /* else {
        return this.selectCategory.books
      } */

    }

  },

  mounted() {
    axios
      .get(
        "https://api.nytimes.com/svc/books/v3/lists/overview.json?api-key=gfERVdYhgWaYtsRfVSNiwPLRvm9XWYtq"
      )
      .then((response) => {
        
        this.categories = response.data.results.lists
        this.categories.length = 10
        /* eslint-disable no-console */
        console.log(this.searchData);
        /* eslint-enable no-console */

        /* eslint-disable no-console */
        console.log(this.categories);
        /* eslint-enable no-console */
      })
      .catch((err) => {
        /* eslint-disable no-console */
        console.log(err);
        /* eslint-enable no-console */
      }); 

  },
};
</script>

<style scoped lang="scss">

.container {
  z-index: 1;
  margin: 36px auto;
  max-width: 826px;
  background-color: white;
}

.card {
  box-shadow: 0 2px 3px rgba(10, 10, 10, 0.1), 0 0 0 1px rgba(10, 10, 10, 0.1);
  padding: 24px;
}

.list {
  > li {
    &:not(:last-child) {
      margin-bottom: 18px;
    }
    > a {
      color: #0a5b8c;
      display: block;
      margin-bottom: 6px;
    }

    > span {
      color: rgba(#3b4242, 0.7);
      font-size: 12px;
    }
    list-style:none;
    margin-top: 20px;
  }
}

.btn {
  color: #fff;
  cursor: pointer;
  background-color: #117a8b;
  border: 1px solid transparent;
  padding: 6px 12px;
  border-radius: 6px;
  transition: all 0.1s ease-in;
  &:hover {
    background-color: #138496;
    border-color: #117a8b;
  }
}

.heading {
  margin-bottom: 12px;

  .title {
    font-size: 18px;
    font-weight: 600;
  }
}
.search {
  margin-bottom: 24px;
  .form-control {
    background-color: transparent;
    border: none;
    border-bottom: 1px solid #ced4da;
    border-radius: 0;
    outline: 0;
    box-shadow: none;
    padding: 6px 0;
    width: 100%;
  }
}
/* #category {
  margin-right: 10px;
}
A {
  text-decoration: none;
  color: black;
 }
.description {
  margin: 10px;
}
h4 {
  display: inline-block;
  padding-right: 10px;
}
hr, .description {
  color: grey;
} */

</style>
