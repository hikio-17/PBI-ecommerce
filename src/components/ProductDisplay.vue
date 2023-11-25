<script>
export default {
   data() {
      return {
         index: 1,
         product: {},
         loading: false,
      };
   },
   methods: {
      async getProduct() {
         this.loading = true;
         const response = await fetch(`https://fakestoreapi.com/products/${this.index}`);
         const responseJson = await response.json();

         if (responseJson.category === `men's clothing` || responseJson.category === `women's clothing`) {
            this.product = responseJson;
         } else {
            this.product = {}
         }
         this.loading = false;
      },
      nextProduct() {
         this.index = this.index % 20 + 1;
         this.getProduct();
      },
   },
   computed: {
      containerClasses() {
         return {
            'bg-blue': this.product.category === "men's clothing",
            'bg-pink': this.product.category === "women's clothing",
            'bg-gray': !['men\'s clothing', 'women\'s clothing'].includes(this.product.category),
         };
      },
      isProductUnavailable() {
         return !['men\'s clothing', 'women\'s clothing'].includes(this.product.category);
      },
      getTitleClass() {
         return this.product.category === `men's clothing` ? 'font-navy' : 'font-magenta';
      },
      getRatingClass() {
         return this.product.category === `men's clothing` ? 'circle bg-navy' : 'circle bg-magenta';
      },
      getPriceClass() {
         return this.product.category === `men's clothing` ? 'font-navy' : 'font-magenta';
      },
      getBuyButtonClass() {
         return this.product.category === `men's clothing` ? 'bg-navy' : 'bg-magenta';
      },
      getNextButtonClass() {
         const buttonClass = this.product.category === `men's clothing` ? 'border-navy font-navy' : 'border-magenta font-magenta';
         return Array.isArray(buttonClass) ? buttonClass : [buttonClass];
      },
   },
   mounted() {
      this.getProduct();
   },
};
</script>

<template>
   <div class="container">
      <div class="container" :class="containerClasses">
         <div class="overlay">
            <img src="../assets/img/bg-shape.svg" alt="background overlay">
         </div>
         <div class="card">
            <div v-if="loading" class="product-container">
               <div class="skeleton-thumbnail"></div>
               <div class="skeleton-details">
                  <div class="skeleton-details-top"></div>
                  <div class="skeleton-details-bottom"></div>
               </div>
            </div>

            <template v-else>
               <template v-if="isProductUnavailable">
                  <div class="product-unavailable-container">
                     <div class="overlay">
                        <img src="../assets/img/bg-sad.svg" alt="background unavailable product">
                     </div>
                     <div class="product-details">
                        <p>This product is unavailable to show</p>
                        <div class="cta">
                           <button type="button" class="cta-next" @click="nextProduct">Next Product</button>
                        </div>
                     </div>
                  </div>
               </template>

               <template v-else>
                  <div class="product-container">
                     <div class="product-thumbnail">
                        <img :src="product.image" alt="Product Image">
                     </div>

                     <div class="product-details">
                        <div class="top">
                           <h3 class="title" :class="getTitleClass">
                              {{ product.title }}</h3>
                           <div class="sub-title">
                              <span>{{ product.category }}</span>
                              <div class="rating">
                                 <span>{{ product.rating.rate }}/5</span>
                                 <div class="rating">
                                    <span class="circle" :class="getRatingClass" v-for="n in 5" :key="n"></span>
                                 </div>
                              </div>
                           </div>
                           <div class="description">
                              <p>{{ product.description }}</p>
                           </div>
                        </div>
                        <div class="bottom">
                           <span class="price" :class="getPriceClass">
                              {{ product.price }}
                           </span>
                           <div class="cta">
                              <button type="button" class="cta-buy"
                                 :class="getBuyButtonClass">Buy Now</button>
                              <button type="button" class="cta-next" :class="getNextButtonClass"
                                 @click="nextProduct">Next Product
                           </button>
                           </div>
                        </div>
                     </div>
                  </div>
               </template>
            </template>
         </div>
      </div>
   </div>
</template>
 