<template>
    <div>
        <section class=" section section-hot-product" id="hot-product">
            <div class="container">
                <div class="row">
                    <div class="col-xs-12">
                        <div class="title" v-if="source">
                            <h3>{{source.title}}</h3>
                        </div>
                    </div>
                </div>
                <div class="list-hot-product" v-if="source && source.data && source.allow_show">
                    <div class="row row-flex" itemscope itemtype="http://schema.org/ItemList">
                        <span itemprop="numberOfItems" hidden>{{ source.data.length }}</span>
                        <div class="col-xs-6 col-sm-4 col-md-3 col-flex" itemprop="itemListElement" itemscope
                            v-for="(item, index) in source.data"
                            :key="index">
                            <div class="item-product-thumb wow delay-5s fadeInLeft">
                                <div class="product-inner">
                                    <!-- bienthe -->
                                    <!-- sp don -->
                                    <div v-if="item.product_type_id == '5a052e34659d5e8445714bbd'">
                                        <div v-if="item.manage_inventory == false"></div>
                                        <div v-else>
                                            <div v-if="item.available_quantity && item.available_quantity > 0"></div>
                                            <div class="ribbon-two ribbon-two-primary" v-else>
                                                <span>Hết hàng</span>
                                            </div>
                                        </div>
                                    </div>
                                    <!-- bien nhom -->
                                    <div v-if="item.product_type_id == '5a0d7270cba70e92b80f55db'">
                                        <div v-if="item.manage_inventory == false"></div>
                                        <div v-else>
                                            <div v-if="item.children.available_quantity && item.children.available_quantity > 0"></div>
                                            <div class="ribbon-two ribbon-two-primary" v-else>
                                                <span>Hết hàng</span>
                                            </div>
                                        </div>
                                    </div>
                                    <!-- combo -->
                                    <div v-if="item.product_type_id == '5a0d7282cba70e92b80f55dd'">
                                        <div v-if="item.manage_inventory == false"></div>
                                        <div v-else>
                                            <div v-if="item.children.available_quantity && item.children.available_quantity > 0"></div>
                                            <div class="ribbon-two ribbon-two-primary" v-else>
                                                <span>Hết hàng</span>
                                            </div>
                                        </div>
                                    </div>
                                    <div class="product-image">
                                        <router-link :to="alias(item.Alias)">
                                            <img v-if="item.Image && item.Image.path"
                                                :src="$store.state.domain + '/' + item.Image.path"
                                                :alt="item.name" >
                                            <img v-else :src="theme_uri + 'static/images/default/img_article.jpg'" />
                                        </router-link>
                                        <div class="product-btn-groups">
                                            <button @click.stop.prevent="$store.commit('showQuickView', item)" class="btn-add-item-to-cart"></button>
                                            <button @click.stop.prevent="$store.commit('toggleFavorite', item)"
                                                class="">
                                                <i style="line-height: 50px;"
                                                    :class="{ 'text-danger ' : inFavorite($store,item._id) }" class="fa fa-heart">
                                                </i>
                                            </button>
                                        </div>
                                    </div>
                                    <div class="product-content">
                                        <router-link itemprop="url" :to="alias(item.Alias)"
                                                        :title="item.name" class="product-title">
                                            <span itemprop="name" style="font-weight: 600;">{{item.name}}</span>
                                        </router-link>
                                        <p v-line-clamp:20="2" style="font-size: 14px;">{{item.description}}</p>
                                        <div class="product-price" itemprop="offers" itemscope itemtype="http://schema.org/Offer">
                                            <span itemprop="priceCurrency" content="VND"></span>
                                            <span itemprop="price" :content="item.price_sale_web"
                                                class="product-price-new">
                                                    <span v-if="item.price_promotion_web&&item.price_promotion_web >0">{{item.price_promotion_web | money}} đ</span>
                                                    <span v-else>{{getWebPrice(item)  | money}} đ</span>
                                            </span>
                                            <span v-if="item.price_promotion_web&&item.price_promotion_web >0" :itemprop="item.price_promotion_web" :content="item.price_promotion_web" class="product-price-old">
                                                {{getWebPrice(item)  | money}} đ
                                            </span>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>
    </div>
</template>
<style>
    .product-inner {
        position: relative;
    }
    .product-inner .ribbon-two {
        position: absolute;
        left: -5px;
        top: -5px;
        z-index: 1;
        overflow: hidden;
        width: 75px;
        height: 75px;
        text-align: right;
    }
    .product-inner .ribbon-two-primary span {
        background: #f96a74;
    }
    .product-inner .ribbon-two span:before {
        content: "";
        position: absolute;
        left: 0;
        top: 100%;
        z-index: -1;
        border-right: 3px solid transparent;
        border-bottom: 3px solid transparent;
        border-left: 3px solid #f96a74;
        border-top: 3px solid #f96a74;
    }
    .product-inner .ribbon-two span:after {
        content: "";
        position: absolute;
        right: 0;
        top: 100%;
        z-index: -1;
        border-left: 3px solid transparent;
        border-bottom: 3px solid transparent;
        border-right: 3px solid #f96a74;
        border-top: 3px solid #f96a74;
    }
    .product-inner .ribbon-two span {
        font-size: 13px;
        color: #ffffff;
        font-family: 'Nunito Sans', sans-serif;
        text-align: center;
        line-height: 20px;
        transform: rotate(-45deg);
        -webkit-transform: rotate(-45deg);
        width: 100px;
        display: block;
        box-shadow: 0 0 8px 0 rgba(0, 0, 0, 0.06), 0 1px 0 0 rgba(0, 0, 0, 0.02);
        position: absolute;
        top: 19px;
        left: -21px;
    }
</style>

<script>
export default {
    props:['source'],
}
</script>

