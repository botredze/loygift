<template>
  <div class="modal fade right "  id="push-notification" tabindex="-1" role="dialog" aria-labelledby="add-products" aria-hidden="true">
    <div class="modal-dialog modal-full-height myModal-dialog mr-0 mt-0 mb-0 mr-0 h-100" style="max-width: calc(100% - 250px);" role="document" >
      <div class="modal-content myModal-content h-100">
        <div class="modal-header justify-content-start align-items-center">
          <button type="button" data-dismiss="modal" aria-label="Close" class="close">
              <span aria-hidden="true">
                <img src="../../assets/icons/xBlack.svg" alt="">
              </span>
          </button>
          <h3 class="modal-title" @click="sends">Push notification</h3>
        </div>
        <div class=" myModal-body">
          <div class="row">
            <div class="col-lg-8">
              <div class="d-flex client-all justify-content-between">
                <h4 class="push-title">Manual selection of customers</h4>
                <div class="d-flex align-items-center">
                  <div class="table-head "><label class="custom-checkbox mr-2 "><input  v-model="selectAll" @click="toggleSelect" type="checkbox"><span class="checkmark"></span></label></div>
                  <span class="send-all">Send to all</span>
                </div>
              </div>

              <div class="main-search d-flex align-items-center ">
                <img src="../../assets/icons/search-icon.svg">
                <input class="main-input" type="text" placeholder="Search" v-model="search_client">
              </div>

              <div class="d-flex main-content-header">
                <div class="table-head" style="width: 52%;">Name</div>
                <div class="table-head" style="width:35%">Category</div>
                <div class="table-head" style="width:13%">Bonus</div>
              </div>

              <div class="table-content">
                <div v-for="client in filteredClients" :key="client._id" class="table-item d-flex align-items-center">
                  <div class="table-head" style="width: 5%;"><label class="custom-checkbox"><input @click="checkMainSelect" type="checkbox"  :ref="`select${client._id}`"><span class="checkmark"></span></label></div>
                  <div  class="d-flex align-items-center"  style="width: 47%;">
                    <div class="table-img">
                      <img v-if="!client.avatar" src="../../assets/icons/chat.svg">
                      <img v-else :src="imgSrc+'/'+client.avatar">
                    </div>
                    {{client.name}}
                  </div>
                  <div style="width:35%">{{client.category ? client.category.name : "No cat" }}</div>
                  <div style="width:13%">{{client.points}}</div>

                </div>


              </div>

            </div>

            <div class="col-lg-4 pb-5">
              <div class="choose-category">
                <h2 class="notification-title">Choose from category</h2>
                <input class="cashback-input" placeholder="Search" v-model="search_category">
                <ul style="height:120px" class="push-cat p-0 m-0">
                  <li v-if="filteredCategories.length>0" class="category-list"><div  v-for="category in filteredCategories" :key="category._id" @click="filterClient = category._id" :class="{active: filterClient === category._id}">{{category.name}}</div></li>
                  <li v-else class="category-list">You have no categories</li>
                </ul>
              </div>

              <h3 class="notification-title">Select by last purchase date</h3>
              <div class="selects">
                <select v-model="last_purchase_filter" class=" form-control long-form-control  form-control-lg" aria-label=".form-select-lg example">
                <option v-for="(date,index) in lastMonth" :key="index" :value="date.date">{{date.name}}</option>
                </select>
              </div>

              <h3 class="notification-title">News</h3>
              <input :disable="newData.title!=='' || newData.description!==''" :class="{disableInput: newData.title!=='' || newData.description!==''}"  class="cashback-input news-input" placeholder="Select news" v-model="search_news">

            <div class="parent-news">
              <div v-if="search_news!==''" class="news pt-3">
                <div v-if="filteredNews.length>0">
                  <div @click="selectedNews(news)" class="news-title d-flex justify-content-between align-items-center pr-3 pl-3" v-for="news in filteredNews" :key="news._id">
                    {{news.name}}
                  </div>
                </div>
                <div class="pl-3" v-else>
                  You have no news
                </div>
              </div>
            </div>

              <div v-if="newsObj !== ''" class="sale d-flex align-items-center justify-content-between">
                <div style="width:100%">
                  <h4 class="sale-title">{{newsObj.name}}</h4>
                  <span class="news-desc long-text">{{newsObj.desc}}</span>
                </div>
                <img @click="clearNews" src="../../assets/icons/deleteClient.svg">
              </div>

              <h3 class="notification-title">Custom text</h3>
              <input :disabled="newData.news!==''" :class="{disableInput:newData.news!==''}" v-model="newData.title"  class="cashback-input mb-3" placeholder="Title">
              <textarea :disabled="newData.news!==''" :class="{disableInput:newData.news!==''}"  v-model="newData.description"  class="general-area p-2" placeholder="Description"></textarea>
              <button class="save" @click.prevent="onSubmit">Send</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<script>


