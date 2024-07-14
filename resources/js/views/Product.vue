<template>
    <section class="product-hero-section">
        <div v-if="isLoadling">
            <Loader /> 
        </div>
        <div v-else class="section-container">
            <div class="section-container__row">
                <div class="product-slider-outer">
                    <div class="product-slider">
                        <div class="slider product-slider__main">
                            <div class="product-slider__main--box">
                                <img :src="images[activeImage] || ''">
                            </div>
                        </div>
                        <div class="slider product-slider__nav">
                            <div class="product-slider__nav--box" v-for="(image,key) in images" :key="key" :class="{'slick-current': activeImage == key}" @click="activeImage = key">
                                <img :src="image">
                            </div>
                        </div>
                    </div>
                </div>
                <div class="product-information-outer">
                    <div class="product-information">
                        <h1 class="product-information__title">
                            {{ data?.name || '' }}
                        </h1>
                        <p class="product-information__description">
                            {{ data?.description || '' }}
                        </p>
                        <div class="product-information__price">
                            <div class="product-information__price--text">
                                <span>${{ data?.price?.discounted || 0.00 }}</span>
                                <del>${{ data?.price?.full || 0.00 }}</del>
                            </div>
                            <span class="product-information__price--offer-percentage">{{ data?.discount?.amount || 0 }}{{ data?.discount?.type == 'percent' ? '%' : '' }}</span>
                        </div>
                        <div class="product-information__addtocart-box">
                            <div class="product-information__addtocart-box--quantity-input">
                                <input type="number" v-model="quantity">
                                <span class="minus-for-quantity" @click="decreaseQuantity">
                                    <img src="/images/minus-icon.svg" alt="">
                                </span>
                                <span class="plus-for-quantity" @click="increaseQuantity">
                                    <img src="/images/plus-icon.svg" alt="">
                                </span>
                            </div>
                            <div class="product-information__addtocart-box--button">
                                <button @click="addToCart">
                                    <img src="/images/shopping-cart-icon.svg" alt="">
                                    Add to cart
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
  </template>
  
  <script>
  import Loader from './Loader.vue';
  export default {
    data() {
      return {
        data:null,
        images:[],
        activeImage:0,
        quantity:1,
        isLoadling : false
      };
    },
    components:{
        Loader
    },
    methods: {
      async fetchProduct()
      {

        this.isLoadling = true;
        try 
        {
            const slug = this.$route?.params?.slug || '';
            
            if(slug)
            {
                const response = await fetch("/client/products/"+slug);
                if (!response.ok) {
                    this.$router.push({ name: 'not-found' });
                }
                const data = await response.json();
                this.data = data?.data || null;
                this.images = data?.data?.images || [];
                this.isLoadling = false;
            }
            else
            {
                this.isLoadling = false;
                this.$router.push({ name: 'not-found' });
            }
        } catch (error) {
            this.isLoadling = false;
            this.$router.push({ name: 'not-found' });
        }
      },
      decreaseQuantity() {
        if (this.quantity > 1) this.quantity--;
      },
      increaseQuantity() {
        this.quantity++;
      },
      addToCart() {
        console.log(`Added ${this.quantity} items to the cart`);
      },
    },
    created()
    {
        this.fetchProduct()
    }
  };
  </script>
