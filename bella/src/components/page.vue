<!-- Cấu trúc trang mặc định -->
<template>
    <section class="section section-posts" style="padding: 10px 0;">      
        <div class="container">
            <div class="wrapper" style="padding-bottom:0;">
                <div class="row">
                    <div class="col-xs-12">
                        <div class="rules"  v-html="source.alias_object.content"></div>
                    </div>
                </div>
            </div>
        </div>
    </section>
      
</template>
<style>
    table {
        width: 100%;
        display: table;
        overflow-x: auto;
    }
    .header-top .btn-groups > ul > li .dropdown-menu {
        right: 0;
        left: auto;
    }
    .order-table .table thead {
        background: #888;
        color: #fff;
    }
    .order-table .table thead tr th {
        border-bottom: 0px solid #ddd;
    }
    .order-table h3 {
        font-size: 20px;
        font-weight: 600;
        color: #555;
    }
</style>

<script>
export default {
    props:['source']
}
</script>
