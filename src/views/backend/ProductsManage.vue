<template>
  <div>
    <!-- <loading :active.sync="isLoading"/>    讀取中的程式碼 -->
    <div class="text-right mt-4">
      <button class="btn btn-primary"
        @click="openModal(true)">
        <i class="fas fa-plus me-2"></i>
        建立新的產品
      </button>
    </div>
    <table class="table mt-4">
      <thead>
        <tr>
          <th width="120">分類</th>
          <th>產品名稱</th>
          <th width="120">原價</th>
          <th width="120">售價</th>
          <th width="100">是否啟用</th>
          <th width="160">編輯</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item) in products" :key="item.id">
          <td>{{ item.category }}</td>
          <td>{{ item.title }}</td>
          <td class="text-right">
            {{ currencyFilter(item.origin_price) }}
          </td>
          <td v-if="!item.price" class="text-right">
            {{ currencyFilter(item.origin_price) }}
          </td>
          <td v-else class="text-right">
            {{ currencyFilter(item.price) }}
          </td>
          <td>
            <span v-if="item.is_enabled" class="text-success">
              啟用
            </span>
            <span v-else>
              未啟用
            </span>
          </td>
          <td>
            <button class="btn btn-outline-primary btn-sm"
              @click="openModal(false, item)">
              編輯
            </button>
            <button class="btn btn-outline-danger btn-sm"
              @click="deleteModal(item)">
              刪除
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <!-- pagination -->
    <Pagination :pagination='pagination' @emitProductPage="getProducts" />
    <!-- Product-modal -->
    <div class="modal fade" id="productModal" tabindex="-1" role="dialog"
    aria-hidden="true">
      <div class="modal-dialog modal-lg" role="document">
        <div class="modal-content border-0">
          <div class="modal-header bg-dark text-white">
            <h5 class="modal-title" id="exampleModalLabel">
              <span>新增產品</span>
            </h5>
            <button type="button" class="close" data-bs-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <div class="row">
              <div class="col-sm-4">
                <div class="form-group">
                  <label for="image">輸入圖片網址</label>
                  <input type="text" class="form-control" id="image"
                    v-model="tempProduct.imageUrl"
                    placeholder="請輸入圖片連結">
                </div>
                <div class="form-group">
                  <label for="customFile">或 上傳圖片
                    <i class="fas fa-spinner fa-spin" v-if="status.fileUploading"></i>
                  </label>
                  <input type="file" id="customFile" class="form-control"
                    ref="files" @change="uploadFile">
                </div>
                <img class="img-fluid" :src="tempProduct.imageUrl" alt="上傳圖片">
              </div>
              <div class="col-sm-8">
                <div class="form-group">
                  <label for="title">標題</label>
                  <input type="text" class="form-control" id="title"
                    v-model="tempProduct.title"
                    placeholder="請輸入標題">
                </div>
                <div class="form-row">
                  <div class="form-group col-md-6">
                    <label for="category">分類</label>
                    <input type="text" class="form-control" id="category"
                      v-model="tempProduct.category"
                      placeholder="請輸入分類">
                  </div>
                  <div class="form-group col-md-6">
                    <label for="price">單位</label>
                    <input type="unit" class="form-control" id="unit"
                      v-model="tempProduct.unit"
                      placeholder="請輸入單位">
                  </div>
                </div>

                <div class="form-row">
                  <div class="form-group col-md-6">
                  <label for="origin_price">原價</label>
                    <input type="number" class="form-control" id="origin_price"
                      v-model="tempProduct.origin_price"
                      placeholder="請輸入原價">
                  </div>
                  <div class="form-group col-md-6">
                    <label for="price">售價</label>
                    <input type="number" class="form-control" id="price"
                      v-model="tempProduct.price"
                      placeholder="請輸入售價">
                  </div>
                </div>
                <hr>

                <div class="form-group">
                  <label for="description">產品描述</label>
                  <textarea type="text" class="form-control" id="description"
                    v-model="tempProduct.description"
                    placeholder="請輸入產品描述"></textarea>
                </div>
                <div class="form-group">
                  <label for="content">說明內容</label>
                  <textarea type="text" class="form-control" id="content"
                    v-model="tempProduct.content"
                    placeholder="請輸入產品說明內容"></textarea>
                </div>
                <div class="form-group">
                  <div class="form-check">
                    <input class="form-check-input" type="checkbox"
                      v-model="tempProduct.is_enabled"
                      :true-value="1"
                      :false-value="0"
                      id="is_enabled">
                    <label class="form-check-label" for="is_enabled">
                      是否啟用
                    </label>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-outline-secondary" data-bs-dismiss="modal">取消</button>
            <button type="button" class="btn btn-primary" @click="updateProduct">確認</button>
          </div>
        </div>
      </div>
    </div>
    <div class="modal fade" id="delProductModal" tabindex="-1" role="dialog"
      aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog" role="document">
        <div class="modal-content border-0">
          <div class="modal-header bg-danger text-white">
            <h5 class="modal-title" id="exampleModalLabel">
              <span>刪除產品</span>
            </h5>
            <button type="button" class="close" data-bs-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            是否刪除 <strong class="text-danger">{{ tempProduct.title }}</strong> 商品
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-outline-secondary" data-bs-dismiss="modal">取消</button>
            <button type="button" class="btn btn-danger"
              @click="deleteProduct"
              >確認刪除</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// import $ from 'jquery'
