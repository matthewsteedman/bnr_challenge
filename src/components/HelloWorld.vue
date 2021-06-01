<template>
<h3>Top Headlines</h3>

  <q-page-container>
    <div class="parent_div">
      <q-item v-for="articles in items" :key="articles.id">
      
     <q-card class="my-card">
       <img :src="articles.urlToImage">
      <q-card-section>
        <h6>{{ articles.author }}</h6>
        <div class="text-subtitle2">{{ articles.description }}</div>
      </q-card-section>

      <q-card-section class="q-pt-none">
        {{ articles.publishedAt }}
      </q-card-section>
    </q-card>
    </q-item>
     </div>
    </q-page-container>
  
</template>

<style>
body{
    background-color: #2c2c2c25;
}
.my-card{
  width: 100%;  
  max-width: 250px;

}
h3{
  text-align: center;
}

</style>

<script>
export default {
  name: 'HelloWorld',
    created(){
    this.all_articles()
  },
  methods: {
      all_articles(){
        try {
          const url = "https://newsapi.org/v2/top-headlines?country=us&apiKey=dd4c8ce956fb4fa98ada252f4e25153d";
          fetch(url)
          .then((response) => response.json())
          .then((json) => {
            console.log(json)
            this.items = json.articles
            console.log("ITEMS",this.items)
             })
        } catch (error) {
          console.log("Server Error")
          
        }
    }
  },

  setup(){
    return {
      items: []
    }
  }
}

</script>
