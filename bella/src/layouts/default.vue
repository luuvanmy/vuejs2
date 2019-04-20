<template>
  <div id="default-layout" class="main">
    <div id="loading-box" class="text-center">
        <img :src="theme_uri + 'static/images/grid/load3.gif'" />
    </div>
    <div class="modal-banner" v-if="bannerAll && bannerAll.allow_show == true">
        <div class="modal-campaign">
            <div class="popup-campaign sale-off">
                <div class="popup-content">
                    <div class="popup-header">
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">×</span>
                        </button>
                    </div>
                    <div class="popup-body text-center col-xs-12">
                        <a :href="bannerAll.info.link" target="_blank" class="">
                            <img class="img-responsive" :src="$store.state.domain + '/' + bannerAll.data.path" alt="bannerAll.info.title" width="100%">
                        </a>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- <div class="banner-all ">
        <div  class="banner-load col-xs-6" v-if="bannerAll && bannerAll.allow_show == true">
            <img class="img-responsive" :src="$store.state.domain + '/' + bannerAll.data.path" alt="">
            <a href=""><i class="fas fa-close"></i></a>
        </div>
    </div> -->
    <div id="nav"></div>
    <app-header :source="$store.state" />
    <router-view />
    <app-footer />
    <product-modal :source="$store.state.current_product" />
  </div>
</template>
<style  scope>
    #loading-box{
        z-index: 100000000000;
        background: #fff;
        position: fixed;
        height: 100%;
        width: 100%;
        top: 50%;
        left: 50%;
       transform: translate(-50% , -50%);
        transition: all .4s ease-out;
        opacity: 1;
        height: 100%;
    }
    #loading-box img{
        max-width: 200px;
        /* height: auto;
        margin: 120px auto; */
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%)
    }
    #loading-box.close{
        opacity: 0;
        display: none;
    }
    .banner-all {
        position: fixed;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        background: #55555536;
        z-index: 1000000;
    }
    .banner-all .banner-load {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%)
    }
    /* .banner-load {
        position: fixed;
        height: 100%;
        width: 100%;
        top: 0;
        left: 0;
        bottom: 0;
        right: 0;
        z-index: 100000;
        background: #fff;
        opacity: 1;
        transition: all 0.5s ease;
    }
    .banner-load img {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%)
    }
    .banner-load.close {
        opacity: 0;
        overflow: hidden;
        visibility: hidden;
        transition: all 0.5s ease;
        transition-delay: 0.1s;
    } */
    .modal-banner {
        position: fixed;
        top: 0;
        right: 0;
        left: 0;
        bottom: 0;
        background: #55555547;
        height: 100%;
        z-index: 10000000;
        overflow: hidden;
    }
    @media(max-width: 992px){
        .modal-campaign {
            width: 100%;
        }
    }
    .modal-campaign {
        position: fixed;
        /* display: none; */
        top: 50%;
        left: 50%;
        -webkit-transform: translate(-50%, -50%);
        transform: translate(-50%, -50%);
        z-index: 1050;
    }
    .modal-campaign .popup-campaign {
        position: relative;
        display: block;
        /* width: 800px; */
        /* max-width: 100%; */
        background: #fff;
        border: 5px solid #ffc1c1;
        border-radius: 15px;
    }
    .modal-campaign .sale-off {
        background: transparent;
        border: none;
    }
    .modal-campaign .popup-campaign .popup-content .close {
        position: absolute;
        right: -2px;
        top: -16px;
        font-size: 32px;
        background: #fff;
        border-radius: 50%;
        width: 32px;
        height: 32px;
        z-index: 11;
        color: #e41b23;
        opacity: 1;
    }
    .modal-campaign .popup-campaign .popup-content .popup-body {
        display: -webkit-box;
        display: -ms-flexbox;
        display: flex;
        height: 100%;
        -ms-flex-wrap: wrap;
        flex-wrap: wrap;
    }
    .modal-campaign .popup-campaign .popup-content .popup-body a {
        background: #fff;
        padding: 10px;
        border-radius: 5px;
    }
