<template>
  <div class="modal fade right "  id="edit-product" tabindex="-1" role="dialog" aria-labelledby="edit-product" aria-hidden="true">
    <div class="modal-dialog modal-full-height myModal-dialog mr-0 mt-0 mb-0 mr-0 h-100" style="max-width:calc(100% - 250px)" role="document" >
      <div class="modal-content myModal-content h-100">
        <div class="modal-header justify-content-start align-items-center">
          <img data-dismiss="modal" aria-label="Close" class="close mr-2" src="../../../assets/icons/xBlack.svg" alt="">
          <h3 class="modal-title">Edit</h3>
        </div>
        <div class="myModal-body">
          <div class="row">
            <div class="col-11 m-auto">
              <form  class="modal-form">
                <div class="d-flex  mb-3">
                  <div style="width:50%" class="mr-3">
                    <label class="product-label">Name</label>
                    <input :class="{errorInput: validateName}" name="name"  v-model="currentData.name" style="width:100%" class="cashback-input">
                    <div v-if="validateName" class="fill-fields">Fill in the fields</div>
                  </div>

                  <div class="quantity-category mr-3">
                    <label class="product-label">Quantity</label>
                    <input :class="{errorInput: validateQuantity}" name="quantity"  v-model="currentData.quantity" class="cashback-input">
                    <div v-if="validateQuantity" class="fill-fields" >Fill in the fields</div>
                  </div>

                  <div style="width:25%;">
                    <label class="product-label">Select category</label>

                    <select v-if="currentData.category"   name="category" v-model="currentData.category._id"  class="form-control mb-0 select-phone" >
                      <option :value="cat._id" v-for="cat in listCategory.slice(1)" :key="cat._id">{{cat.name}}</option>
                    </select>

                    <select v-else  name="category" v-model="no_category"  class="form-control mb-0 select-phone" >
                      <option :value="cat._id" v-for="cat in listCategory.slice(1)" :key="cat._id">{{cat.name}}</option>
                    </select>
                  </div>
                </div>

                <label>Name in russian</label><br>
                <input v-model="currentData.name_ru" class="cashback-input mb-3" style="width:50%"><br>
                <label>Description</label>
                <textarea v-model="currentData.description" class="general-area p-3 mb-3" style="height:160px"   name="description"></textarea>

              <div class="d-flex mb-3">
                <label class="custom-checkbox">
                  <input v-model="currentData.hasMultipleTypes"   type="checkbox">
                  <span class="checkmark"></span>
                </label>
                <span>Has Sizes</span>
              </div>
              <div v-if="currentData.hasMultipleTypes">
                <div>SIZES</div>
                <div class="d-flex" style="justify-content: space-between;">
                  <div style="flex: 1 1 0px">Size</div>
                  <div style="flex: 1 1 0px">Quantity</div>
                  <div style="flex: 1 1 0px">Price</div>
                  <div style="flex: 1 1 0px">VendorCode</div>
                </div>
                <div v-for="(size, index) in currentData.sizes" :key="index" class="d-flex" style="justify-content: space-between;">
                  <div style="flex: 1 1 0px">{{ size.size }}</div>
                  <div style="flex: 1 1 0px">{{ size.quantity }}</div>
                  <div style="flex: 1 1 0px">{{ size.price }} </div>
                  <div style="flex: 1 1 0px">{{ size.vendorCode }}</div>
                </div>
              </div>

                <div v-else class="d-flex ">
                  <div style=" width:33.33%; margin-right:8px;">
                    <label>Price</label>
                    <input :class="{errorInput: validatePrice}" name="price" v-model="currentData.price" class="form-input cashback-input" placeholder="Price"  >
                    <div v-if="validatePrice" class="fill-fields">Fill in the fields</div>
                  </div>

                  <div style="width:33.33%;">
                    <label>Vendor Code</label>
                    <input name="vendorCode" v-model="currentData.vendorCode"  class="form-input cashback-input mb-4" placeholder="Vendor code"  >
                  </div>
                </div>

                <div class="d-flex mb-3 mt-3">
                  <label class="custom-checkbox">
                    <input v-model="showPrice" @change="checkDiscount"  id="edit-show-price" type="checkbox">
                    <span class="checkmark"></span>
                  </label>
                  <span>Discount</span>
                </div>
                <div class="d-flex ">
                  <div v-if="showPrice"  style="width:33.33%; margin-right:8px;">
                    <label>Promotional price (for all sizes)</label>
                    <input :class="{errorInput:validatePromoPrice}"  name="promoPrice" v-model="currentData.promoPrice" class="form-input cashback-input" placeholder="Price">
                    <div class="fill-fields" v-if="validatePromoPrice">Fill in the fields</div>
                  </div>

                </div>

                <label class="valid-label mt-4">Period of action</label>
                <div class=" product-calendar d-flex">
                  <div class="mr-2">
                    <label >From</label>
                    <div :class="{errorInput:validateFrom}" class="calendar d-flex align-items-center">
                      <input v-model="promoStart.formatted" id="promoStart_input" class="calendar-input">
                      <img src="../../../assets/icons/Calendar.svg">
                    </div>
                    <div class="fill-fields" v-if="validateFrom">Fill in the fields</div>
                  </div>

                  <div>
                    <label>To</label>
                    <div :class="{errorInput: validateTo}" class="calendar d-flex align-items-center">
                      <input v-model="promoEnd.formatted" id="promoEnd_input" class="calendar-input">
                      <img src="../../../assets/icons/Calendar.svg">
                    </div>
                    <div class="fill-fields" v-if="validateTo">Fill in the fields</div>
                  </div>

                </div>

                <div class="d-flex mb-3">
                  <label class="custom-checkbox"><input v-model="currentData.recommend"  type="checkbox" ><span class="checkmark"></span></label>
                  <span>Recommended</span>
                </div>

                <div class="modal-img ">
                  <label>Photos</label>
                  <p>
                    You can upload 4 more JPG or PNG photos, the minimum resolution is 400*400px, the size is<br>
                    not more than 3 MB. The first photo will be shown as the main one by default .</p>

                  <div class="d-flex">
                    <label >
                      <img src="../../../assets/img/modal-img.svg">
                      <input @change="onFileChange($event)" ref="addImage" class="d-none" multiple id="imgsArray" type="file" name="imgArray">
                    </label>
                    <div  class="d-flex">
                      <div v-for="(img, index) in imagePreview" :key="index" >
                        <div v-if="img" class="selected-images">
                          <img v-if="img ? img.startsWith('images'): ''" :src="imgSrc+'/'+img" class="show-images mr-2" />
                          <img v-else-if="img !== null"   :src="img" class="show-images mr-2" />
                          <div class="selected-overlay">
                            <img @click="removeImage(index)" class="remove-image"  src="../../../assets/icons/deleteClient.svg">
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
                <div class="modal-btn d-flex">
                  <button @click.prevent="onSubmit" class="save">Save</button>
                  <button type="button" class="cancel" data-dismiss="modal" aria-label="Close" >Cancel</button>
                </div>
              </form>
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
  name: "EditProduct",
  props: ['listCategory','select_product','getProducts'],
  data(){
    return{
      validateName:false,
      validateQuantity:false,
      validatePrice:false,
      showPrice:false,
      validateTo:false,
      validateFrom:false,
      validatePromoPrice:false,
      today:this.$moment().format("YYYY-MM-DD"),
      no_category:'',
      currentData:{
        img:'',
        imgArray:[]
      },
      imgSrc:'',
      promoStartLightpick:{},
      promoEndLightpick:{},
      promoStart: {
        obj:"",
        formatted:'',
      },
      promoEnd: {
        obj:"",
        formatted:'',
      },
    }
  },
  computed:{
    mainImg(){
      if(typeof this.currentData.img === 'object'){
        return URL.createObjectURL(this.currentData.img)
      }
      return this.currentData.img
    },
    imagePreview(){

      return  this.currentData.imgArray.map((item)=>{
        if(this.currentData.imgArray.length>0){
          if(item !== null){
            if(typeof item === 'object'){
              return URL.createObjectURL(item)
            }
          }
          return item;

        }
      })
    },

  },
  methods:{

    removeImage(idx){
      this.currentData.imgArray.forEach((item,index)=>{
        if(index === idx){
          this.currentData.imgArray[index] = ""
        }
      })
      console.log(this.currentData.imgArray)
    },

    checkDiscount(){
      if(this.showPrice===false){
        this.promoStart = {
          obj:'',
          formatted:'',
        }
        this.promoEnd={
          obj:'',
          formatted:'',
        }
        this.currentData.promoPrice = 0
      }
    },
    selectDates(){
      let that=this;
      this.promoStartLightpick = new this.$lightpick({
        field: document.getElementById('promoStart_input'),
        onSelect: function(date){
          that.promoStart.obj = date;
          that.promoStart.formatted = date.format('YYYY-MM-DD');
        }
      });
      this.promoEndLightpick = new this.$lightpick({
        field: document.getElementById('promoEnd_input'),
        onSelect: function(date){
          that.promoEnd.obj = date;
          that.promoEnd.formatted = date.format('YYYY-MM-DD');
        }
      });
    },
    onFileChange() {
      var valid = ["image/png", "image/jpg", "image/jpeg", "image/svg"];
      $.each($("#imgsArray")[0].files, (i,file)=> {
        console.log(i)
        if(file && file.size > 3000000){
          this.$warningAlert('Image size exceeds 3 mb ')
        }
        else if(file && !valid.includes(file.type)){
          this.$warningAlert('Image type not png or jpg')
        }
        else{
          if(this.currentData.imgArray.length<4) {
            this.currentData.imgArray.push(file)

          }
          else{
            for (let j = 0; j <this.currentData.imgArray.length; j++) {
              if(this.currentData.imgArray[j] === ''){
                this.currentData.imgArray[j] = file;
                break;
              }
              else{
                this.$warningAlert('Maximum is 4 images')
              }
            }
          }
        }
      })
      console.log(this.currentData.imgArray,'imgArray')
    },

    onSubmit(){
      const updatedProduct = this.currentData;
      const form = new FormData();
      const img = updatedProduct.imgArray.find((item=>item!==''))
      if(!img){
        form.append('img', '')
      }else{
        form.append('img',img)

      }
      const newImgArray = []
      for(let item in updatedProduct.imgArray){
        if(updatedProduct.imgArray[item] !== img){
          newImgArray.push(updatedProduct.imgArray[item])
        }
      }
      for (let itm in newImgArray) {
        form.append('imgArray'+itm,  newImgArray[itm])


      }
      if(updatedProduct.name === ""){
        this.validateName = true
      }
      else{
        this.validateName = false
      }

      if(updatedProduct.quantity === ""){
        this.validateQuantity = true
      }
      else{
        this.validateQuantity = false
      }

      if(updatedProduct.price === ""){
        this.validatePrice = true
      }
      else{
        this.validatePrice = false
      }
      if(updatedProduct.category){
        form.append('category',updatedProduct.category._id)
      }
      else{
        form.append('category',this.no_category)
      }

      if(updatedProduct.promoPrice > updatedProduct.price){
        this.$warningAlert('Promotional price must be less than original price')
        return;
      }

      if(this.showPrice===true){
        if(updatedProduct.promoPrice === 0 || updatedProduct.promoPrice === ''){
          this.validatePromoPrice = true
        }
        else{
          this.validatePromoPrice = false
        }
        if(this.promoStart.obj === ''){
          this.validateFrom = true;

        }
        else{
          this.validateFrom  = false
        }

        if(this.promoEnd.obj === ''){
          this.validateTo = true;
          return;
        }
        else{
          this.validateTo  = false
        }
      }
      else{
        this.validateFrom = false;
        this.validateTo = false;
        this.validatePromoPrice = false;
      }



      if((new Date(this.promoEnd.formatted).getTime() < new Date(this.today).getTime())){
        this.$warningAlert('End date must greater than todays date')
        return;
      }

      form.append("name", updatedProduct.name)
      form.append("name_ru", updatedProduct.name_ru)
      form.append("quantity", updatedProduct.quantity)
      form.append("price", updatedProduct.price)
      form.append("description", updatedProduct.description)
      form.append("promoPrice", updatedProduct.promoPrice)
      form.append("vendorCode", updatedProduct.vendorCode)
      form.append("recommend", updatedProduct.recommend)
      form.append('promoStart',this.promoStart.obj)
      form.append('promoEnd',this.promoEnd.obj)

      if(updatedProduct.promoPrice > updatedProduct.price){
        this.$warningAlert("Promotional price must be < original price")
      }
      else{
        this.axios.put(this.url('updateProduct',updatedProduct._id),form)
            .then(()=>{
              this.getProducts()
              this.$informationAlert('Changes are saved')
              $('#edit-product').modal("hide")
              this.validateFrom = false;
              this.validateTo = false
              this.no_category = '';
              this.validateQuantity = false;
              this.validateName = false;
              this.validatePrice = false;
            })
      }


    }
  },

  watch:{
    select_product(newCat){
      this.currentData = Object.assign({}, newCat);

      if(this.currentData.img){
        this.currentData.imgArray.unshift(this.currentData.img);
      }
      if(this.currentData.promoPrice>0){
        this.showPrice = true;
      }
      else{
        this.showPrice = false
      }
      delete this.currentData['img']
      // this.promoStart.obj = this.currentData.promoStart;
      this.promoStartLightpick.setDate(this.currentData.promoStart);
      // this.promoEnd.obj = this.currentData.promoEnd;
      this.promoEndLightpick.setDate(this.currentData.promoEnd);

    },
    currentData:{
      handler(val) {
        console.log(val.promoPrice, !val.promoPrice || val.promoPrice<=0)
        if(!val.promoPrice || val.promoPrice<=0){
          this.unSetDates
          this.promoEnd.obj = '';
          this.promoEnd.formatted = "";
          this.promoStart.obj = '';
          this.promoStart.formatted = "";
        }
      },
      deep: true,
      immediate:true,
    },
  },
  mounted() {
    this.selectDates();
    this.imgSrc = this.$server;
  },
}
</script>

<style scoped>
.show-images{
  object-fit: contain;
}
.selected-images:hover .remove-image{
  opacity: 1;
}

.remove-image{
  opacity:1;
  transition:.3s;
}
.modal.fade:not(.in).right .modal-dialog {
  -webkit-transform: translate3d(0,0,0);
  transform: translate3d(0, 0, 0);
}
.valid-label{
  margin-bottom: 22px !important;
}
.modal-header .close{
  margin: 0;
}

.fill-fields{
  color:#E94A4A;
  margin-top: 5px;

}
.show-price.active{
  display:block;
}
.product-calendar{
  margin-bottom: 40px;
}

.calendar{
  margin-bottom: 0;
}
.modal-form input{
  width: 100%;
  font-size: 16px;
}
.modal-form label{
  margin-right: 10px;
  margin-bottom: 8px;
}
.modal-img{
  margin-bottom: 60px;
}
.modal-img p{
  margin-bottom: 15px;
  color: #828282;
  font-size: 12px;
}


</style>