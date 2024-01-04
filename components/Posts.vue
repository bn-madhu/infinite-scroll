<template>
  <div class="card-container">
    <h1>This is post component</h1>
    <div class="card" v-for="(title, index) in titles" :key="index"> 
      <div style="padding: 16px 0;">{{ title.body }}</div>
    </div>
  </div>
</template>

<script>
  export default{
    name: "Post-component",
    data(){
      return{
        titles: [],
        page: 1,
      }
    },
    async mounted() {
      await this.getData();
      window.addEventListener('scroll', this.scrollBottom)
    },
    methods:{
      async getData(){
        await fetch("https://jsonplaceholder.typicode.com/posts?_page=" + this.page )
        .then(res=>res.json())
        .then(data=>{
            this.showUser(data);
          }
        )
      },
      showUser(data){
        // this.titles = [...this.titles, ...data];
        this.titles.push(...data)
        // console.log(this.titles);
      },
      async scrollBottom(e){
        let cardContainerPosition = document.querySelector('.card-container').getBoundingClientRect();
        // 632.875
        if(document.documentElement.clientHeight >= cardContainerPosition.bottom+7.875){
          this.page++;
          await this.getData();
        }
      }
    }
  }
</script>


<style scoped>
.card-container{
  background-color: #70708b;
  color: #fff;
  padding: 16px;
  border-radius: 8px;
}
  .card{
    border: 1px solid #ddd;
    border-radius: 8px;
    padding: 16px 24px;
    margin-bottom: 8px;
    box-shadow: 0 0 6px #ddd;
  }
  .card:last-child {
    margin-bottom: 0;
  }
</style>