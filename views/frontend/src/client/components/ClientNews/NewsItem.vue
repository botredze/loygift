<template>
  <div class="news container">
    <div class="d-flex align-items-center mb-4" style="cursor:pointer" @click="$router.go(-1)">
      <img class="mr-2" src="../../../assets/clients/slide.svg">
      <h3 class="path-title">
        News
      </h3>
    </div>
    <div class="row">

      <div class="col-lg-3 col-md-4 col-sm-6 col-xs-6 pr-0 news-box" v-for="newss in news" :key="newss._id" @click="openNews(newss._id)">
              <div class="new-img">
<!--                <img :src="server+'/'+newss.img">-->
                <img v-if="!newss.error" :src="server+'/'+newss.img" @error="newss.error=true">
                <img v-else src="../../../assets/img/default.svg" >
              </div>
             <div class="news-text">
               <div class="d-flex align-items-center calendar-news justify-content-between" >
               <div>
                 <img src="../../../assets/icons/Calendar.svg">
                 <span class="date">
                 {{newss.updatedAt}}
               </span>
               </div>
                 <div  v-if="newss.startDate || newss.endDate">
                   <span class="date mr-2">Promotion</span>
                   <img style="width:23px;height:23px;" src="../../../assets/icons/promotion.svg">
                 </div>


               </div>
               <h4 class="news-content">{{newss.name}}</h4>
               <p class="news-description">
                 {{newss.desc}}
               </p>
             </div>
            </div>
        </div>
      </div>

</template>

<script>
export default {
  name: "NewsItem",
  props:['news'],
  computed:{
    currentCompanyCatalog() {
      return this.$route.params.bekon;
    },
    server(){
      return this.$server;
    },
  },
  methods:{
    openNews(id){
      this.$router.push(`/${this.currentCompanyCatalog}/news-detail/${id}`)
    },
  }
}
</script>

<style scoped>
.news{
  padding-right: 30px;
}
.news-box{
  margin-bottom: 37px;
  cursor:pointer;
}
.calendar-news{
  border-bottom: 1px solid #e3e3e3;
  margin-bottom: 10px;
  padding-bottom: 8px;
}
.new-img{
  height: 150px;
  width: 100%;
  margin-right: 10px;

}
.new-img img{
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 5px;

}
.date{
  color: #858585;
  font-size: 14px;

}
.news-box:hover .news-content{
  color:#616cf5;

}
.news-content{
  color:#222;
  font-size: 16px;
  font-weight: 600;
  margin-bottom: 10px;
}
.news-text{
  padding: 10px 0;
}
.news-text img{
  width: 15px;
  height: 14px;
  margin-bottom: 3px;
  margin-right: 5px;
}
.news-description{
  font-size: 14px;
  color:#858585;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 3; /* number of lines to show */
  -webkit-box-orient: vertical;
}
@media(min-width:1200px){
  .news{
    max-width: calc(100vw - 240px);
  }
}
</style>