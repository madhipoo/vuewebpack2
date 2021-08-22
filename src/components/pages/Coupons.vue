<template>
    <div>
        <loading :active.sync="isLoading"/>
        <div class ="text-right mt-4 ">
            <button class="btn btn-primary" @click="openModal(true)">建立新的產品</button>
        </div>
        <table class="table mt-4">
            <thead>
                <th width="120">名稱</th>
                <th >折扣百分比</th>
                <th width="120">到期日</th>
                <th width="100">是否啟用</th>
                <th width="120">編輯</th>

            </thead>
            <tbody>
                <tr v-for="(item) in coupons" :key="item.id">
                    <td>{{item.title}}</td>
                    <td>{{item.percent}}</td>
                    <td class="text-right">{{item.due_date}}</td>
                    <td>
                        <span v-if="item.is_enabled" class="text-success">啟用</span>
                        <span v-else>未啟用</span>
                    </td>
                    <td>
                        <button class="btn btn-outline-primary btn-sm"  @click="openModal(false,item)">編輯</button>
                    </td>
                </tr>
            </tbody>
        </table>


        <nav aria-label="Page navigation example">
        <ul class="pagination">
            <li class="page-item" :class="{'disabled':!pagination.has_pre}">
            <a class="page-link" href="#" aria-label="Previous" @click="getCoupons(pagination.current_page - 1)">
                <span aria-hidden="true">&laquo;</span>
            </a>
            </li>
            <li class="page-item"  v-for="page in pagination.total_pages" :key="page"
            :class="{'active':pagination.current_page === page}">
                <a class="page-link" href="#" @click="getCoupons(page)">{{page}}</a>
            </li>
            
            <li class="page-item" :class="{'disabled':!pagination.has_next}">
            <a class="page-link" href="#" aria-label="Next" @click="getCoupons(pagination.current_page + 1)">
                <span aria-hidden="true">&raquo;</span>
            </a>
            </li>
        </ul>
        </nav>



        <div class="modal fade" id="productModal" tabindex="-1" role="dialog"
        aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog modal-lg" role="document">
                <div class="modal-content border-0">
                    <div class="modal-header bg-dark text-white">
                        <h5 class="modal-title" id="exampleModalLabel">
                        <span>Modal title</span>
                        </h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <div class="modal-body">
                        <div class="row">
                        <div class="col-sm-12">
                            <div class="form-group">
                                <label for="title">標題</label>
                                <input type="text" class="form-control" id="title"
                                    placeholder="請輸入標題" v-model="tempCoupon.title">

                                <label for="code" class="my-3">優惠碼</label>
                                <input type="text" class="form-control" id="code"
                                placeholder="請輸入優惠碼" v-model="tempCoupon.code">

                                <label for="due_date" class="my-3">到期日</label>
                                <input type="date" class="form-control" id="due_date"
                                placeholder="請輸入" v-model="tempCoupon.due_date">

                                <label for="percent" class="my-3">折扣百分比</label>
                                <input type="number" class="form-control" id="percent"
                                placeholder="請輸入百分比(數字)" v-model="tempCoupon.percent">
                            </div>

                            <div class="form-group">
                                <div class="form-check">
                                    <input class="form-check-input" type="checkbox"
                                    id="is_enabled" v-model="tempCoupon.is_enabled"
                                    :true-value="1" :false-value="0">
                                    <label class="form-check-label" for="is_enabled">
                                    是否啟用
                                    </label>
                                </div>
                            </div>
                        </div>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-outline-secondary" data-dismiss="modal">取消</button>
                        <button type="button" class="btn btn-primary" @click ="updateCoupon()">確認</button>
                    </div>
                </div>
            </div>
        </div>

    </div>
</template>
<script>
import $ from 'jquery'
export default {
    data(){
        return{
            coupons:[],
            pagination:{},
            tempCoupon:{},
            isNew:false,
            isLoading:false,
            status:{
                fileUploading:false,
            },
        }
    },
    methods:{
        getCoupons(page = 1){
            const api = `${process.env.APIPATH}/api/${process.env.COSTOMPATH}/admin/coupons?page=${page}`;
            const vm = this;
            //API 伺服器路徑
            // 所申請的APIPath
            // console.log(process.env.APIPATH, process.env.COSTOMPATH)
            vm.isLoading = true;
            this.$http.get(api).then((response) => {
                console.log(response.data);
                vm.isLoading = false;
                vm.coupons = response.data.coupons;
                vm.pagination = response.data.pagination;
            });
        },
        
        openModal(isNew,item){
            
            if(isNew){
                this.tempCoupon = {};
                this.isNew = true;
            }else{
                this.tempCoupon = Object.assign({},item);
                this.isNew = false;
            }
            $('#productModal').modal('show');
        },
        updateCoupon(){
            let api = `${process.env.APIPATH}/api/${process.env.COSTOMPATH}/admin/coupon`;
            const vm = this;
            
            this.$http.post(api ,{data: vm.tempCoupon}).then((response) => {
                console.log(response.data);
                if(response.data.success){
                    $('#productModal').modal('hide');
                    vm.getCoupons();
                }else{
                    $('#productModal').modal('hide');
                    vm.getCoupons();
                    console.log('新增失敗');
                }
                
            });
        },
    },
    created() {
        this.getCoupons(); 
    },
}
</script>