import $ from "jquery";

export default {
  name: "Push notification",
  props:['clientList'],
  data(){
    return{
      lastMonth:[
        {date: this.$moment().subtract(2,'months').endOf('month').format('YYYY-MM-DD'), name:'1 month'},
        {date: this.$moment().subtract(3,'months').endOf('month').format('YYYY-MM-DD') , name:'2 month'},
        {date: this.$moment().subtract(4,'months').endOf('month').format('YYYY-MM-DD') , name:'3 month'},
        {date: this.$moment().subtract(5,'months').endOf('month').format('YYYY-MM-DD'), name:'4 month' },
        {date: this.$moment().subtract(6,'months').endOf('month').format('YYYY-MM-DD'), name:'5 month'},
        {date: this.$moment().subtract(7,'months').endOf('month').format('YYYY-MM-DD'), name:'6 month'},
        {date: this.$moment().subtract(13,'months').endOf('month').format('YYYY-MM-DD'),name:'12 month'},
      ],
      imgSrc:"",
      newsList:[],
      clientCategory:[],
      search_news:'',
      search_client:'',
      search_category:'',
      selectAll:false,
      filterClient:'',
      newsObj:'',
      last_purchase_filter:'2021-08-01',
      newData:{
        clients:[],
        news:'',
        title:'',
        description:'',
        sendToAll:false
      }
    }
  },
  computed:{
    filteredClients(){
      return this.clientList
          .filter((item)=>{
            return item.name.toLowerCase().includes(this.search_client.toLowerCase())
          })
          .filter((item)=>{
            if(item.category){
              return item.category._id.includes(this.filterClient);
            }else{
              return item;
            }
          })
          // .filter((item)=>{
          //    if(item.last_purchase){
          //      return new Date(item.last_purchase).getTime() >= new Date(this.last_purchase_filter).getTime()
          //
          //    }
          // })



    },
    filteredCategories(){
      return this.clientCategory.filter((item)=>{
        return item.name.toLowerCase().includes(this.search_category.toLowerCase())
      })
    },
    filteredNews(){
      return this.newsList.filter((item)=>{
        return item.name.toLowerCase().includes(this.search_news.toLowerCase())
      })
    },

  },
  methods:{
    sends(){
      console.log(this.clientList)
      let x = this.$moment().format('YYYY-MM-DD')
      // x.setDate(1);
      // x.setMonth(x.getMonth()-1);
      console.log(x)
    },
    clearNews(){
      this.newsObj = '';
      this.newData.news = ''
    },
    selectedNews(selected){
      this.newData.news = selected._id
      this.newsObj = selected
      this.search_news = ''
    },

    getCategories(){
      this.axios.get(this.url('getCategories')+'?type=client')
          .then((response)=>{
            this.clientCategory = response.data.objects
            this.clientCategory.unshift({_id:'',name:'All'})
          })
    },
    toggleSelect(){
      console.log(new Date(this.last_purchase_filter).getTime())
      this.clientList.forEach((client)=>{
        if(this.$refs[`select${client._id}`]!==undefined && this.$refs[`select${client._id}`] !== null){
          if(this.selectAll === false){
            this.$refs[`select${client._id}`].checked = true
          }
          else{
            this.$refs[`select${client._id}`].checked = false
          }
        }
      })
    },
    checkAll(item) {
      if(this.$refs[`select${item._id}`] !== null){
        return  this.$refs[`select${item._id}`].checked === true
      }
    },
    checkMainSelect() {
      if(this.clientList.every(this.checkAll)){
        this.selectAll = true
      }
      else{
        this.selectAll = false;
      }
    },
    getNews(){
      this.axios.get(this.url('getNews'))
          .then((response) => {
            this.newsList = response.data.objects;
          })
    },
    onSubmit(){
      const new_data = this.newData;
      this.clientList.forEach((client)=>{
        if(this.$refs[`select${client._id}`]!==undefined && this.$refs[`select${client._id}`] !== null){
          if(this.$refs[`select${client._id}`].checked === true){
            if(!new_data.clients.includes(client._id)){
              new_data.clients.push(client._id)
            }
          }
        }
      })
      if(this.selectAll === true){
          new_data.sendToAll = true
      }

      console.log(new_data)
      if(new_data.clients.length === 0 ){
        this.$warningAlert('Please select whom you want to send push notification')
      }
      else{
        const form = new FormData();
        if(new_data.news !== ''){
          form.append('news', new_data.news)
        }
        else{
          form.append('title', new_data.title);
          form.append('description', new_data.description);
        }

        form.append('sendToAll', new_data.sendToAll);
        form.append('clients', new_data.clients);
        this.axios.post(this.url('sendPushNotification'),form)
            .then((res)=>{
              this.$successAlert('Push has been sent')
              console.log(res, 'Success push')
              //clear
              this.newData = {
                clients:[],
                news:'',
                title:'',
                description:'',
                sendToAll:false
              }
            })
        $('#push-notification').modal("hide")
      }

    },

  },
  mounted(){
    this.imgSrc=this.$server
    this.getCategories()
    this.getNews()

  }




}
</script>