import { Modal } from 'bootstrap'
import Pagination from '@/components/Pagination'

export default {
  components: {
    Pagination
  },
  data () {
    return {
      products: [],
      tempProduct: {},
      pagination: {},
      isNew: false,
      isLoading: false,
      status: {
        fileUploading: false
      },
      productModal: null,
      delProductModal: null
    }
  },
  methods: {
    getProducts (page = 1) {
      const api = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/products?page=${page}`
      const vm = this
      vm.isLoading = true
      vm.$http.get(api).then((response) => {
        // console.log(response.data);
        vm.isLoading = false
        vm.products = response.data.products
        vm.pagination = response.data.pagination
      })
    },
    openModal (isNew, item) {
      if (isNew) {
        this.tempProduct = {}
        this.isNew = true
      } else {
        this.tempProduct = Object.assign({}, item)
        this.isNew = false
      }
      // $('#productModal').modal('show')
      // console.log(new bootstrap)
      this.productModal.show()
    },
    deleteModal (item) {
      this.tempProduct = item
      // $('#delProductModal').modal('show')
      this.delProductModal.show()
    },
    deleteProduct () {
      const vm = this
      const api = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/admin/product/${vm.tempProduct.id}`
      vm.$http.delete(api).then(response => {
        console.log(response.data)
        if (response.data.success) {
          vm.getProducts()
        } else {
          vm.getProducts()
          console.log('刪除失敗！')
        }
        this.delProductModal.hide()
      })
    },
    updateProduct () {
      let api = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/admin/product`
      let httpMethod = 'post'
      const vm = this
      if (!vm.isNew) {
        api = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/admin/product/${vm.tempProduct.id}`
        httpMethod = 'put'
      }
      vm.$http[httpMethod](api, { data: vm.tempProduct }).then((response) => {
        // console.log(response.data);
        if (response.data.success) {
          // $('#productModal').modal('hide')
          vm.getProducts()
        } else {
          // $('#productModal').modal('hide')
          vm.getProducts()
          console.log('新增失敗！')
        }
        this.productModal.hide()
      })
    },
    uploadFile () {
      // console.log(this);
      const uploadedFile = this.$refs.files.files[0]
      const vm = this
      const forData = new FormData()
      forData.append('file-to-upload', uploadedFile)
      const url = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/admin/upload`
      vm.status.fileUploading = true
      vm.$http.post(url, forData, {
        headers: {
          'Content-Type': 'multipart/form-data'
        }
      }).then((response) => {
        // console.log(response.data);
        vm.status.fileUploading = false
        if (response.data.success) {
          // vm.tempProduct.imageUrl = response.data.imageUrl;
          // console.log(vm.tempProduct);
          // 使用 set 強制寫入才能雙向綁定。
          vm.$set(vm.tempProduct, 'imageUrl', response.data.imageUrl)
        } else {
          this.$bus.$emit('message:push', response.data.message, 'danger')
        };
      })
    },
    currencyFilter (num) {
      const n = Number(num)
      return `$${n.toFixed(0).replace(/./g, (c, i, a) => {
        const currency = (i && c !== '.' && ((a.length - i) % 3 === 0) ? `, ${c}`.replace(/\s/g, '') : c)
        return currency
      })}`
    }
  },
  created () {
    this.getProducts()
  },
  mounted () {
    this.productModal = new Modal(document.getElementById('productModal'))
    this.delProductModal = new Modal(document.getElementById('delProductModal'))
  }
}
</script>
