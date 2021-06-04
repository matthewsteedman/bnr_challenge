<template>
    <h3>TODAYS TOP HEADLINES</h3>
    <!-- input for the search function and the button to make it function -->
    <div class="search_form">
      <q-form
            @submit="onSubmit"
            class="q-gutter-md"
            
            >
              <q-input 
              filled 
              placeholder="Search" 
              class="search"
              autocorrect="on"
              autocomplete="on"
              v-model= data.title
              hint="china, testa, bitcoin"
              ></q-input>

              <q-btn 
              type="submit"
              class="search_btn" 
              :loading="loading[0]" 
              color="secondary" 
              @click="simulateProgress(0)" 
              label="Search" />
      </q-form>
    </div>  
        <q-form  @submit="all_articles"
                  class="q-gutter-md">

                  <q-btn 
                    type="submit"
                    class="topheadlins_btn" 
                    :loading="loading[1]" 
                    color="secondary" 
                    @click="simulateProgress(1)" 
                    label="Top Headliness" />
          </q-form>
    <!-- all articles -->

    <div class="row blog_container" v-if="items">
        <q-item class="col-xs-12 col-sm-6 col-md-4" v-for="articles in items" :key="articles.id">
          <q-card class="my-card">
            <img :src="articles.urlToImage">
            <q-card-section>
              <h6>{{ articles.title }}</h6>
              <div class="text-subtitle2">{{ articles.description }}</div>
            </q-card-section>

            <q-card-section class="q-pt-none">
              {{ articles.publishedAt }}
            </q-card-section>
          </q-card>
        </q-item>
     </div>
</template>

<script>
import { ref } from 'vue'

const data = ({ref})

export default {
  name: 'HelloWorld',
  data() {
    return {
      items: undefined,
    }
  },
  created(){
    this.all_articles()
  },
  methods: {
    // function to get all articles

   all_articles(){
        try {
          const url = "http://localhost:8000/api/articles";
          fetch(url)
          .then((response) => response.json())
          .then((json) => {
            console.log(json.stringify);
            this.items = json.articles
              console.log("ITEMS", this.items)
            })
        } catch (error) {
          console.log("Server Error")
        }
    },
    // function to search through articles

   onSubmit (){
          try {
            fetch(`http://localhost:8000/api/search`, {
              method: "POST",
              body: JSON.stringify(this.data),
              headers: {
                "Content-Type": "application/json"
              }
            })
                .then((response) => response.json())
                .then((json) => {
                  if (this.data.title){
                    this.items = json.articles
                  }
                  
                  })
          } catch (error) {
            console.log(error)
          }
      }
  },

  setup(){
    const loading = ref([
      false,
    ])
    const progress = ref(false)

     function simulateProgress (number) {
      // we set loading state
      loading.value[ number ] = true

      // simulate a delay
      setTimeout(() => {
        // we're done, we reset loading state
        loading.value[ number ] = false
      }, 3000)
     }
    return {
      items: ref([]),
      filtered_data: ref([]),
      loading,
      progress,
      simulateProgress,
      data,
       skeletonTypes: [
        'text', 'rect', 'circle',
        'QBtn', 'QBadge', 'QChip', 'QToolbar',
        'QCheckbox', 'QRadio', 'QToggle',
        'QSlider', 'QRange', 'QInput',
        'QAvatar'
      ]
    }
  }
}

</script>

<style>
body{
    background-color: #2c2c2c25;
}
.my-card{
  width: 100%;  
  max-width: 400px;
  
}
.blog_container{
  width: 100%;
}
.search_form{
  margin: 20px;
}
.q-gutter-md .topheadlins_btn{
   margin-right: auto;
  margin-left: auto;
  display: block;
}
.q-gutter-md .search_btn{
  margin-right: auto;
  margin-left: auto;
  display: block;
}
.q-gutter-md .search{
  width: 25%;
  display: block;
  margin-right: auto;
  margin-left: auto;
}
.parent_div{
  margin: 0 -5px;
}
h6{
  font-weight: 200;
  font-family: sans-serif;
  text-transform: capitalize;
}

h3{
  font-weight: 200;
  text-align: center;
  font-family: sans-serif;
  text-transform: capitalize;
}

</style>