<template>
    <div>
        <section class="section section-categories " id="tab-list-product" >
            <div class="container">
                <div class="row ">
                    <div class="col-xs-12 col-sm-3 ">
                        <div class="list-home-banners" v-if="banner && banner.allow_show == true">
                            <a :href="item.info.link" target="_blank" v-for="(item, index) in banner.data" :key="index" :title="item.info.title">
                                <img :src="$store.state.domain + '/' + item.path"
                                    :item="item.info.title" :alt="item.info.title" class="img-responsive">
                            </a>
                        </div>
                    </div>
                    <div class="col-xs-12 col-sm-12 " v-if="source && banner.data == 0 || banner.allow_show == false">
                        <template v-if="source.allow_show == true">
                            <ul class="nav nav-pills">
                                <li role="presentation" :class="{active : index == 0 }" v-for="(item, index) in source.data.tab_data" :key="index">
                                    <a :href="'#tab-new-product-' + index" role="tab" data-toggle="tab">{{item.name}}</a>
                                </li>
                            </ul>
                            <div class="tab-content">
                                <div v-if="source" v-for="(tab, tab_index) in source.data.tab_data" :key="tab_index" 
                                    :class="{active : tab_index == 0}"
                                    class="tab-pane fade in "  :id="'tab-new-product-' + tab_index">
                                    <div class="list-tab-product" v-if="tab.data">
                                        <div class="row row-flex" itemscope itemtype="http://schema.org/ItemList">
                                            <span itemprop="numberOfItems" hidden>{{tab.data.length}}</span>
                                            <div class="col-xs-6 col-sm-6 col-md-3 col-flex" itemprop="itemListElement" v-for="(item, index) in tab.data" :key="index"
                                                itemscope itemtype="http://schema.org/Product">
                                                <div class="item-product-thumb wow delay-5s fadeInRight">
                                                    <div class="product-inner">
                                                        <!-- bienthe -->
                                                        <!-- sp don -->
                                                        <div v-if="item.product_type_id == '5a052e34659d5e8445714bbd'">
                                                            <div v-if="item.available_quantity && item.available_quantity > 0"></div>
                                                            <div class="ribbon-two ribbon-two-primary" v-else>
                                                                <span>Hết hàng</span>
                                                            </div>
                                                        </div>
                                                        <!-- bien nhom -->
                                                        <div v-if="item.product_type_id == '5a0d7270cba70e92b80f55db'">
                                                            <div v-if="item.children.available_quantity && item.children.available_quantity > 0"></div>
                                                            <div class="ribbon-two ribbon-two-primary" v-else>
                                                                <span>Hết hàng</span>
                                                            </div>
                                                        </div>
                                                        <!-- combo -->
                                                        <div v-if="item.product_type_id == '5a0d7282cba70e92b80f55dd'">
                                                            <div v-if="item.children.available_quantity && item.children.available_quantity > 0"></div>
                                                            <div class="ribbon-two ribbon-two-primary" v-else>
                                                                <span>Hết hàng</span>
                                                            </div>
                                                        </div>
                                                        <router-link class="product-image" itemprop="url" :to="alias(item.Alias)">
                                                            <img v-if="item.Image && item.Image.path" :src="$store.state.domain + '/' + item.Image.path"
                                                                itemprop="image" :alt="item.name" :title="item.name">
                                                            <img v-else :src="theme_uri + 'static/images/default/img_article.jpg'" />
                                                            <div class="product-btn-groups">
                                                                <button @click.stop.prevent="$store.commit('showQuickView', item)" class="btn-add-item-to-cart"></button>

                                                                <button  @click.stop.prevent="$store.commit('toggleFavorite', item)"
                                                                    class="">
                                                                    <i style="line-height: 50px;"
                                                                        :class="{ 'text-danger ' : inFavorite($store,item._id) }" class="fa fa-heart">
                                                                    </i>
                                                                </button>
                                                            </div>
                                                        </router-link>
                                                        <div class="product-content">
                                                            <router-link itemprop="url" :to="alias(item.Alias)"
                                                                        :title="item.name" class="product-title">
                                                                <span itemprop="name" style="font-weight: 600;">{{item.name}}</span>
                                                            </router-link>
                                                            <p v-line-clamp:20="2" style="font-size: 14px;">{{item.description}}</p>
                                                            <div class="product-price" itemprop="offers" itemscope=""
                                                                itemtype="http://schema.org/Offer">
                                                                <span itemprop="priceCurrency" content="VND"></span>
                                                                <span itemprop="price" :content="item.price_sale_web"
                                                                    class="product-price-new">
                                                                    <span v-if="item.price_promotion_web&&item.price_promotion_web >0">{{item.price_promotion_web | money}} đ</span>
                                                                    <span v-else>{{ getWebPrice(item)  | money}} đ</span>
                                                                </span>
                                                                <span v-if="item.price_promotion_web&&item.price_promotion_web >0" :itemprop="item.price_promotion_web" :content="item.price_promotion_web" class="product-price-old">
                                                                    {{ getWebPrice(item)  | money}} đ
                                                                </span>
                                                            </div>
                                                        </div>
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </template>
                    </div>
                    <div class="col-xs-12 col-sm-9 " v-else>
                        <template v-if="source.allow_show == true">
                            <ul class="nav nav-pills">
                                <li role="presentation" :class="{active : index == 0 }" v-for="(item, index) in source.data.tab_data" :key="index">
                                    <a :href="'#tab-new-product-' + index" role="tab" data-toggle="tab">{{item.name}}</a>
                                </li>
                            </ul>
                            <div class="tab-content">
                                <div v-if="source" v-for="(tab, tab_index) in source.data.tab_data" :key="tab_index" 
                                    :class="{active : tab_index == 0}"
                                    class="tab-pane fade in "  :id="'tab-new-product-' + tab_index">
                                    <div class="list-tab-product" v-if="tab.data">
                                        <div class="row row-flex" itemscope itemtype="http://schema.org/ItemList">
                                            <span itemprop="numberOfItems" hidden>{{tab.data.length}}</span>
                                            <div class="col-xs-6 col-sm-6 col-md-4 col-flex" itemprop="itemListElement" v-for="(item, index) in tab.data" :key="index"
                                                itemscope itemtype="http://schema.org/Product">
                                                <div class="item-product-thumb wow delay-5s fadeInRight">
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
                                                        <router-link class="product-image" itemprop="url" :to="alias(item.Alias)">
                                                            <img :src="$store.state.domain + '/' + item.Image.path"
                                                                itemprop="image" :alt="item.name" :title="item.name">
                                                            <div class="product-btn-groups">
                                                                <button @click.stop.prevent="$store.commit('showQuickView', item)" class="btn-add-item-to-cart"></button>

                                                                <button  @click.stop.prevent="$store.commit('toggleFavorite', item)"
                                                                    class="">
                                                                    <i style="line-height: 50px;"
                                                                        :class="{ 'text-danger ' : inFavorite($store,item._id) }" class="fa fa-heart">
                                                                    </i>
                                                                </button>
                                                            </div>
                                                        </router-link>
                                                        <div class="product-content">
                                                            <router-link itemprop="url" :to="alias(item.Alias)"
                                                                        :title="item.name" class="product-title">
                                                                <span itemprop="name" style="font-weight: 600;">{{item.name}}</span>
                                                            </router-link>
                                                            <p v-line-clamp:20="2" style="font-size: 14px;">{{item.description}}</p>
                                                            <div class="product-price" itemprop="offers" itemscope=""
                                                                itemtype="http://schema.org/Offer">
                                                                <span itemprop="priceCurrency" content="VND"></span>
                                                                <span itemprop="price" :content="item.price_sale_web"
                                                                    class="product-price-new">
                                                                    <span v-if="item.price_promotion_web&&item.price_promotion_web >0">{{item.price_promotion_web | money}} đ</span>
                                                                    <span v-else>{{ getWebPrice(item)  | money}} đ</span>
                                                                </span>
                                                                <span v-if="item.price_promotion_web&&item.price_promotion_web >0" :itemprop="item.price_promotion_web" :content="item.price_promotion_web" class="product-price-old">
                                                                    {{ getWebPrice(item)  | money}} đ
                                                                </span>
                                                            </div>
                                                        </div>
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </template>
                    </div>
                </div>
            </div>
        </section>
    </div>
</template>
<script>
export default {
    props:['source','banner'],
}
</script>

