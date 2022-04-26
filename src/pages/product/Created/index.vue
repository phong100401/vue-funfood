<template>
  <a-form :form="form" :label-col="{ span: 5 }" :wrapper-col="{ span: 12 }" >
    <a-form-item label="Product Name">
      <a-input v-model="form.name"/>
      <span id="error-name" style="color: red"></span>
    </a-form-item>
    <a-form-item label="Price">
      <a-input v-model="form.price"/>
      <span id="error-price" style="color: red"></span>
    </a-form-item>
    <a-form-item label="Category">
      <a-select v-model="form.categoryId">
        <a-select-option v-for="item in data" :key="item.id" :value="item.id">
          {{item.name}}
        </a-select-option>
      </a-select>
      <span id="error-category" style="color:red;"></span>
    </a-form-item>
    <a-form-item label="Description" >
      <a-input v-model="form.description" />
    </a-form-item>
    <a-form-item label="Quantity">
      <a-input v-model="form.quantity"/>
    </a-form-item>
    <a-form-item label="Thumbnail">
      <a-input v-model="form.thumbnail"/>
    </a-form-item>
    <a-form-item label="Status" hidden>
      <a-input v-model="form.status"/>
    </a-form-item>
    <a-form-item style="height: auto" label="Detail">
<!--      <ckeditor></ckeditor>-->
      <a-input v-model="form.detail"/>
    </a-form-item>
    <a-form-item :wrapper-col="{ span: 12, offset: 5 }">
      <a-button type="primary" @click.stop.prevent="handleSubmit">
        Submit
      </a-button>
    </a-form-item>
  </a-form>
</template>

<script>
import CategoryService from "@/service/CategoryService";
import ProductService from "@/service/ProductService";
export default {
  data() {
    return {
      data:[],
      form:{
        name: undefined,
        price: undefined,
        thumbnail: undefined,
        categoryId: undefined,
        quantity: undefined,
        detail: undefined,
        description: undefined,
        status: 'ACTIVE'
      }
    };
  },
  created() {
    this.getCategorise()
  },
  methods: {
    getCategorise(){
      CategoryService.getAll().then(
          res => {
            this.data = res.data
          }
      )
    },
    validate(){
      var errorName = document.getElementById('error-name')
      var errorPrice = document.getElementById('error-price')
      // var errorCategory = document.getElementById('error-category')
      if(this.form.name === undefined){
        errorName.innerText = 'Vui lòng nhập tên sản phẩm'
      }else {
        errorName.innerText = ''
      }
      if (this.form.price === undefined){
        errorPrice.innerText = 'Vui lòng nhập giá tên lớn hơn 0'
      }else {
        errorPrice.innerText = ''
      }
    },
    handleSubmit(e){
      e.preventDefault()
      if(!this.validate()){
        return
      }
      ProductService.save(this.form).then(
          this.$router.push("/products/list")
      ).catch(error => {
        console.log(error)
      })
    }
  },
};

</script>

<style scoped>

</style>