<style scoped>
.category-list .active{
  background: #EBEEFF;
  color: #616CF5;
}
.news-title{
  font-size:14px;
  padding:7px 0;
  cursor:pointer;
}
.news-title:hover{
  background: #fafafa;
}
.parent-news{
  position: relative;
}
.news-desc{
  color:#8C94A5;
  font-size: 14px;
}
.long-text{
  width: 280px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  display: block;
}
.news{
  height:100px;
  background: #fff;
  position: absolute;
  width: 100%;
  top:-15px;
  max-height:200px;
  overflow-y: auto;
  box-shadow: 2px 11px 35px rgba(0, 0, 0, 0.1);
  z-index: 99;
}
.push-cat{
  overflow-y:auto;
}
.save{
  width: 120px;
}
.general-area{
  margin-bottom: 20px;
}
.news-input{
  margin-bottom: 20px;
}
.sale{
  border-bottom: 1px solid #D3D3D3;
  padding-bottom: 14px;
  margin-bottom: 20px;
}
.sale-input{
  border:none;
  height: 100%;
  width: 100%;

}
.form-control{
  background: none;
}
.sale-title{
  font-size: 14px;
  font-weight: normal;
  margin-bottom: 3px;
}
.notification-title{
  font-size: 16px;
  font-weight: normal;
  margin-bottom: 12px;
}
.choose-category{
  background: #F8F9FB;
  border-radius: 10px;
  padding: 20px;
  margin-bottom: 20px;
}
.choose-category .cashback-input{
  height: 35px;
  background: none;
  margin-bottom: 20px;
}

.cashback-input{
  width: 100%;
}
.push-notification{
  padding: 0 30px;
}

.push-title{
  font-size: 18px;
  font-weight: normal;
}
.client-all{
  margin-bottom: 21px;
}
.main-search{
  margin-bottom: 30px;
}
.category-list div{
  padding: 5px 10px;
}
.category-list{
  list-style-type:none;
  cursor:pointer;
}

.selects:before{
  content:'';
  background: url("../../assets/icons/selectDown.svg") no-repeat;
  width:20px;
  height:20px;
  position: absolute;
  z-index:88;
  right: 20px;
  top:27%;


}
.selects{
  position: relative;
  margin-bottom: 20px;
}
</style>