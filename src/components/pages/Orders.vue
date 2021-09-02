<template>
    <div>
        <loading :active.sync="isLoading"/>
        <table class="table mt-4">
            <thead>
                <th width="120">購買時間</th>
                <th>e-mail</th>
                <th width="220">購買款項</th>
                <th width="220">應付金額</th>
                <th width="120">是否付款</th>

            </thead>
            <tbody>
                <tr v-for="(item) in orders" :key="item.id">
                    <td>{{item.paid_data}}</td>
                    <td>{{item.user.email}}</td>
                    <td>{{item.products.product}}</td>
                    <td class="text-right">{{item.total | currency}}</td>
                    <td>
                        <span v-if="item.is_paid" class="text-success">啟用</span>
                        <span v-else>未啟用</span>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>
<script>
export default {
    data(){
        return{
            orders:[],
            // tempProduct:{},
            isLoading:false,
        }           
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
                // vm.pagination = response.data.pagination;
            });
        },
    },
    created() {
        this.getOrders();
    },
}
</script>