# vuePagination
用法 <vue-pagination @size-change='sizeChange' @current-change='currentChange' :totals='totals' :page-size='pageSize' :tab='tab' :pageSizeSettings='pageSizeSettings'></vue-pagination>

size-change 改变pageSize会将pageSize传出父组件监听重新获取数据


current-change 改变页码 传出页码currentPage 父组件监听重新获取数据


totals 异步请求的数据总条数


page-size 请求的每页数据条数


pageSizeSettings 可设置下拉 选择不同的pageSize 默认数组


tab 重置currentPage bool值 默认false 当点击不同筛选条件时 请将tab值取反


