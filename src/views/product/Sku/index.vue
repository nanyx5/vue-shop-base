<template>
  <div>
    <el-table border="" :data="records">
      <el-table-column label="序号" width="80" align="center" type="index" />
      <el-table-column label="名称" prop="skuName" />
      <el-table-column label="描述" prop="skuDesc" />
      <el-table-column label="默认图片" width="110">
        <template slot-scope="{row,$index}">
          <img :src="row.skuDefaultImg" style="height:80px;width:80px"></img>
        </template>
      </el-table-column>
      <el-table-column label="重量（kg）" width="80" prop="weight" />
      <el-table-column label="价格（元）" prop="price" />
      <el-table-column label="操作">
        <template slot-scope="{row,$index}">
          <el-button v-if="row.isSale == 0" type="success" icon="el-icon-sort-down" size="mini" @click="sale(row)" />
          <el-button v-else type="success" icon="el-icon-sort-up" size="mini" @click="cancel(row)" />
          <el-button type="primary" icon="el-icon-edit" size="mini" @click="edit(row)" />
          <el-button type="info" icon="el-icon-info" size="mini" @click="getSkuInfo(row)" />
          <el-button type="danger" icon="el-icon-delete" size="mini" />

        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      style="textAlign:center"
      :total="total"
      :current-page="page"
      :page-size="limit"
      :page-sizes="[1,5,10]"
      layout="prev,pager,next,jumper,->,sizes,total"
      @current-change="getSkuList"
      @size-change="handleSizeChange"
    />

    <el-drawer
      :visible.sync="show"
      :show-close="false"
      size="50%"
    >
      <el-row>
        <el-col :span="5">名称</el-col>
        <el-col :span="16">{{ skuInfo.skuName }}</el-col>
      </el-row>
      <el-row>
        <el-col :span="5">描述</el-col>
        <el-col :span="16">{{ skuInfo.skuDesc }}</el-col>
      </el-row>  <el-row>
        <el-col :span="5">价格</el-col>
        <el-col :span="16">{{ skuInfo.price }}</el-col>
      </el-row>  <el-row>
        <el-col :span="5">平台属性</el-col>
        <el-col :span="16">
          <template>
            <el-tag v-for="(attr,index) in skuInfo.skuAttrValueList" :key="index" style="marginRight:10px" type="success">{{ attr.attrId }}-{{ attr.valueId }}</el-tag>
          </template>
        </el-col>
      </el-row>  <el-row>
        <el-col :span="5">商品图片</el-col>
        <el-col :span="16">
          <el-carousel>
            <el-carousel-item v-for="item in skuInfo.skuImageList" :key="item.id">
              <img :src="item.imgUrl"></img>
            </el-carousel-item>
          </el-carousel>
        </el-col>
      </el-row>
    </el-drawer>

  </div>
</template>

<script>
export default {
  data() {
    return {
      page: 1,
      limit: 10,
      total: 0,
      records: [],
      skuInfo: {},
      show: false
    }
  },
  mounted() {
    this.getSkuList()
  },
  methods: {
    edit() {
      this.$message('正在开发中')
    },
    async sale(row) {
      row.isSale = 1
      const result = await this.$API.sku.reqSale(row.id)
      if (result.code == 200) {
        this.$message(
          {
            type: 'success',
            message: '上架成功'
          }
        )
      }
    },
    async cancel(row) {
      row.isSale = 0
      const result = await this.$API.sku.reqCancel(row.id)
      if (result.code == 200) {
        this.$message(
          {
            type: 'success',
            message: '下架成功'
          }
        )
      }
    },
    async getSkuList(pages = 1) {
      this.page = pages
      const { page, limit } = this
      const result = await this.$API.sku.reqSkuList(page, limit)
      if (result.code == 200) {
        this.total = result.data.total
        this.records = result.data.records
      }
    },
    handleSizeChange(limit) {
      this.limit = limit
      this.getSkuList()
    },
    async getSkuInfo(sku) {
      this.show = true
      const result = await this.$API.sku.reqSkuById(sku.id)
      if (result.code == 200) {
        this.skuInfo = result.data
      }
    }

  }

}
</script>

<style lang="scss" scoped>
.el-row {
  .el-col-5 {
    font-size: 18px;
    text-align: right;
  }
  .el-col {
    margin: 10px;
  }
}
</style>

<style>
  .el-carousel__item h3 {
    color: #475669;
    font-size: 14px;
    opacity: 0.75;
    line-height: 150px;
    margin: 0;
  }

  .el-carousel__item:nth-child(2n) {
     background-color: #99a9bf;
  }

  .el-carousel__item:nth-child(2n+1) {
     background-color: #d3dce6;
  }

.el-carousel__button{
  width: 10px;
  height: 10px;
  background: red;
  border-radius: 50%;
}

</style>