</style>
<script>
  import AppHeader from '@/layouts/partials/header'
  import AppFooter from '@/layouts/partials/footer'
  import ProductModal from '@/components/product/product-modal'
  export default {
    name : 'layoutDefault',
    components : {
      AppHeader,
      AppFooter,
      ProductModal
    },
    data(){
        return {
            // domain : '',
            path1:''
        }
    },
    // beforeCreate() {
    //       this.$store.state.carts = this.getLocalStore('carts')
    // },
    created() {
        var user = this.getSession('user', null)
        var user_token = this.getLocalStore('user_token', null, false);
        if( !user ){
            if( user_token ){
                var headers = {
                    Authorization : this.$store.state.user_token,
                    Domain : this.$store.state.domain
                }
                this.callAPI( 'user/view-profile','post', {},headers)
                .then((res)=>{
                    if(res.data.success){
                        sessionStorage.setItem('user', JSON.stringify(res.data.data))
                        this.$store.state.user = this.getSession('user', null)
                    }
                })
            }
        }else{
            if(!user_token){
                sessionStorage.removeItem('user')
                this.$store.state.user = null
            }
        }
        this.$store.dispatch('initClientData')
        this.$store.dispatch('loadDefaultResources')
    },
    computed:{
        bannerAll() {
            return this.$store.state.default_data ? this.$store.state.default_data.banner_all : null;
        },
    },
    mounted(){
        this.$store.state.loading_alias = true
        // setTimeout(() => {
        //     var html = document.getElementsByTagName("html")[0].innerHTML
        //     this.$store.dispatch('updateView', {
        //         content : html,
        //         route : this.$route.params.alias
        //     }) 
        // }, 500)
        this.$store.state.open_footer_cart = false;
        $('html,body').click(function(){
            $('.modal-banner').addClass('hidden')
        })
        $('.close').click(function(){
            $('.modal-banner').addClass('hidden')
        })
    },
    watch:{
        '$store.state.default_data.favicon' : function(new_val){
            if(new_val.data && new_val.data.path != ''){
                if(new_val.data && new_val.data.path !== '' ){
                    this.changeFavicon(this.domain + '/' + new_val.data.path )
                }
            }
        },
         '$store.state.default_data.theme_color' : function(new_val){
             if( typeof new_val !== "undefined" && new_val ){
                 var style_uri = $('body').data('theme-uri') + 'static/css/style-' +  new_val.data +'.css';
                $('#theme_color').attr('href', style_uri)
             }
          
        },
        '$route.path' : function(){
            this.closeMobileMenu();
            $("html, body").scrollTop(0);
            this.$store.commit('checkLogin')
            if( this.$route.params.alias && this.$route.params.alias != '' ){
                this.$store.dispatch('loadPageResource', this.$route.params.alias)
            }
            this.$store.state.open_footer_cart = false
            if( ! this.$route.params.alias ){
                $('#loading-box').removeClass('close');
                $('#banner-load').removeClass('close')
            }
            setTimeout(()=>{$('#loading-box').addClass('close');}, 800);
            setTimeout(()=>{$('#banner-load').addClass('close');}, 1200)
            if(this.$route && this.$route.params && this.$route.params.alias =='search'){
                this.$store.state.search = true
            }
              if(!this.$route.query.hasOwnProperty('keyword_search')){
                  this.$store.state.search = ''
                  this.$store.state.search_type = ''
            } 
            // setTimeout(() => {
            //     var html = document.getElementsByTagName("html")[0].innerHTML
            //     this.$store.dispatch('updateView', {
            //         content : html,
            //         route : this.$route.params.alias
            //     })
            // }, 500)
        },
        '$store.state.default_data': function(){
            clearTimeout(this.$store.state.timeout)
            $('#loading-box').removeClass('close')
            $('#banner-load').removeClass('close')
            this.$store.state.timeout = setTimeout(()=>{$('#loading-box').addClass('close')}, 800)
            this.$store.state.timeout = setTimeout(()=>{$('#banner-load').addClass('close')}, 1200)
        }
    }
  }
</script>
