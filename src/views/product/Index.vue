<script>

export default {
  name: 'Index',
  data() {
    return {
      products: [],
      popupProduct: null,
      filterList: [],
      categories: [],
      colors: [],
      tags: [],
      prices: [],
      name: false,
      order: '',
      pagination: []
    }
  },
  mounted() {
    $(document).trigger('changed')
    this.getProducts()
    this.getFilterList()
    this.$nextTick(() => {
      // Навешиваем обработчик изменения на исходный select
      $('select').on('change', (e) => {
        this.order = String(e.target.value)
        this.filterProducts()
      })
    })

  },
  methods: {
    filterProducts() {
      this.getProducts()
    },
    onOrderChange() {
      console.log(this.order)
    },
    addColor(id) {
      if (!this.colors.some(elem => elem.id === id)) {
        this.colors.push({ id })
      } else {
        this.colors = this.colors.filter(elem => elem.id !== id)
      }
    },
    addTag(id) {
      if (!this.tags.some(elem => elem.id === id)) {
        this.tags.push({ id })
      } else {
        this.tags = this.tags.filter(elem => elem.id !== id)
      }
    },

    getProducts(page = 1) {
      this.axios.post('http://127.0.0.1:8876/api/products', {
        'categories': this.categories,
        'colors': this.colors,
        'tags': this.tags,
        'prices': this.prices,
        'order': this.order,
        'page': page
      })
        .then(res => {
          this.products = res.data.data
          this.pagination = res.data.meta
          console.log(res)
        })
        .finally(v => {
          $(document).trigger('changed')
        })
    },
    getProduct(id) {
      this.axios.get(`http://127.0.0.1:8876/api/products/${id}`)
        .then(res => {
          this.popupProduct = res.data.data
          console.log(res)
        })
        .finally(v => {
          $(document).trigger('changed')
        })
    },
    getFilterList() {
      this.axios.get(`http://127.0.0.1:8876/api/products/filters`)
        .then(res => {
          this.filterList = res.data
          //  Price Filter
          if ($('#price-range').length) {
            $('#price-range').slider({
              range: true,
              min: this.filterList.price.min,
              max: this.filterList.price.max,
              values: [this.filterList.price.min, this.filterList.price.max],
              slide: (event, ui) => {
                $('#priceRange').val(`$${ui.values[0]} - $${ui.values[1]}`)
                this.prices = [ui.values[0], ui.values[1]]
              }
            })
            $('#priceRange').val('$' + $('#price-range').slider('values', 0) + ' - $' + $('#price-range').slider('values', 1))
            this.prices = [this.filterList.price.min, this.filterList.price.max]
          }
        })
        .finally(v => {
          $(document).trigger('changed')
        })
    }
  }
}
</script>

