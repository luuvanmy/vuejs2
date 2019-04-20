<template>
    <div>
        <section class="wrapper" style="min-height: 356px;">
            <div class="section section-breadcrumb" style="
                background-image:url('upload/bella.iamsale.vn/slider/images/2018-07/8020_1530526598.jpg')">
                <div class="container">
                    <div class="row">
                        <div class="col-xs-12">
                            <div class="breadcrumb-inner">
                                <div class="page-title">
                                    <strong>Giỏ hàng</strong>
                                </div>
                                <ol class="breadcrumb">
                                    <li>
                                        <a href="/">Trang chủ</a>
                                    </li>
                                    <li class="active">Giỏ hàng</li>
                                </ol>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <section id="cart-page" class="section section-cart">
                <div class="container">
                    <div class="row">
                        <div class="col-xs-12" v-if=" carts &&  carts.length > 0">
                            <table class="table table-cart">
                                <thead>
                                    <tr>
                                        <th class="td-name">TÊN SẢN PHẨM</th>
                                        <th class="td-price text-center">GIÁ TIỀN</th>
                                        <th class="td-quantity text-center">SỐ LƯỢNG</th>
                                        <th class="td-total text-center">THÀNH TIỀN</th>
                                        <th class="td-action"></th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <template>
                                        <tr data-index="0" class="item" v-for="(item, index) in carts" :key="index">
                                            <td data-show="true" data-title="Sản phẩm :" class="td-name">
                                                <div class="td-name-inner">
                                                    <div class="td-name-inner-image">
                                                        <img v-if="item.Image" :src="$store.state.domain +'/'+item.Image.path" alt="">
                                                    </div>
                                                    <!-- <div class="product-content">
                                                        <div class="product-name" v-if="item.VariantName && item.VariantName!=''">
                                                            <router-link :to="alias(item.Alias)">{{ item.VariantName }}</router-link> 
                                                        </div>
                                                        <div class="product-name" v-else>
                                                            <router-link :to="alias(item.Alias)">{{ item.name }}</router-link>
                                                        </div>
                                                    </div> -->
                                                    <div class="td-name-inner-name" v-if="item.VariantName && item.VariantName!=''">
                                                        <router-link :to="alias(item.Alias)" style="color:#555;">
                                                            {{ item.VariantName }}
                                                        </router-link> 
                                                    </div>
                                                    <div class="td-name-inner-name" v-else>
                                                        <router-link :to="alias(item.Alias)" style="color:#555;">
                                                            {{ item.name }}
                                                        </router-link>
                                                    </div>
                                                </div>
                                            </td>
                                            <td data-show="true" data-title="Giá : " class="td-price"> 
                                                <template v-if="item.price_promotion_web && isPrice(item.price_promotion_web)">
                                                    <span>{{ item.price_promotion_web | money }} <small>đ</small></span>
                                                    <span style="text-decoration: line-through;">{{ item.price_sale_web | money }} <small>đ</small></span>
                                                </template>
                                                <span v-else>{{ item.price_sale_web | money }} <small>đ</small></span>
                                            </td>
                                            <td data-show="true" data-title="Số lượng : " class="td-quantity">
                                                <div class="input-group">
                                                    <div class="input-group input-group-spinner" data-type="input-spinner"
                                                        data-target="#btn_add_to_cart">
                                                        <span class="input-group-btn">
                                                            <button @click.stop.prevent="incrementQuantity(item, false, saveCart)" class="btn btn-default" type="button" 
                                                                    data-type="input-spinner-btn"
                                                                    data-input-type="false">
                                                                <i class="fas fa-minus"></i>
                                                            </button>
                                                        </span>
                                                        <input type="number" class="form-control" v-model="item.quantity"
                                                            data-type="input-spinner-field" readonly="">
                                                        <span class="input-group-btn">
                                                            <button @click.stop.prevent="incrementQuantity(item, true, saveCart)" class="btn btn-default" type="button" data-type="input-spinner-btn"
                                                                    data-input-type="true">
                                                                <i class="fas fa-plus"></i>
                                                            </button>
                                                        </span>
                                                    </div>
                                                </div>
                                            </td>
                                            <td data-show="true" data-title="Thành tiền : " class="td-total">{{getWebPirce(item) * item.quantity | money }} <small>đ</small></td>
                                            <td data-show="true" data-title="" class="td-action"><a @click.stop.prevent="removeFromCart( item)" href="#"></a></td>
                                        </tr>
                                    </template>
                                </tbody>
                                <tfoot>
                                    <tr>
                                        <th colspan="5">
                                            <div class="text-right">
                                                <h4><strong>Tổng tiền : </strong> <strong>{{this.total() |money}}</strong></h4>
                                            </div>
                                        </th>
                                    </tr>
                                </tfoot>
                            </table>
                        </div>
                        <div class="col-md-6 col-md-offset-3 col-xs-12" v-if="! carts || ! carts.length">
                            <div class="empty-cart">
                                <img :src="theme_uri+'static/images/icons/empty-cart.png'" alt="gio-hang" class="img-responsive"> 
                                <h3>Chưa có sản phẩm nào được yêu thích !</h3> 
                                <a href="/">Quay lại trang chủ</a>
                            </div>
                        </div>
                        <div class="col-xs-12" v-if=" carts && carts.length > 0">
                            <div class="cart-buttons">
                                <a href="/" class="btn btn-default btn-black mg-right-15">
                                    <i class="fa fa-arrow-left mg-right-10"></i>TIẾP TỤC MUA SẮM
                                </a> 
                                <a href="/checkout" class="btn btn-submit">THANH TOÁN
                                    <i class="fa fa-arrow-right mg-left-10"></i>
                                </a>
                            </div>
                        </div>
                    </div>
                </div>
            </section>
        </section>
    </div>
</template>
<script>
    var timeout = null
    export default{
        data(){
            return{
                title : '',
                current_url:''
            }
        },
        head: {
            title: function () {
                return {
                    inner:this.title,
                    separator: '|',
                    complement : 'Giỏ hàng'
                }
            }
        },
        methods : {
            saveCart( ){
                var current_cart = this.$store.state.carts ? this.$store.state.carts : {}
                localStorage.setItem('carts', JSON.stringify(current_cart)); 
                clearTimeout(timeout)
                timeout = setTimeout(()=>{
                    this.showNotification('Cập nhật giỏ hàng thành công!!', 'shopping_cart', 'success' )
                }, 500)
            },
            removeFromCart(item){
                this.$store.commit('removeFromCart', item)
    
            },
            total(){
                var total = 0
                if( this.$store.state.carts ){
                    Object.values(this.$store.state.carts).forEach((item)=>{
                        if(item.price_promotion_web > 0){
                            total += item.quantity * item.price_promotion_web;
                        }else{
                            total += item.quantity * item.price_sale_web;
                        }
                    });
                }
                return total 
            }
        },
        mounted(){
            this.title = this.title1;
        },
        created(){
            this.current_url = window.location.href;
        },
        computed:{
            title1(){
                return (  this.$store.state &&this.$store.state.hasOwnProperty('default_data') && this.$store.state.default_data.default_meta.data ) ? this.$store.state.default_data.default_meta.data.meta_title : '';
            },
            carts(){
                return Object.values(this.$store.state.carts)
            }
        },
        watch:{
           '$store.state.default_data.default_meta':function(new_val){
                this.title = new_val.data.meta_title;
                if(new_val.data.meta_title != ''){
                    this.$emit('updateHead')
                        this.$forceUpdate();
                }
                
            },
            'title':function(){
                this.$emit('updateHead')
            }
        }
    }
</script>