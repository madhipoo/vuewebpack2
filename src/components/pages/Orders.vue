<template>
    <div>
        <loading :active.sync="isLoading"/>
        <table class="table mt-4">
            <thead>
                <tr>
                    <th>購買時間</th>
                    <th>Email</th>
                    <th>購買款項</th>
                    <th>應付金額</th>
                    <th>是否付款</th>
                </tr>
            </thead>
            <tbody>
<<<<<<< HEAD
                <tr v-for="(item) in orders" :key="item.id">
                    <td>{{item.paid_data}}</td>
                    <td>{{item.user.email}}</td>
                    <td>{{item.products.product}}</td>
=======
                <tr v-for="(item,key) in orders" :key="key" v-if="orders.length">
                    <td>{{item.create_at | date}}</td>
                    <td><span v-text="item.user.email" v-if="item.user"></span></td>
                    <td>
                        <ul class="list-unstyled">
                            <li v-for="(product, i) in item.products" :key="i">
                                {{ product.product.title }} 數量：{{ product.qty }}
                                {{ product.product.unit }}
                            </li>
                        </ul>
                    </td>
>>>>>>> master
                    <td class="text-right">{{item.total | currency}}</td>
                    <td>
                        <span v-if="item.is_paid" class="text-success">啟用</span>
                        <span v-else>未啟用</span>
                    </td>
                </tr>
            </tbody>
        </table>

        <Pagination :pages="pagination" @emitPages="getOrders"></Pagination>
    </div>
</template>
<script>
import Pagination from '../Pagination'

export default {
    data(){
        return{
            orders:[],
            pagination: {},
            isLoading:false,
        }           
    },
    components: {
    Pagination,
    },
    methods: {
        getOrders(page = 1){
            const api = `${process.env.APIPATH}/api/${process.env.COSTOMPATH}/orders?page=${page}`;
            const vm = this;
            // console.log(process.env.APIPATH, process.env.COSTOMPATH)
            vm.isLoading = true;
            this.$http.get(api).then((response) => {
                console.log(response);
                vm.isLoading = false;
                vm.orders = response.data.orders;
                vm.pagination = response.data.pagination;
            });
        },
    },
    // computed: {
    //     sortOrder() {
    //     const vm = this;
    //     let newOrder = [];
    //     if (vm.orders.length) {
    //         newOrder = vm.orders.sort((a, b) => {
    //         const aIsPaid = a.is_paid ? 1 : 0;
    //         const bIsPaid = b.is_paid ? 1 : 0;
    //         return bIsPaid - aIsPaid;
    //         });
    //     }
    //     return newOrder;
    //     },
    // },
    created() {
        this.getOrders();
    },
}
</script>