<template>
  <div>
    <main class="overflow-hidden ">
      <!--Start Breadcrumb Style2-->
      <div class="breadcrumb-area" style="background-image: url(/src/assets/images/inner-pages/breadcum-bg.png);">
        <div class="container">
          <div class="row">
            <div class="col-xl-12">
              <div class="breadcrumb-content pb-60 text-center wow fadeInUp animated">
                <h2>Shop Grid</h2>
                <div class="breadcrumb-menu">
                  <ul>
                    <li><a href="index.html"><i class="flaticon-home pe-2"></i>Home</a></li>
                    <li><i class="flaticon-next"></i></li>
                    <li class="active">Shop Grid</li>
                  </ul>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!--End Breadcrumb Style2-->
      <!--Start Product Categories One-->
      <section class="product-categories-one pb-60">
        <div class="container">
          <div class="row wow fadeInUp animated">
            <div class="col-xl-12">
              <div class="product-categories-one__inner">
                <ul>
                  <li><a href="#0" class="img-box">
                    <div class="inner"><img src="/src/assets/images/shop/product-categories-v1-img1.png"
                                            alt="" /></div>
                  </a>
                    <div class="title"><a href="#0">
                      <h6>Accessories</h6>
                    </a></div>
                  </li>
                  <li><a href="#0" class="img-box">
                    <div class="inner"><img src="/src/assets/images/shop/product-categories-v1-img2.png"
                                            alt="" /></div>
                  </a>
                    <div class="title"><a href="#0">
                      <h6>Furnitures</h6>
                    </a></div>
                  </li>
                  <li><a href="#0" class="img-box">
                    <div class="inner"><img src="/src/assets/images/shop/product-categories-v1-img3.png"
                                            alt="" /></div>
                  </a>
                    <div class="title"><a href="#0">
                      <h6>Jewellery</h6>
                    </a></div>
                  </li>
                  <li><a href="#0" class="img-box">
                    <div class="inner"><img src="/src/assets/images/shop/product-categories-v1-img4.png"
                                            alt="" /></div>
                  </a>
                    <div class="title"><a href="#0">
                      <h6>Shoes</h6>
                    </a></div>
                  </li>
                  <li><a href="#0" class="img-box">
                    <div class="inner"><img src="/src/assets/images/shop/product-categories-v1-img5.png"
                                            alt="" /></div>
                  </a>
                    <div class="title"><a href="#0">
                      <h6>Electronics</h6>
                    </a></div>
                  </li>
                  <li><a href="#0" class="img-box">
                    <div class="inner"><img src="/src/assets/images/shop/product-categories-v1-img6.png"
                                            alt="" /></div>
                  </a>
                    <div class="title"><a href="#0">
                      <h6>Fashion</h6>
                    </a></div>
                  </li>
                </ul>
              </div>
            </div>
          </div>
        </div>
      </section>
      <!--End Product Categories One-->
      <!--Start product-grid-->
      <div class="product-grid pt-60 pb-120">
        <div class="container">
          <div class="row gx-4">
            <div class="col-xl-3 col-lg-4">
              <div class="shop-grid-sidebar">
                <button class="remove-sidebar d-lg-none d-block"><i
                  class="flaticon-cross"> </i></button>
                <div class="sidebar-holder">
                  <form action="#0" class="footer-default__subscrib-form m-0 p-0 wow fadeInUp animated">
                    <div class="footer-input-box p-0 "><input type="email" placeholder="Email address"
                                                              name="email">
                      <button type="submit" class="subscribe_btn"><i
                        class="flaticon-magnifying-glass"></i></button>
                    </div>
                  </form>
                  <div class="single-sidebar-box mt-30 wow fadeInUp animated ">
                    <h4>Select Categories</h4>
                    <div class="checkbox-item">
                      <form>
                        <div v-for="category in filterList.categories" class="form-group"><input :value="category.id"
                                                                                                 v-model="categories"
                                                                                                 type="checkbox"
                                                                                                 :id="category.id">
                          <label
                            :for="category.id">{{ category.title }}</label></div>
                      </form>
                    </div>
                  </div>
                  <div class="single-sidebar-box mt-30 wow fadeInUp animated">
                    <h4>Color Option </h4>
                    <ul class="color-option">
                      <li v-for="color in filterList.colors">
                        <a @click.prevent="addColor(color.id)" href="#0" class="color-option-single"
                           :style="`background: #${color.title}`"> <span>{{ color.title }}</span> </a>
                      </li>
                    </ul>
                  </div>
                  <div class="single-sidebar-box mt-30 wow fadeInUp animated">
                    <h4>Filter By Price</h4>
                    <div class="slider-box">
                      <div id="price-range" class="slider"></div>
                      <div class="output-price"><label for="priceRange">Price:</label> <input
                        type="text" id="priceRange" readonly></div>
                      <button @click.prevent="filterProducts" class="filterbtn"
                              type="submit"> Filter
                      </button>
                    </div>
                  </div>
                  <div class="single-sidebar-box mt-30 wow fadeInUp animated pb-0 border-bottom-0 ">
                    <h4>Tags </h4>
                    <ul class="popular-tag">
                      <li v-for="tag in filterList.tags">
                        <a @click.prevent="addTag(tag.id)" href="#0">{{ tag.title }}</a>
                      </li>
                    </ul>
                  </div>
                </div>
              </div>
            </div>
            <div class="col-xl-9 col-lg-8">
              <div class="row">
                <div class="col-xl-12">
                  <div
                    class="shop-grid-page-top-info p-0 justify-content-md-between justify-content-center">
                    <div class="left-box wow fadeInUp animated">
                      <p>Showing 1–12 of 50 Results</p>
                    </div>
                    <div
                      class="right-box justify-content-md-between justify-content-center wow fadeInUp animated">
                      <div class="short-by">
                        <div class="select-box">
                          <select v-model="order" @change="onOrderChange" class="wide">
                            <option value="" data-display="Short by latest">Featured</option>
                            <option value="ASC">Price, low to high</option>
                            <option value="DESC">Price, high to low</option>
                          </select>
                        </div>
                      </div>
                      <div class="product-view-style d-flex justify-content-md-between justify-content-center">
                        <ul class="nav nav-pills" id="pills-tab" role="tablist">
                          <li class="nav-item" role="presentation">
                            <button class="nav-link active" id="pills-grid-tab"
                                    data-bs-toggle="pill" data-bs-target="#pills-grid" type="button"
                                    role="tab" aria-selected="true">
                              <i class="flaticon-grid"></i>
                            </button>
                          </li>
                          <li class="nav-item" role="presentation">
                            <button class="nav-link" id="pills-list-tab" data-bs-toggle="pill"
                                    data-bs-target="#pills-list" type="button" role="tab"
                                    aria-selected="false">
                              <i class="flaticon-list"></i>
                            </button>
                          </li>
                        </ul>
                        <button class="slidebarfilter d-lg-none d-flex"><i class="flaticon-edit"></i></button>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <div class="row">
                <div class="col-12">
                  <div class="tab-content" id="pills-tabContent">
                    <div class="tab-pane fade show active" id="pills-grid" role="tabpanel"
                         aria-labelledby="pills-grid-tab">
                      <div class="row">
                        <div v-for="product in products" class="col-xl-4 col-lg-6 col-6 ">
                          <div class="products-three-single w-100  mt-30">
                            <div class="products-three-single-img"><a
                              href="shop-details-3.html" class="d-block"> <img
                              :src="product.image_url"
                              class="first-img" alt="" /> <img
                              src="/src/assets/images/home-three/productss2-hover-1.png"
                              alt="" class="hover-img" />
                            </a>
                              <div class="products-grid-one__badge-box"> <span
                                class="bg_base badge new ">New</span>
                              </div>
                              <a href="cart.html" class="addcart btn--primary style2">
                                Add To Cart </a>
                              <div class="products-grid__usefull-links">
                                <ul>
                                  <li><a href="wishlist.html"> <i class="flaticon-heart">
                                  </i> <span>
                                                                            wishlist</span> </a></li>
                                  <li><a href="compare.html"> <i
                                    class="flaticon-left-and-right-arrows"></i>
                                    <span>
                                                                            compare</span> </a></li>
                                  <li><a @click.prevent="getProduct(product.id)" :href="`#popup${product.id}`"
                                         class="popup_link"> <i
                                    class="flaticon-visibility"></i>
                                    <span> quick view</span>
                                  </a></li>
                                </ul>
                              </div>
                            </div>
                            <div :id="`popup${product.id}`" class="product-gird__quick-view-popup mfp-hide">
                              <div v-if="popupProduct" class="container">
                                <div class="row justify-content-between align-items-center">
                                  <div class="col-lg-6">
                                    <div class="quick-view__left-content" v-if="popupProduct.product_images.length > 2">
                                      <div class="tabs">
                                        <div class="popup-product-thumb-box">
                                          <ul>
                                            <li v-for="productImage in popupProduct.product_images"
                                                class="tab-nav popup-product-thumb">
                                              <a :href="`#tabb${productImage.id}`">
                                                <img :src="productImage.url"
                                                     alt="" /> </a></li>
                                          </ul>
                                        </div>
                                        <div class="popup-product-main-image-box">
                                          <div v-for="productImage in popupProduct.product_images"
                                               :id="`tabb${productImage.id}`"
                                               class="tab-item popup-product-image">
                                            <div
                                              class="popup-product-single-image">
                                              <img :src="productImage.url"
                                                   alt="" /></div>
                                          </div>
                                          <button class="prev"><i
                                            class="flaticon-back"></i>
                                          </button>
                                          <button class="next"><i
                                            class="flaticon-next"></i>
                                          </button>
                                        </div>
                                      </div>
                                    </div>
                                  </div>
                                  <div class="col-lg-6">
                                    <div class="popup-right-content">
                                      <h3>{{ popupProduct.title }}</h3>
                                      <div class="ratting"><i
                                        class="flaticon-star"></i> <i
                                        class="flaticon-star"></i> <i
                                        class="flaticon-star"></i>
                                        <i class="flaticon-star"></i> <i
                                          class="flaticon-star"></i>
                                        <span>({{ popupProduct.count }})</span></div>
                                      <p class="text"> {{ popupProduct.description }}
                                      </p>
                                      <div class="price">
                                        <h2> ${{ popupProduct.price }} USD
                                          <del> ${{ popupProduct.past_price }} USD</del>
                                        </h2>
                                        <h6> In stuck</h6>
                                      </div>
                                      <div class="color-varient">
                                        <template v-for="groupProduct in popupProduct.group_products">
                                          <a @click.prevent="getProduct(groupProduct.id)"
                                             v-for="color in groupProduct.colors" href="#0"
                                             :style="`background: #${color.title};`"
                                             class="color-name"><span>{{ color.title }}</span></a>
                                        </template>
                                      </div>
                                      <div class="add-product">
                                        <h6>Qty:</h6>
                                        <div class="button-group">
                                          <div class="qtySelector text-center">
                                                                                    <span class="decreaseQty"><i
                                                                                      class="flaticon-minus"></i>
                                                                                    </span> <input type="number"
                                                                                                   class="qtyValue"
                                                                                                   value="1" />
                                            <span class="increaseQty"> <i
                                              class="flaticon-plus"></i>
                                                                                    </span></div>
                                          <button class="btn--primary "> Add to
                                            Cart
                                          </button>
                                        </div>
                                      </div>
                                      <div class="payment-method"><a href="#0"> <img
                                        src="/src/assets/images/payment_method/method_1.png"
                                        alt=""> </a>
                                        <a href="#0"> <img
                                          src="/src/assets/images/payment_method/method_2.png"
                                          alt=""> </a> <a href="#0"> <img
                                          src="/src/assets/images/payment_method/method_3.png"
                                          alt=""> </a>
                                        <a href="#0"> <img
                                          src="/src/assets/images/payment_method/method_4.png"
                                          alt=""> </a></div>
                                    </div>
                                  </div>
                                </div>
                              </div>
                            </div>
                            <div class="products-three-single-content text-center"><span>{{
                                product.category.title
                              }}</span>
                              <h5>
                                <router-link :to="{name: 'products.show', params: { id: product.id }}"> {{ product.title }} </router-link>
                              </h5>
                              <p>
                                <del>${{ product.past_price }}.00</del>
                                ${{ product.price }}.00
                              </p>
                            </div>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <div v-if="pagination.last_page > 1" class="row">
                <div class="col-12 d-flex justify-content-center wow fadeInUp animated">
                  <ul class="pagination text-center">
                    <!-- Кнопка "Назад" -->
                    <li v-if="pagination.current_page !== 1" class="next">
                      <a @click.prevent="getProducts(pagination.current_page - 1)" href="#0">
                        <i class="flaticon-left-arrows" aria-hidden="true"></i>
                      </a>
                    </li>

                    <!-- Ссылки на страницы -->
                    <li v-for="link in pagination.links" :key="link.label">
                      <!-- Отображаем числовые ссылки (1,2,3...N), которые должны быть видны -->
                      <template
                        v-if="
        Number(link.label) &&
        (
          (pagination.current_page - link.label < 2 && pagination.current_page + link.label > -2)
          || Number(link.label) === 1
          || Number(link.label) === pagination.last_page
        )
      "
                      >
                        <a
                          @click.prevent="getProducts(link.label)"
                          :class="link.active ? 'active' : ''"
                          href="#0"
                        >
                          {{ link.label }}
                        </a>
                      </template>

                      <!-- Отображаем троеточия, когда между страницами появляется разрыв -->
                      <template
                        v-else-if="
        Number(link.label) &&
        (
          (pagination.current_page !== 3 && (pagination.current_page - link.label === 2))
          ||
          (pagination.current_page !== pagination.last_page - 2 && (pagination.current_page - link.label === -2))
        )
      "
                      >
                        <a>...</a>
                      </template>
                    </li>

                    <!-- Кнопка "Вперёд" -->
                    <li v-if="pagination.current_page !== pagination.last_page" class="next">
                      <a @click.prevent="getProducts(pagination.current_page + 1)" href="#0">
                        <i class="flaticon-next-1" aria-hidden="true"></i>
                      </a>
                    </li>
                  </ul>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!--End product-grid-->
    </main>
  </div>
</template>

<style scoped>

</style>
