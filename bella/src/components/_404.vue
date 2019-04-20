<template>
    <div>
        <section class="wrapper" >
            <div class="section section-breadcrumb">
                <div class="container">
                    <div class="row">
                        <div class="col-xs-12">
                            <div class="breadcrumb-inner">
                                <div class="page-title">
                                    <strong>Không tìm thấy dữ liệu</strong>
                                </div>
                                <ol class="breadcrumb">
                                    <li>
                                        <a href="/">Trang chủ</a>
                                    </li>
                                    <li class="active">404</li>
                                </ol>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <section class="section section-404">
                <div class="container">
                    <div class="row">
                        <div class="col-xs-12">
                            <img :src="theme_uri + 'static/images/logo/404.png'" alt="">
                            <h4>Xin lỗi, trang bạn đang tìm kiếm không tồn tại, đã bị xóa hoặc đổi tên</h4>
                            <p>
                                Bạn có thể quay lại
                                <a href="/">Trang chủ</a> hoặc sử dụng tìm kiếm
                            </p>
                        </div>
                    </div>
                </div>
            </section>
        </section>
    </div>
</template>
<script>
    export default {
        data(){
            return {
                title : '',
            }
        },
        head: {
            title: function () {
                return {
                    inner:this.title,
                    separator: '|',
                    complement:'404'
                }
            }
        },
        computed:{
            title1(){
                return (  this.$store.state &&this.$store.state.hasOwnProperty('default_data') && this.$store.state.default_data.default_meta.data ) ? this.$store.state.default_data.default_meta.data.meta_title : '';
            },
        },
        mounted(){
            this.title = this.title1;
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
